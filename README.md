

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


The following are known issues with the Kleidi system:

- If the cold signers are malicious or compromised, they can execute transactions to compromise the system if neither recovery spells or the guardian are used
- If the hot signers are malicious or compromised, they can deploy a compromised system instance on a new chain with compromised recovery spells and malicious calldata checks that allow funds to be stolen
- If DEX's are whitelisted, this opens up the ability for the hot signers to steal funds from the system via high slippage and front and back running
- If a malicious protocol is whitelisted, this opens up the ability for hot signers to inadvernantly lose funds
- If a non-malicious but improperly configured protocol is whitelisted, this opens up the ability for hot signers to inadvernantly lose funds by using a protocol incorrectly
- Fee on transfer tokens may make the actual amount of tokens sent to destinations less or more than expected, this finding is out of scope for the system
- The system only works on EVM compatible chains, does not work on chains that have not undergone the Shanghai EVM upgrade
- The system only works with contracts that have a known ABI, it does not work with contracts that have dynamic ABIs
the return value of token transfers are unchecked, however the call to the token contract is checked, the timelock has no accounting mechanisms
- Salt in the `DeploymentParams` struct is not used in the call to `createSystemInstance`, this is a known issue, but is not a security concern. The same system instance with the same parameters can only be deployed once.

The whole of the docs contain known issues, so we don't want to be rewarding for things we already know. 
  - https://github.com/solidity-labs-io/kleidi/blob/main/docs/KNOWN_ISSUES.md 
  - https://github.com/solidity-labs-io/kleidi/blob/main/docs/EDGECASES.md 
  - https://github.com/solidity-labs-io/kleidi/blob/main/docs/REQUIREMENTS.md 


# Overview

[ ⭐️ SPONSORS: add info here ]

## Links

- **Previous audits:**  https://github.com/solidity-labs-io/kleidi/blob/main/audit/Kleidi-Recon-Report.pdf
- **Documentation:** https://github.com/solidity-labs-io/kleidi/tree/main/docs
- **Website:** https://www.kleidi.io/
- **X/Twitter:** https://x.com/KleidiWallet
- **Discord:** N/A

---

# Scope
### Files in scope

| File   | Logic Contracts | Interfaces | nSLOC | Purpose | Libraries used |
| ------ | --------------- | ---------- | ----- | -----   | ------------ |
| [/src/ConfigurablePause.sol](https://github.com/code-423n4/2024-10-kleidi/blob/main/src/ConfigurablePause.sol) | 1| **** | 54 | ||
| [/src/Timelock.sol](https://github.com/code-423n4/2024-10-kleidi/blob/main/src/Timelock.sol) | 1| **** | 565 | |@openzeppelin-contracts/contracts/access/AccessControl.sol<br>@openzeppelin-contracts/contracts/access/extensions/AccessControlEnumerable.sol<br>@openzeppelin-contracts/contracts/token/ERC1155/IERC1155Receiver.sol<br>@openzeppelin-contracts/contracts/token/ERC721/IERC721Receiver.sol<br>@openzeppelin-contracts/contracts/utils/introspection/ERC165.sol<br>@openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol<br>@safe/Safe.sol<br>src/BytesHelper.sol<br>src/ConfigurablePause.sol<br>src/utils/Constants.sol|
| [/src/TimelockFactory.sol](https://github.com/code-423n4/2024-10-kleidi/blob/main/src/TimelockFactory.sol) | 1| **** | 39 | |src/Timelock.sol<br>src/utils/Create2Helper.sol|
| [/src/Guard.sol](https://github.com/code-423n4/2024-10-kleidi/blob/main/src/Guard.sol) | 1| **** | 18 | |@safe/base/GuardManager.sol<br>@safe/common/Enum.sol<br>@safe/Safe.sol<br>src/BytesHelper.sol|
| [/src/views/AddressCalculation.sol](https://github.com/code-423n4/2024-10-kleidi/blob/main/src/views/AddressCalculation.sol) | 1| **** | 108 | |@safe/proxies/SafeProxyFactory.sol<br>@safe/proxies/SafeProxy.sol<br>src/Timelock.sol<br>src/TimelockFactory.sol<br>src/utils/Create2Helper.sol<br>src/InstanceDeployer.sol|
| [/src/utils/Constants.sol](https://github.com/code-423n4/2024-10-kleidi/blob/main/src/utils/Constants.sol) | ****| **** | 4 | ||
| [/src/utils/Create2Helper.sol](https://github.com/code-423n4/2024-10-kleidi/blob/main/src/utils/Create2Helper.sol) | ****| **** | 45 | ||
| [/src/InstanceDeployer.sol](https://github.com/code-423n4/2024-10-kleidi/blob/main/src/InstanceDeployer.sol) | 1| **** | 214 | |@safe/proxies/SafeProxyFactory.sol<br>@safe/base/ModuleManager.sol<br>@safe/base/OwnerManager.sol<br>@safe/base/GuardManager.sol<br>@interface/IMulticall3.sol<br>@safe/proxies/SafeProxy.sol<br>@safe/common/Enum.sol<br>@safe/Safe.sol<br>src/Guard.sol<br>src/Timelock.sol<br>src/TimelockFactory.sol<br>src/utils/Create2Helper.sol|
| [/src/RecoverySpell.sol](https://github.com/code-423n4/2024-10-kleidi/blob/main/src/RecoverySpell.sol) | 1| **** | 144 | |@openzeppelin-contracts/contracts/utils/cryptography/EIP712.sol<br>@openzeppelin-contracts/contracts/utils/cryptography/ECDSA.sol<br>@safe/common/Enum.sol<br>@safe/Safe.sol<br>@interface/IMulticall3.sol<br>@safe/base/OwnerManager.sol<br>@safe/base/ModuleManager.sol|
| [/src/RecoverySpellFactory.sol](https://github.com/code-423n4/2024-10-kleidi/blob/main/src/RecoverySpellFactory.sol) | 1| **** | 56 | |@src/RecoverySpell.sol<br>src/utils/Create2Helper.sol|
| [/src/deploy/SystemDeploy.s.sol](https://github.com/code-423n4/2024-10-kleidi/blob/main/src/deploy/SystemDeploy.s.sol) | 1| **** | 113 | |@forge-proposal-simulator/src/proposals/MultisigProposal.sol<br>@forge-proposal-simulator/addresses/Addresses.sol<br>src/Guard.sol<br>src/Timelock.sol<br>src/TimelockFactory.sol<br>src/InstanceDeployer.sol<br>src/views/AddressCalculation.sol<br>src/RecoverySpellFactory.sol|
| [/src/BytesHelper.sol](https://github.com/code-423n4/2024-10-kleidi/blob/main/src/BytesHelper.sol)_ | 1| **** | 33 | | Function `getFirstWord` is **out of scope** in this file. |
| **Totals** | **10** | **** | **1393** | | |




### Files out of scope
- See [out_of_scope.txt](https://github.com/code-423n4/2024-10-kleidi/blob/main/out_of_scope.txt)
- Any files not listed in the scope table are Out Of Scope 

## Scoping Q &amp; A

### General questions


| Question                                | Answer                       |
| --------------------------------------- | ---------------------------- |
| ERC20 used by the protocol              |       Any (all possible ERC20s)             |
| Test coverage                           | ✅ SCOUTS: Please populate this after running the test coverage command                          |
| ERC721 used  by the protocol            |           Any              |
| ERC777 used by the protocol             |          Any                |
| ERC1155 used by the protocol            |              Any            |
| Chains the protocol will be deployed on | Arbitrum,Ethereum,Optimism,Base |

### ERC20 token behaviors in scope

| Question                                                                                                                                                   | Answer |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| [Missing return values](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#missing-return-values)                                                      |   Out of scope  |
| [Fee on transfer](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#fee-on-transfer)                                                                  |  Out of scope  |
| [Balance changes outside of transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#balance-modifications-outside-of-transfers-rebasingairdrops) | Out of scope    |
| [Upgradeability](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#upgradable-tokens)                                                                 |   Out of scope  |
| [Flash minting](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#flash-mintable-tokens)                                                              | Out of scope    |
| [Pausability](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#pausable-tokens)                                                                      | Out of scope    |
| [Approval race protections](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#approval-race-protections)                                              | Out of scope    |
| [Revert on approval to zero address](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-approval-to-zero-address)                            | Out of scope    |
| [Revert on zero value approvals](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-approvals)                                    | Out of scope    |
| [Revert on zero value transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-transfers)                                    | Out of scope    |
| [Revert on transfer to the zero address](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-transfer-to-the-zero-address)                    | Out of scope    |
| [Revert on large approvals and/or transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-large-approvals--transfers)                  | Out of scope    |
| [Doesn't revert on failure](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#no-revert-on-failure)                                                   |  Out of scope   |
| [Multiple token addresses](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-transfers)                                          | Out of scope    |
| [Low decimals ( < 6)](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#low-decimals)                                                                 |   Out of scope  |
| [High decimals ( > 18)](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#high-decimals)                                                              | Out of scope    |
| [Blocklists](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#tokens-with-blocklists)                                                                | Out of scope    |

### External integrations (e.g., Uniswap) behavior in scope:


| Question                                                  | Answer |
| --------------------------------------------------------- | ------ |
| Enabling/disabling fees (e.g. Blur disables/enables fees) | No   |
| Pausability (e.g. Uniswap pool gets paused)               |  No   |
| Upgradeability (e.g. Uniswap gets upgraded)               |   No  |


### EIP compliance 
N/A


# Additional context

## Main invariants

- All invariants are described here: https://github.com/solidity-labs-io/kleidi/blob/main/docs/INVARIANTS.md



## Attack ideas (where to focus for bugs)
There are no specific areas of concern, rather we want to make sure that all system components work together. We have already done a lot of testing around this, but we always want more eyes on things.


## All trusted roles in the protocol

Cold signers & Hot signers


## Describe any novel or unique curve logic or mathematical models implemented in the contracts:

N/A



## Running tests
- Clone the repo;
```bash
git clone --recurse https://github.com/code-423n4/2024-10-kleidi.git
cd 2024-10-kleidi
```
- Before running the tests, make sure your `ETH_RPC_URL`is set.
For `bash/zsh`;
```bash
export ETH_RPC_URL="https://mainnet.infura.io/v3/YOUR_PROJECT_ID"
```

- Build;
```bash
foundryup      
forge build
``` 

- Test;
```bash
forge test -vvv
```

- Unit Testing;
```bash    
forge test --mc UnitTest -vvv
```

- Integration Testing;
```bash
forge test --mc IntegrationTest -vvv --fork-url $ETH_RPC_URL --fork-block-number 20515328
```
- Unit Test Coverage;
```bash
forge coverage --mc UnitTest --report lcov
```
- Unit & Integration Test Coverage;
```bash
forge coverage --report summary --report lcov --fork-url $ETH_RPC_URL --fork-block-number 20515328
```
- To run gas benchmarks;
```bash
forge test --gas-report
```

## Miscellaneous
Employees of Kleidi and employees' family members are ineligible to participate in this audit.

Code4rena's rules cannot be overridden by the contents of this README. In case of doubt, please check with C4 staff.


