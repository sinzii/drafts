# SubProfile

- **Team Name:** SubProfile
- **Payment Address:** BTC, Ethereum (USDT/USDC/DAI) or Polkadot/Kusama (aUSD) payment address. Please also specify the currency. (e.g. 0x8920... (DAI))
- **[Level](https://github.com/w3f/Grants-Program/tree/master#level_slider-levels):** 2

## Project Overview :page_facing_up:

### Overview

- Wallet is a key factor to blockchain technology & cryptocurrencies, it should be secure, easy to use and for mass adoption it should have a frictionless users onboarding experience.
- Polkadot & Kusama ecosystem has seen a few wallet solutions out there with great UI/UX (SubWallet, Talisman, Enkrypt, Nova). On desktop, most of the solutions are browser extension-based wallet with which users need to install an extension in order to interact with dapps and networks. On mobile, most of the browsers do not support extensions, so users would need to install wallet mobile apps and then interact with dapp via Dapp Browser build inside the apps (SubWallet, Nova). We believe this creates an inconsistent experience for users on desktop & mobile since most of the dapps are website-based thus posing a barrier in onboarding new users to the ecosystem, especially for those who are new to or less-educated about crypto.
- As users, we love the website-based wallet experience that the NEAR wallet bring to the NEAR ecosystem where users can connect to dapps using their favorite browsers and access their wallet smoothly inside the same browser on both desktop & mobile.
- With that inspiration, we propose to build SubProfile, a website-based multi-chain wallet, to bring the similar experience to Polkadot & Kusama ecosystem with which we believe it will bring a huge benefits to both users & the ecosystem.

### Project Details

#### Design Goals
#### Account Creation
#### Dapp-Wallet Communication
#### Integration Process into Dapps
#### Vision
#### Mockups
#### Technology Stack

### Ecosystem Fit

SubProfile is born with the intention to help mitigate the inconsistent experience on desktop & mobile and bring a seamless onboarding new users experience to the Polkadot & Kusama ecosystem.

As discussed above, there’re a few wallet solutions out there in the ecosystem that have great UX/UI but most are extension-based wallet, SubProfile take a different approach as to be a website-based wallet.

Choko wallet is also an other website-based wallet but there’re a few differences:

- Choko wallet took the redirection-based approach like the near wallet did while SubProfile uses the `window.postMessage` API for cross-tab & cross-origin communication like how dapps interact with extension-based wallets.
- SubProfile is compatible with `@polkadot/extension` API which is widely adopted in the ecosystem, so dapps can integrate with SubProfile just like how they integrate with extension-based wallets.

## Team :busts_in_silhouette:

### Team members

- Thang X. Vu - Leader / Lead Developer
- Tung Vu - Frontend Developer

### Contact

- **Contact Name:** Thang X. Vu
- **Contact Email:** zthangxv@gmail.com

### Legal Structure

None

### Team's experience

We have more than 7 years of experience in software development for startups mostly in web-based solutions. Seeing the potential of blockchain technologies, we have spent more than 1 year exposing to blockchain and Polakdot & Kusama ecosystem. Thang is a participant of the first Polkadot DevCamp in May 2022 and also closely worked with SubWallet team in helping to review the source code to improve performance & stability of the wallet. We as users also experience the UX problems in Polkadot & Kusama ecosystem. With that, we know where and how to solve these paint points to help bring the ecosystem closer to end-users.

### Team Code Repos

Projects repos

- https://github.com/subprofile/wallet
- https://github.com/subprofile/sdk

Team members

- Thang X. Vu - https://github.com/sinzii
- Tung Vu - https://github.com/1cedrus

## Development Status :open_book:

If you've already started implementing your project or it is part of a larger repository, please provide a link and a description of the code here. In any case, please provide some documentation on the research and other work you have conducted before applying. This could be:

- links to improvement proposals or [RFPs](https://github.com/w3f/Grants-Program/tree/master/rfp-proposal) (requests for proposal),
- academic publications relevant to the problem,
- links to your research diary, blog posts, articles, forum discussions or open GitHub issues,
- references to conversations you might have had related to this project with anyone from the Web3 Foundation,
- previous interface iterations, such as mock-ups and wireframes.

## Development Roadmap :nut_and_bolt:

This section should break the development roadmap down into milestones and deliverables. To assist you in defining it, we have created a document with examples for some grant categories [here](../docs/grant_guidelines_per_category.md). Since these will be part of the agreement, it helps to describe _the functionality we should expect in as much detail as possible_, plus how we can verify and test that functionality. Whenever milestones are delivered, we refer to this document to ensure that everything has been delivered as expected.

Below we provide an **example roadmap**. In the descriptions, it should be clear how your project is related to Substrate, Kusama or Polkadot. We _recommend_ that teams structure their roadmap as 1 milestone ≈ 1 month.

> :exclamation: If any of your deliverables is based on somebody else's work, make sure you work and publish _under the terms of the license_ of the respective project and that you **highlight this fact in your milestone documentation** and in the source code if applicable! **Teams that submit others' work without attributing it will be immediately terminated.**

### Overview

- **Total Estimated Duration:** Duration of the whole project (e.g. 2 months)
- **Full-Time Equivalent (FTE):**  Average number of full-time employees working on the project throughout its duration (see [Wikipedia](https://en.wikipedia.org/wiki/Full-time_equivalent), e.g. 2 FTE)
- **Total Costs:** Requested amount in USD for the whole project (e.g. 12,000 USD). Note that the acceptance criteria and additional benefits vary depending on the [level](../README.md#level_slider-levels) of funding requested. This and the costs for each milestone need to be provided in USD; if the grant is paid out in Bitcoin, the amount will be calculated according to the exchange rate at the time of payment.

### Milestone 1 Example — Basic functionality

- **Estimated duration:** 1 month
- **FTE:**  1,5
- **Costs:** 8,000 USD

> :exclamation: **The default deliverables 0a-0d below are mandatory for all milestones**, and deliverable 0e at least for the last one. If you do not intend to deliver one of these, please state a reason in its specification (e.g. Milestone X is research oriented and as such there is no code to test).

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | Apache 2.0 / GPLv3 / MIT / Unlicense |
| **0b.** | Documentation | We will provide both **inline documentation** of the code and a basic **tutorial** that explains how a user can (for example) spin up one of our Substrate nodes and send test transactions, which will show how the new functionality works. |
| **0c.** | Testing and Testing Guide | Core functions will be fully covered by comprehensive unit tests to ensure functionality and robustness. In the guide, we will describe how to run these tests. |
| **0d.** | Docker | We will provide a Dockerfile(s) that can be used to test all the functionality delivered with this milestone. |
| 0e. | Article | We will publish an **article**/workshop that explains [...] (what was done/achieved as part of the grant). (Content, language and medium should reflect your target audience described above.) |
| 1. | Substrate module: X | We will create a Substrate module that will... (Please list the functionality that will be implemented for the first milestone. You can refer to details provided in previous sections.) |
| 2. | Substrate module: Y | The Y Substrate module will... |
| 3. | Substrate module: Z | The Z Substrate module will... |
| 4. | Substrate chain | Modules X, Y & Z of our custom chain will interact in such a way... (Please describe the deliverable here as detailed as possible) |
| 5. | Library: ABC | We will deliver a JS library that will implement the functionality described under "ABC Library" |
| 6. | Smart contracts: ... | We will deliver a set of ink! smart contracts that will...


### Milestone 2 Example — Additional features

- **Estimated Duration:** 1 month
- **FTE:**  1,5
- **Costs:** 8,000 USD

...


## Future Plans

Please include here

- how you intend to use, enhance, promote and support your project in the short term, and
- the team's long-term plans and intentions in relation to it.

## Referral Program (optional) :moneybag: 

You can find more information about the program [here](../README.md#moneybag-referral-program).
- **Referrer:** Name of the Polkadot Ambassador or GitHub account of the Web3 Foundation grantee
- **Payment Address:** BTC, Ethereum (USDT/USDC/DAI) or Polkadot/Kusama (aUSD) payment address. Please also specify the currency. (e.g. 0x8920... (DAI))

## Additional Information :heavy_plus_sign:

**How did you hear about the Grants Program?** Web3 Foundation Website / Medium / Twitter / Element / Announcement by another team / personal recommendation / etc.

Here you can also add any additional information that you think is relevant to this application but isn't part of it already, such as:

- Work you have already done.
- If there are any other teams who have already contributed (financially) to the project.
- Previous grants you may have applied for.
