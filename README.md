# ✨ So you want to run an audit

This `README.md` contains a set of checklists for our audit collaboration.

Your audit will use two repos: 
- **an _audit_ repo** (this one), which is used for scoping your audit and for providing information to wardens
- **a _findings_ repo**, where issues are submitted (shared with you after the audit) 

Ultimately, when we launch the audit, this repo will be made public and will contain the smart contracts to be reviewed and all the information needed for audit participants. The findings repo will be made public after the audit report is published and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the audit sponsor (⭐️)**.

---

# Audit setup

## 🐺 C4: Set up repos
- [ ] Create a new private repo named `YYYY-MM-sponsorname` using this repo as a template.
- [ ] Rename this repo to reflect audit date (if applicable)
- [ ] Rename audit H1 below
- [ ] Update pot sizes
  - [ ] Remove the "Bot race findings opt out" section if there's no bot race.
- [ ] Fill in start and end times in audit bullets below
- [ ] Add link to submission form in audit details below
- [ ] Add the information from the scoping form to the "Scoping Details" section at the bottom of this readme.
- [ ] Add matching info to the Code4rena site
- [ ] Add sponsor to this private repo with 'maintain' level access.
- [ ] Send the sponsor contact the url for this repo to follow the instructions below and add contracts here. 
- [ ] Delete this checklist.

# Repo setup

## ⭐️ Sponsor: Repo checklist

- [ ] Confirm that this repo is a self-contained repository with working commands that will build (at least) all in-scope contracts, and commands that will run tests producing gas reports for the relevant contracts.
- [ ] Please have final versions of contracts and documentation added/updated in this repo **no less than 48 business hours prior to audit start time.**
- [ ] Be prepared for a 🚨code freeze🚨 for the duration of the audit — important because it establishes a level playing field. We want to ensure everyone's looking at the same code, no matter when they look during the audit. (Note: this includes your own repo, since a PR can leak alpha to our wardens!)
- [ ] Modify the [Overview](#overview) section of this `README.md` file. Describe how your code is supposed to work with links to any relevent documentation and any other criteria/details that the auditors should keep in mind when reviewing. (Here are two well-constructed examples: [Ajna Protocol](https://github.com/code-423n4/2023-05-ajna) and [Maia DAO Ecosystem](https://github.com/code-423n4/2023-05-maia))
- [ ] Review the Gas award pool amount, if applicable. This can be adjusted up or down, based on your preference - just flag it for Code4rena staff so we can update the pool totals across all comms channels.
- [ ] Optional: pre-record a high-level overview of your protocol (not just specific smart contract functions). This saves wardens a lot of time wading through documentation.
- [ ] [This checklist in Notion](https://code4rena.notion.site/Key-info-for-Code4rena-sponsors-f60764c4c4574bbf8e7a6dbd72cc49b4#0cafa01e6201462e9f78677a39e09746) provides some best practices for Code4rena audit repos.

## ⭐️ Sponsor: Final touches
- [ ] Review and confirm the pull request created by the Scout (technical reviewer) who was assigned to your contest. *Note: any files not listed as "in scope" will be considered out of scope for the purposes of judging, even if the file will be part of the deployed contracts.*
- [ ] Check that images and other files used in this README have been uploaded to the repo as a file and then linked in the README using absolute path (e.g. `https://github.com/code-423n4/yourrepo-url/filepath.png`)
- [ ] Ensure that *all* links and image/file paths in this README use absolute paths, not relative paths
- [ ] Check that all README information is in markdown format (HTML does not render on Code4rena.com)
- [ ] Delete this checklist and all text above the line below when you're ready.

---

# Kleidi audit details
- Total Prize Pool: $40,000 in USDC
  - HM awards: $31,780 in USDC
  - QA awards: $1,320 in USDC
  - Judge awards: $3,800 in USDC
  - Validator awards: $2,600 in USDC
  - Scout awards: $500 in USDC
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts October 15, 2024 20:00 UTC
- Ends October 25, 2024 20:00 UTC

## Automated Findings / Publicly Known Issues

The 4naly3er report can be found [here](https://github.com/code-423n4/2024-10-kleidi/blob/main/4naly3er-report.md).

_Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards._


## 🐺 C4: Begin Gist paste here (and delete this line)





# Scope

*See [scope.txt](https://github.com/code-423n4/2024-10-kleidi/blob/main/scope.txt)*

### Files in scope


| File   | Logic Contracts | Interfaces | nSLOC | Purpose | Libraries used |
| ------ | --------------- | ---------- | ----- | -----   | ------------ |
| /src/ConfigurablePause.sol | 1| **** | 54 | ||
| /src/Timelock.sol | 1| **** | 565 | |@openzeppelin-contracts/contracts/access/AccessControl.sol<br>@openzeppelin-contracts/contracts/access/extensions/AccessControlEnumerable.sol<br>@openzeppelin-contracts/contracts/token/ERC1155/IERC1155Receiver.sol<br>@openzeppelin-contracts/contracts/token/ERC721/IERC721Receiver.sol<br>@openzeppelin-contracts/contracts/utils/introspection/ERC165.sol<br>@openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol<br>@safe/Safe.sol<br>src/BytesHelper.sol<br>src/ConfigurablePause.sol<br>src/utils/Constants.sol|
| /src/TimelockFactory.sol | 1| **** | 39 | |src/Timelock.sol<br>src/utils/Create2Helper.sol|
| /src/Guard.sol | 1| **** | 18 | |@safe/base/GuardManager.sol<br>@safe/common/Enum.sol<br>@safe/Safe.sol<br>src/BytesHelper.sol|
| /src/views/AddressCalculation.sol | 1| **** | 108 | |@safe/proxies/SafeProxyFactory.sol<br>@safe/proxies/SafeProxy.sol<br>src/Timelock.sol<br>src/TimelockFactory.sol<br>src/utils/Create2Helper.sol<br>src/InstanceDeployer.sol|
| /src/utils/Constants.sol | ****| **** | 4 | ||
| /src/utils/Create2Helper.sol | ****| **** | 45 | ||
| /src/InstanceDeployer.sol | 1| **** | 214 | |@safe/proxies/SafeProxyFactory.sol<br>@safe/base/ModuleManager.sol<br>@safe/base/OwnerManager.sol<br>@safe/base/GuardManager.sol<br>@interface/IMulticall3.sol<br>@safe/proxies/SafeProxy.sol<br>@safe/common/Enum.sol<br>@safe/Safe.sol<br>src/Guard.sol<br>src/Timelock.sol<br>src/TimelockFactory.sol<br>src/utils/Create2Helper.sol|
| /src/RecoverySpell.sol | 1| **** | 144 | |@openzeppelin-contracts/contracts/utils/cryptography/EIP712.sol<br>@openzeppelin-contracts/contracts/utils/cryptography/ECDSA.sol<br>@safe/common/Enum.sol<br>@safe/Safe.sol<br>@interface/IMulticall3.sol<br>@safe/base/OwnerManager.sol<br>@safe/base/ModuleManager.sol|
| /src/RecoverySpellFactory.sol | 1| **** | 56 | |@src/RecoverySpell.sol<br>src/utils/Create2Helper.sol|
| /src/deploy/SystemDeploy.s.sol | 1| **** | 113 | |@forge-proposal-simulator/src/proposals/MultisigProposal.sol<br>@forge-proposal-simulator/addresses/Addresses.sol<br>src/Guard.sol<br>src/Timelock.sol<br>src/TimelockFactory.sol<br>src/InstanceDeployer.sol<br>src/views/AddressCalculation.sol<br>src/RecoverySpellFactory.sol|
| /src/BytesHelper.sol | 1| **** | 33 | | Function `getFirstWord` is **out of scope** in this file. |
| **Totals** | **10** | **** | **1393** | | |

### Files out of scope

*See [out_of_scope.txt](https://github.com/code-423n4/2024-10-kleidi/blob/main/out_of_scope.txt)*

| File         |
| ------------ |
| ./lib/forge-proposal-simulator/addresses/Addresses.sol |
| ./lib/forge-proposal-simulator/addresses/IAddresses.sol |
| ./lib/forge-proposal-simulator/mocks/MockAuction.sol |
| ./lib/forge-proposal-simulator/mocks/MockBravoProposal.sol |
| ./lib/forge-proposal-simulator/mocks/MockDuplicatedActionProposal.sol |
| ./lib/forge-proposal-simulator/mocks/MockGovernorAlpha.sol |
| ./lib/forge-proposal-simulator/mocks/MockMultisigProposal.sol |
| ./lib/forge-proposal-simulator/mocks/MockOZGovernorProposal.sol |
| ./lib/forge-proposal-simulator/mocks/MockSavingContract.sol |
| ./lib/forge-proposal-simulator/mocks/MockTimelockProposal.sol |
| ./lib/forge-proposal-simulator/mocks/MockToken.sol |
| ./lib/forge-proposal-simulator/mocks/MockTokenWrapper.sol |
| ./lib/forge-proposal-simulator/mocks/MockUpgrade.sol |
| ./lib/forge-proposal-simulator/mocks/MockVotingContract.sol |
| ./lib/forge-proposal-simulator/src/interface/ICompoundConfigurator.sol |
| ./lib/forge-proposal-simulator/src/interface/IGovernor.sol |
| ./lib/forge-proposal-simulator/src/interface/IGovernorBravo.sol |
| ./lib/forge-proposal-simulator/src/interface/IProxy.sol |
| ./lib/forge-proposal-simulator/src/interface/IProxyAdmin.sol |
| ./lib/forge-proposal-simulator/src/interface/ITimelockController.sol |
| ./lib/forge-proposal-simulator/src/interface/IVotes.sol |
| ./lib/forge-proposal-simulator/src/proposals/CrossChainProposal.sol |
| ./lib/forge-proposal-simulator/src/proposals/GovernorBravoProposal.sol |
| ./lib/forge-proposal-simulator/src/proposals/IProposal.sol |
| ./lib/forge-proposal-simulator/src/proposals/MultisigProposal.sol |
| ./lib/forge-proposal-simulator/src/proposals/OZGovernorProposal.sol |
| ./lib/forge-proposal-simulator/src/proposals/Proposal.sol |
| ./lib/forge-proposal-simulator/src/proposals/TimelockProposal.sol |
| ./lib/forge-proposal-simulator/test/Addresses.t.sol |
| ./lib/forge-proposal-simulator/test/BravoProposal.t.sol |
| ./lib/forge-proposal-simulator/test/DuplicatedAction.t.sol |
| ./lib/forge-proposal-simulator/test/GovernorOZProposal.t.sol |
| ./lib/forge-proposal-simulator/test/MultisigProposal.t.sol |
| ./lib/forge-proposal-simulator/test/MultisigProposalCalldataTest.t.sol |
| ./lib/forge-proposal-simulator/test/TimelockProposal.t.sol |
| ./lib/forge-proposal-simulator/test/multisigProposals/MultisigProposal_01.sol |
| ./lib/forge-proposal-simulator/test/multisigProposals/MultisigProposal_02.sol |
| ./lib/forge-proposal-simulator/test/multisigProposals/MultisigProposal_03.sol |
| ./lib/forge-proposal-simulator/test/multisigProposals/MultisigProposal_04.sol |
| ./lib/forge-proposal-simulator/test/multisigProposals/MultisigProposal_05.sol |
| ./lib/forge-proposal-simulator/utils/Address.sol |
| ./lib/forge-proposal-simulator/utils/Constants.sol |
| ./lib/safe-smart-account/certora/harnesses/SafeHarness.sol |
| ./lib/safe-smart-account/contracts/Safe.sol |
| ./lib/safe-smart-account/contracts/SafeL2.sol |
| ./lib/safe-smart-account/contracts/accessors/SimulateTxAccessor.sol |
| ./lib/safe-smart-account/contracts/base/Executor.sol |
| ./lib/safe-smart-account/contracts/base/FallbackManager.sol |
| ./lib/safe-smart-account/contracts/base/GuardManager.sol |
| ./lib/safe-smart-account/contracts/base/ModuleManager.sol |
| ./lib/safe-smart-account/contracts/base/OwnerManager.sol |
| ./lib/safe-smart-account/contracts/common/Enum.sol |
| ./lib/safe-smart-account/contracts/common/NativeCurrencyPaymentFallback.sol |
| ./lib/safe-smart-account/contracts/common/SecuredTokenTransfer.sol |
| ./lib/safe-smart-account/contracts/common/SelfAuthorized.sol |
| ./lib/safe-smart-account/contracts/common/SignatureDecoder.sol |
| ./lib/safe-smart-account/contracts/common/Singleton.sol |
| ./lib/safe-smart-account/contracts/common/StorageAccessible.sol |
| ./lib/safe-smart-account/contracts/examples/guards/DebugTransactionGuard.sol |
| ./lib/safe-smart-account/contracts/examples/guards/DelegateCallTransactionGuard.sol |
| ./lib/safe-smart-account/contracts/examples/guards/OnlyOwnersGuard.sol |
| ./lib/safe-smart-account/contracts/examples/guards/ReentrancyTransactionGuard.sol |
| ./lib/safe-smart-account/contracts/examples/libraries/Migrate_1_3_0_to_1_2_0.sol |
| ./lib/safe-smart-account/contracts/external/SafeMath.sol |
| ./lib/safe-smart-account/contracts/handler/CompatibilityFallbackHandler.sol |
| ./lib/safe-smart-account/contracts/handler/HandlerContext.sol |
| ./lib/safe-smart-account/contracts/handler/TokenCallbackHandler.sol |
| ./lib/safe-smart-account/contracts/interfaces/ERC1155TokenReceiver.sol |
| ./lib/safe-smart-account/contracts/interfaces/ERC721TokenReceiver.sol |
| ./lib/safe-smart-account/contracts/interfaces/ERC777TokensRecipient.sol |
| ./lib/safe-smart-account/contracts/interfaces/IERC165.sol |
| ./lib/safe-smart-account/contracts/interfaces/ISignatureValidator.sol |
| ./lib/safe-smart-account/contracts/interfaces/ViewStorageAccessible.sol |
| ./lib/safe-smart-account/contracts/libraries/CreateCall.sol |
| ./lib/safe-smart-account/contracts/libraries/MultiSend.sol |
| ./lib/safe-smart-account/contracts/libraries/MultiSendCallOnly.sol |
| ./lib/safe-smart-account/contracts/libraries/SafeStorage.sol |
| ./lib/safe-smart-account/contracts/libraries/SignMessageLib.sol |
| ./lib/safe-smart-account/contracts/proxies/IProxyCreationCallback.sol |
| ./lib/safe-smart-account/contracts/proxies/SafeProxy.sol |
| ./lib/safe-smart-account/contracts/proxies/SafeProxyFactory.sol |
| ./lib/safe-smart-account/contracts/test/4337/Test4337ModuleAndHandler.sol |
| ./lib/safe-smart-account/contracts/test/ERC1155Token.sol |
| ./lib/safe-smart-account/contracts/test/ERC20Token.sol |
| ./lib/safe-smart-account/contracts/test/TestHandler.sol |
| ./lib/safe-smart-account/contracts/test/Token.sol |
| ./src/interface/CErc20Interface.sol |
| ./src/interface/CEtherInterface.sol |
| ./src/interface/IMorpho.sol |
| ./src/interface/IMulticall3.sol |
| ./src/interface/WETH9.sol |
| ./test/integration/AddressCalculation.t.sol |
| ./test/integration/Deployment.t.sol |
| ./test/integration/InstanceDeployer.t.sol |
| ./test/integration/RecoverySpells.t.sol |
| ./test/integration/SocialRecovery.t.sol |
| ./test/integration/System.t.sol |
| ./test/mock/MockERC1155.sol |
| ./test/mock/MockERC721.sol |
| ./test/mock/MockLending.sol |
| ./test/mock/MockReentrancyExecutor.sol |
| ./test/mock/MockSafe.sol |
| ./test/unit/BytesHelper.t.sol |
| ./test/unit/CalldataList.t.sol |
| ./test/unit/Guard.t.sol |
| ./test/unit/RecoverySpell.t.sol |
| ./test/unit/RecoverySpellFactory.t.sol |
| ./test/unit/Timelock.t.sol |
| ./test/unit/TimelockFactory.t.sol |
| ./test/unit/TimelockPause.t.sol |
| ./test/unit/TimelockReceiving.t.sol |
| ./test/utils/CallHelper.t.sol |
| ./test/utils/NestedArrayHelper.sol |
| ./test/utils/SigHelper.sol |
| ./test/utils/SystemIntegrationFixture.sol |
| ./test/utils/TimelockUnitFixture.sol |
| Totals: 116 |

