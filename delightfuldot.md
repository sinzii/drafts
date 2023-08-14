# DelightfulDOT

- **Team Name:** Coong Crafts (formerly Coong Team)
- **Payment Address:** 
- **[Level](https://github.com/w3f/Grants-Program/tree/master#level_slider-levels):** 3

## Project Overview :page_facing_up:

### 1. Overview
Dapps have always been a very important part of any blockchain ecosystem, it is where users can connect and interact with blockchain nodes. Given the complex nature of interacting with Substrate-based blockchains, in order for developers can focus on the business logic, a middle layer between dapps and blockchain nodes to facilitate the connections & interactions is always an essential part and this is where `@polkadot/api` comes in.

`@polkadot/api` has done a great job in helping applications connect to networks in an easy and effortless way by abstracting away all the complexities of connecting with a Substrate-based blockchain and scale-codec serialisation process under the hood. But through development experience, benchmarking and profiling, we found out that `@polkadot/api` has a relatively high memory consumption, which might not be problematic for dapps that only connect to one or a few networks, but for dapps that need to connect to dozens or even hundreds of networks at the same time, it’s a problem which might create a great impact on the overall user experience (e.g: a wallet app or portfolio app needs to connect to hundreds of networks to fetch users’ balances & assets or to listen to on-chain events).

- If we enable all 100+ Substrate networks on SubWallet, it could increase the memory consumption to over a GB of RAM.
<img width="680" alt="image" src="https://github.com/sinzii/w3-grant-draft/assets/6867026/fb9f909d-f466-467e-8a1b-6959e1464c2a">

- Talisman is having their own solution for connecting to networks and fetching balances effectively without relying on `@polkadot/api` ([@talismn/balances](https://github.com/TalismanSociety/talisman/tree/dev/packages/balances)**,** [@talismn/api](https://github.com/TalismanSociety/api))
- We ran [a NodeJS script](https://github.com/sinzii/delightfuldot-poc/blob/main/src/benchmarks/benchmark_connect_multiple_endpoints.ts) that connects to 100 substrate-based network endpoints to fetch balances for an account, the overall memory consumption is around 830MB. More details about the benchmark could be found [here](https://github.com/sinzii/delightfuldot-poc/tree/main#memory-consumption-benchmark-result).

As we’re heading toward a multi-chain future, there will absolutely be more parachains, parathreads or solochains built on Substrate to come, and users might have assets spreading over dozens or hundreds of networks. With that, we do see the need of connecting to a large number of networks at the same time effectively and efficiently, Coong Crafts propose to build `delightfuldot`, an alternative solution to `@polkadot/api` to address this issue in contributing to the whole effort of paving the way to a multi-chain future of the Polkadot & Kusama ecosystem.

### 2. Project Details

**2.1. Why does `@polkadot/api` has a high memory consumption?**

We ran memory profiling for a [NodeJS script](https://github.com/sinzii/delightfuldot-poc/blob/main/src/profiles/profile_connect_to_polkadot_via_polkadotapi.ts) to connect to Polkadot network to see how much memory `@polkadot/api` consume during the bootstrapping process (initialization). Below are captures of the results:
- Result of `Allocation Sampling` profiling via Google Dev Tools
<img width="680" alt="image" src="https://github.com/sinzii/w3-grant-draft/assets/6867026/24adc5f5-6394-4705-8586-d487dc1d5f35">

- Result of `Allocation instrumentation on timeline` profiling via Google Dev Tools
<img width="680" alt="image" src="https://github.com/sinzii/w3-grant-draft/assets/6867026/822b9096-1688-4d6f-a36a-d9a54331fb83">

From the results, we can see that the memory consumption from `Metadata` and its types system are relatively high. As we looked into the source code itself, we found out that `@polkadot/api` has its own types and structure for every piece in the metadata, during the decoding process it will create types for all of the pieces in the metadata hierarchy/structure which result in the lot of `Type` objects and a big `Metadata` object ([`PortableRegistry` is a part of the Metadata](https://github.com/polkadot-js/api/blob/319535a1e938e89522ff18ef2d1cef66a5af597c/packages/types/src/interfaces/metadata/v14.ts#L43-L47))

We tried to build a small [proof of concept alternative solution]([url](https://github.com/sinzii/delightfuldot-poc/blob/main/src/poc/delighfuldot.ts)) using [`scale-ts`](https://github.com/paritytech/scale-ts) (now `subShape`) for scale-codec encoding to do the same functionality and the memory consumption has improved noticeably.
<img width="680" alt="image" src="https://github.com/sinzii/w3-grant-draft/assets/6867026/71374ff9-db78-43ce-aef6-b26e44747f22">

Going further, instead of connecting to 1 network, this time we tried to connect to 20, 50, and 100 network endpoints to fetch balances for an account using `@polkadot/api` and our PoC solution for comparison, and as we can see from the result table, the memory consumption of our PoC solution is significantly smaller.
- This is the script used for benchmarking: [src/benchmarks/benchmark_connect_multiple_endpoints.ts](https://github.com/sinzii/delightfuldot-poc/blob/main/src/benchmarks/benchmark_connect_multiple_endpoints.ts)

**2.2. Design Goals & Approach**

_**2.2.a. API style similar to `@polkadot/api`**_

`@polkadot/api` is currently using an easy to use and intuitive API style (e.g: `api.query.balances.account(address)` to query account balance, or `api.consts.balances.[constant_name]` to access constants of pallet `Balances`).

So we decided to use the same API style so that users don’t have to learn new syntax when switching to use `delightfuldot` and making the migration progress easier.

_**2.2.b. Less overhead evaluation**_

During the bootstrapping process, `@polkadot/api` will try to register all possible type definitions (ref1, ref2) and expose all available methods/props after decoding the metadata retrieved from a network (ref). This would help making the API execution faster but at the same time making the bootstrapping process longer and increase the overall memory-consumption. Secondly, most of the dapps only makes use of a few APIs and types (e.g: …), the registration all of APIs and types would be unnecessary in most of the cases.

`delighfuldot`'s approach is to perform API evaluation and execution on the fly, which is at the time an API is called, `delighfuldot` will check if the API is existed or not, register all necessary types (and cache those types if possible), execute the API and then handle the response.

For example, upon calling `api.query.balances.account('5xxxxx...')` to fetching balances for an account, `delightfuldot` will do the following steps:
 - Check if the pallet named `Balances` is existed in the metadata, else throw an error.
 - Check if the storage entry named `Account` is existed in the metadata, else throw an error.
 - Gather all of the necessary information to perform a storage query through an RPC `state_getStorage` like input types, output type, calculate storage entry hash …
 - Execute RPC `state_getStorage` with the calculated storage entry hash
 - Decode the response with output type and return the decoded data.

Unlike `@polkadot/api` where the first 2 steps are already done in the bootstrapping process. We believe that our approach would help speed up the bootstrapping process, and reduce the overhead memory consumption. We archived this by using a [proxy technique](https://github.com/sinzii/delightfuldot-poc/blob/c52b8fd1dcd1ee82869db9ef7d63366e3307977c/src/poc/delighfuldot.ts#L82-L92), you could find more in detail about it in the PoC repository.

_**2.2.c. Caching**_

Metadata has always been an important part of any Substrate-based blockchain, it’s where we can find all information about on-chain functionalities (pallets, storage, extrinsics, constants, …), but it takes time to encode the metadata retrieved from networks and take space in the memory to store all of the information after decoding the metadata.

Since Metadata is only updated through runtime upgrade, so `delighfuldot` will cache the decoded metadata information in the device’s storage (localStorage, IndexedDB, …) and only check to update it on runtime upgrade. This would also help speed up the bootstrapping process and reduce the memory consumption since the metadata is now stored on the device’s storage.

One drawback of this approach is that access speed to storage would be a bit slower than to memory, but given the benefits of the approach, we believe the tradeoffs are acceptable.

**2.3. Vision**

We set a vision for `delightfuldot` to become an essential part of Polkadot & Kusama ecosystem, so dapps can leverage on its utilities to connect to and interact with hundreds of networks quickly and smoothly without having to think about which network to toggle on or off.

This proposal is asking for a grant to support the first development phase of `delighfuldot` for the foundational modules with core functionalities and compatibility layer with `@polkadot/api`. More details are in the upcoming section.

**2.4. What to build**

_**2.4.a. Foundational modules with core functionalities**_

This step, we aim to lay out all the necessary foundational pieces of the library and putting all of them together to form the core functionalities, including:

- New type system built on top of `subShape` with less memory consumption while at the same time can easily switch to use `@polkadot/api`'s type system for easy migration from existing dapps.
- A metadata parser with ability to decode & encode Metadata using scale-codec. In the scope of this grant, we plan to support the latest version of metadata which is v14.
- Ability to execute RPC to a substrate node
    - Each blockchain has its own custom list of supported RPCs, in the scope of this grant we plan to implement the supported RPCs of [Polkadot](https://polkadot.js.org/docs/polkadot/rpc) & [Kusama](https://polkadot.js.org/docs/kusama/rpc) networks.
    - Support registering custom RPCs, so developers can easily add custom RPCs for their custom substrate node.
- Ability to execute Runtime APIs
    - Similar to RPCs, each substrate-based blockchain has its own list of supported Runtime APIs, so in the scope of this grant, we plan to implement the supported Runtime APIs of [Polkadot](https://polkadot.js.org/docs/polkadot/runtime) and [Kusama](https://polkadot.js.org/docs/kusama/runtime) networks.
    - Support registering custom Runtime APIs
- With the format of Metadata V14, on-chain functionalities are exposed through pallet’s definitions including pallet’s constants, storage entries, extrinsic calls, events & errors. We plan to support abilities to:
    - Inspect pallet’s constants
    - Execute pallet’s storage queries
    - Sign and submit extrinsics
    - Inspect pallet’s events & errors

_**2.4.b. Compatibility layer with `@polkadot/api`**_

This is a layer built on top of the foundational modules to allows `delightfuldot` to easily switch between `@polkadot/api`'s type system and its own type system.

With the similar API style and ability to use the `@polkadot/api`'s type system, this would help the migration process from `@polkadot/api` to `delightfuldot` easily and smoothly and open the access to the already built custom types for parachains & other custom substrate-based chains and all the helpful derived APIs from `api-derive` package.

**2.5. Tech Stacks**
- Typescript
- subShape (formerly: scale-ts), rxjs
- Helpful packages from `@polkadot/api`, `@polkadot/common`

### Ecosystem Fit
`delightfuldot` fits perfectly in the Polkadot & Kusama ecosystems as it provides a solution to a critical issue faced by dApps that need to connect to and interact with hundreds of networks efficiently & effectively. Any dApps that need to connect to a large number of networks can benefit from `delightfuldot`'s utilities (e.g wallet apps, portfolio apps)

We as the maintainer of [Coong Wallet](https://grants.web3.foundation/applications/coong_wallet) see that `delightfuldot` is a stepping stone to the next development phase of Coong Wallet with more & more useful features in which Coong Wallet would need to connect to a large number of Substrate-based networks to fetching information.

Aside from `@polkadot/api`, [`capi`](https://github.com/paritytech/capi) is another project to help craft interactions with Substrate-based blockchain, but at the time of writing this proposal, it’s going through a big restructuring, we’re not sure what would it look like until its shape be more concrete. Overall we don’t see any noticeable projects that are trying to solve the same problems as us.

## Team :busts_in_silhouette:

### Team members

- Thang X. Vu (Team Leader)
- Tung Vu

### Contact

- **Contact Name:** Thang X. Vu
- **Contact Email:** thang@coongcrafts.io

### Legal Structure

N/A yet

### Team's experience

We have more than 7 years of experience in software development for startups & enterprises. Seeing the potential of blockchain technologies, we have spent more than 1 year exposing to blockchains and Polakdot & Kusama ecosystem. We closely worked with SubWallet team in helping to review the source code to improve performance & stability of the wallet. Thang is a participant of the first Polkadot DevCamp in May 2022. We as users also experience the UX problems in Polkadot & Kusama ecosystem. With that, we know where and how to solve these paint points to help bring the ecosystem closer to end-users.

If anyone on your team has applied for a grant at the Web3 Foundation previously, please list the name of the project and legal entity here.
- Coong Wallet

### Team Code Repos

Project repositories will be hosted at https://github.com/CoongCrafts

Team members
- Thang X. Vu - https://github.com/sinzii
- Tung Vu - https://github.com/1cedrus

## Development Status :open_book:

- Research `@polkadot/api`
- Benchmarking & profiling
- Build proof of concept solution

## Development Roadmap :nut_and_bolt:

### Overview

- **Total Estimated Duration:** 4.5 months
- **Full-Time Equivalent (FTE):**  2 FTE
- **Total Costs:** 45,000 USD

### Milestone 1 — Foundational modules with core functionalities

- **Estimated duration:** 2.5 month
- **FTE:**  2
- **Costs:** 25,000 USD

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | Apache 2.0 / GPLv3 / MIT / Unlicense |
| **0b.** | Documentation | We will provide both **inline documentation** of the code and a basic **tutorial** that explains how a user can (for example) spin up one of our Substrate nodes and send test transactions, which will show how the new functionality works. |
| **0c.** | Testing and Testing Guide | Core functions will be fully covered by comprehensive unit tests to ensure functionality and robustness. In the guide, we will describe how to run these tests. |
| 1. | Core functionalities
| 2. | Publish to npm


### Milestone 2 - Core functionalities + @polkadot/api compatibility layer

- **Estimated Duration:** 2 month
- **FTE:**  2
- **Costs:** 20,000 USD

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | Apache 2.0 / GPLv3 / MIT / Unlicense |
| **0b.** | Documentation | We will provide both **inline documentation** of the code and a basic **tutorial** that explains how a user can (for example) spin up one of our Substrate nodes and send test transactions, which will show how the new functionality works. |
| **0c.** | Testing and Testing Guide | Core functions will be fully covered by comprehensive unit tests to ensure functionality and robustness. In the guide, we will describe how to run these tests. |
| 1. | More core functionalities
| 2. | `@polkadot/api` compatibility layer

## Future Plans

- Smart Contract
- XCM


## Additional Information :heavy_plus_sign:

**How did you hear about the Grants Program?** Web3 Foundation Website / Medium / Twitter / Element / Announcement by another team / personal recommendation / etc.

Here you can also add any additional information that you think is relevant to this application but isn't part of it already, such as:

- Work you have already done.
- If there are any other teams who have already contributed (financially) to the project.
- Previous grants you may have applied for.
