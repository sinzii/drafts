# Milestone Delivery :mailbox:

> ⚡ Only the GitHub account that submitted the application is allowed to submit milestones. 
> 
> Don't remove any of the mandatory parts presented in bold letters or as headlines! Lines starting with `>`, such as this one, can be removed.

**The [invoice form :pencil:](https://docs.google.com/forms/d/e/1FAIpQLSfmNYaoCgrxyhzgoKQ0ynQvnNRoTmgApz9NrMp-hd8mhIiO0A/viewform) has been filled out correctly for this milestone and the delivery is according to the official [milestone delivery guidelines](https://github.com/w3f/Grants-Program/blob/master/docs/Support%20Docs/milestone-deliverables-guidelines.md).**  

* **Application Document:** [Coong Wallet](https://github.com/w3f/Grants-Program/blob/master/applications/coong_wallet.md)
* **Milestone Number:** 1

**Deliverables**
> Please provide a list of all deliverables of the milestone extracted from the initial application and a link to the deliverable itself. Ideally all links inside the below table should include a commit hash, which will be used for testing. If you don't provide a commit hash, we will work off the default branch of your repository. Thus, if you plan on continuing work after delivery, we suggest you create a separate branch for either the delivery or your continuing work. 
> 
> If there is anything particular about any of the deliverables we or a future reader should know, use the respective `Notes` column.

| Number | Deliverable | Link | Notes |
| -----: | ----------- | ------------- | ------------- |
| **0a.** | License | [Apache 2.0](https://github.com/CoongCrafts/coong-wallet/blob/w3f-milestone-1/LICENSE) |
| **0b.** | Documentation | - [README](https://github.com/CoongCrafts/coong-wallet/blob/w3f-milestone-1/README.md)<br/>- [Live Demo](https://app.coongwallet.io/)<br/>- [Sample Dapp](https://coong-demo-dapp.netlify.app) to demonstrate interactions<br/> - [Integration instruction](https://github.com/CoongCrafts/coong-wallet/blob/w3f-milestone-1/README.md#integrate-coong-wallet-into-your-dapps) | While trying out the example dapp, make sure to disable `Block third-party cookies` setting on your browser & allow open popup if being asked by the browser. Recommended using Google Chrome for best experience. More info can be found in the [known issue](https://github.com/CoongCrafts/coong-wallet/tree/w3f-milestone-1#known-issues) section. |
| **0c.** | Testing and Testing Guide | [How to run tests](https://github.com/CoongCrafts/coong-wallet/tree/w3f-milestone-1#how-to-run-tests) |
| **0d.** | Docker | - [Dockerfile](https://github.com/CoongCrafts/coong-wallet/blob/w3f-milestone-1/Dockerfile)<br/>- [How to run the app on Docker](https://github.com/CoongCrafts/coong-wallet/blob/w3f-milestone-1/README.md#run-it-on-docker) |
| 1. | Wallet App / Core features | We've implemented the following features for the wallet app:<br/>- [Welcome screen](https://github.com/CoongCrafts/coong-wallet/blob/w3f-milestone-1/packages/ui/src/components/pages/Welcome.tsx)<br/>- [Unlock wallet](https://github.com/CoongCrafts/coong-wallet/blob/w3f-milestone-1/packages/ui/src/components/pages/UnlockWallet.tsx)<br/>- [Set up new wallet](https://github.com/CoongCrafts/coong-wallet/tree/w3f-milestone-1/packages/ui/src/components/pages/NewWallet/index.tsx)<br/>- [Create account](https://github.com/CoongCrafts/coong-wallet/blob/w3f-milestone-1/packages/ui/src/components/shared/NewAccountButton.tsx)<br/>- [List accounts](https://github.com/CoongCrafts/coong-wallet/blob/w3f-milestone-1/packages/ui/src/components/pages/Accounts/index.tsx)<br/>- [Request wallet access](https://github.com/CoongCrafts/coong-wallet/blob/w3f-milestone-1/packages/ui/src/components/pages/Request/RequestAccess/index.tsx)<br/>- [Approve transaction](https://github.com/CoongCrafts/coong-wallet/blob/w3f-milestone-1/packages/ui/src/components/pages/Request/RequestTransactionApproval/index.tsx) |
| 2. | Coong SDK | - [Source Code](https://github.com/CoongCrafts/coong-wallet/tree/w3f-milestone-1/packages/sdk)<br/> - [@coong/sdk](https://www.npmjs.com/package/@coong/sdk) |

**Additional Information**
- For [Known issues](https://github.com/CoongCrafts/coong-wallet/tree/w3f-milestone-1#known-issues), we plan to address them in the next milestone with a better UX responses to instruct users to disable `Block third-party cookie` setting and allow open Coong Wallet popup.
- Other issues:
  - Performance: `TODO update`
  - Metadata: `TODO update`
- Demo video:
  - Setup new wallet & accounts
 
https://user-images.githubusercontent.com/6867026/224897764-34f28ef1-fe43-4773-8ff6-f108f1b0994f.mov


  - Demo Dapp x Wallet interactions

https://user-images.githubusercontent.com/6867026/224897826-9e0460fb-d009-4738-8d71-52810a5afae1.mov


