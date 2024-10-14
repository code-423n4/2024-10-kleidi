# Report


## Gas Optimizations


| |Issue|Instances|
|-|:-|:-:|
| [GAS-1](#GAS-1) | Using bools for storage incurs overhead | 1 |
| [GAS-2](#GAS-2) | Cache array length outside of loop | 27 |
| [GAS-3](#GAS-3) | For Operations that will not overflow, you could use unchecked | 114 |
| [GAS-4](#GAS-4) | Use Custom Errors instead of Revert Strings to save Gas | 32 |
| [GAS-5](#GAS-5) | Avoid contract existence checks by using low level calls | 3 |
| [GAS-6](#GAS-6) | Functions guaranteed to revert when called by normal users can be marked `payable` | 5 |
| [GAS-7](#GAS-7) | `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`) | 30 |
| [GAS-8](#GAS-8) | Using `private` rather than `public` for constants, saves gas | 6 |
| [GAS-9](#GAS-9) | Splitting require() statements that use && saves gas | 1 |
| [GAS-10](#GAS-10) | Increments/decrements can be unchecked in for-loops | 29 |
| [GAS-11](#GAS-11) | Use != 0 instead of > 0 for unsigned integer comparison | 3 |
### <a name="GAS-1"></a>[GAS-1] Using bools for storage incurs overhead
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past. See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27).

*Instances (1)*:
```solidity
File: ./src/Timelock.sol

97:     bool public initialized;

```

### <a name="GAS-2"></a>[GAS-2] Cache array length outside of loop
If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

*Instances (27)*:
```solidity
File: ./src/InstanceDeployer.sol

258:             for (uint256 i = 0; i < calls3.length; i++) {

275:             for (uint256 i = 0; i < instance.recoverySpells.length; i++) {

307:             for (uint256 i = 1; i < instance.owners.length - 1; i++) {

```

```solidity
File: ./src/RecoverySpell.sol

196:         for (uint256 i = 0; i < owners.length; i++) {

214:         for (uint256 i = 0; i < v.length; i++) {

276:         for (uint256 i = 1; i < owners.length; i++) {

291:         for (uint256 i = 0; i < calls3.length; i++) {

```

```solidity
File: ./src/RecoverySpellFactory.sol

59:         for (uint256 i = 0; i < owners.length; i++) {

97:         for (uint256 i = 0; i < owners.length; i++) {

98:             for (uint256 j = i + 1; j < owners.length; j++) {

135:         for (uint256 i = 0; i < owners.length; i++) {

```

```solidity
File: ./src/Timelock.sol

306:         for (uint256 i = 0; i < hotSigners.length; i++) {

461:         for (uint256 i = 0; i < indexes.length; i++) {

488:         for (uint256 i = 0; i < calldataChecks.length; i++) {

572:         for (uint256 i = 0; i < targets.length; i++) {

639:         for (uint256 i = 0; i < targets.length; i++) {

692:         for (uint256 i = 0; i < proposals.length; i++) {

743:         for (uint256 i = 0; i < targets.length; i++) {

913:             for (uint256 i = 0; i < dataHashes.length; i++) {

949:         for (uint256 i = 0; i < contractAddresses.length; i++) {

1093:             for (uint256 i = 0; i < indexes.length; i++) {

1133:         for (uint256 i = 0; i < data.length; i++) {

1178:         for (uint256 i = 0; i < contractAddresses.length; i++) {

1214:         for (uint256 i = 0; i < removedDataHashes.length; i++) {

1228:             for (uint256 i = 0; i < dataHashes.length; i++) {

1277:             for (uint256 i = 0; i < dataHashes.length; i++) {

```

```solidity
File: ./src/views/AddressCalculation.sol

36:         for (uint256 i = 0; i < instance.recoverySpells.length; i++) {

```

### <a name="GAS-3"></a>[GAS-3] For Operations that will not overflow, you could use unchecked

*Instances (114)*:
```solidity
File: ./src/BytesHelper.sol

25:         assembly ("memory-safe") {

50:         uint256 length = end - start;

53:         for (uint256 i = 0; i < length; i++) {

54:             sliced[i] = toSlice[i + start];

```

```solidity
File: ./src/ConfigurablePause.sol

70:         return block.timestamp <= pauseStartTime + pauseDuration;

```

```solidity
File: ./src/Guard.sol

3: import {BaseGuard} from "@safe/base/GuardManager.sol";

4: import {Enum} from "@safe/common/Enum.sol";

5: import {Safe} from "@safe/Safe.sol";

7: import {BytesHelper} from "src/BytesHelper.sol";

```

```solidity
File: ./src/InstanceDeployer.sol

3: import {SafeProxyFactory} from "@safe/proxies/SafeProxyFactory.sol";

4: import {ModuleManager} from "@safe/base/ModuleManager.sol";

5: import {OwnerManager} from "@safe/base/OwnerManager.sol";

6: import {GuardManager} from "@safe/base/GuardManager.sol";

7: import {IMulticall3} from "@interface/IMulticall3.sol";

8: import {SafeProxy} from "@safe/proxies/SafeProxy.sol";

9: import {Enum} from "@safe/common/Enum.sol";

10: import {Safe} from "@safe/Safe.sol";

12: import {Guard} from "src/Guard.sol";

13: import {Timelock} from "src/Timelock.sol";

14: import {TimelockFactory, DeploymentParams} from "src/TimelockFactory.sol";

17: } from "src/utils/Create2Helper.sol";

253:             2 + instance.recoverySpells.length + instance.owners.length

258:             for (uint256 i = 0; i < calls3.length; i++) {

263:             calls3[index++].callData =

269:             calls3[index++].callData = abi.encodeWithSelector(

275:             for (uint256 i = 0; i < instance.recoverySpells.length; i++) {

287:                 calls3[index++].callData = abi.encodeWithSelector(

293:             calls3[index++].callData = abi.encodeWithSelector(

307:             for (uint256 i = 1; i < instance.owners.length - 1; i++) {

308:                 calls3[index++].callData = abi.encodeWithSelector(

323:                 calls3[index++].callData = abi.encodeWithSelector(

325:                     instance.owners[instance.owners.length - 1],

342:             assembly ("memory-safe") {

```

```solidity
File: ./src/RecoverySpell.sol

4:     "@openzeppelin-contracts/contracts/utils/cryptography/EIP712.sol";

6:     "@openzeppelin-contracts/contracts/utils/cryptography/ECDSA.sol";

8: import {Enum} from "@safe/common/Enum.sol";

9: import {Safe} from "@safe/Safe.sol";

11: import {IMulticall3} from "@interface/IMulticall3.sol";

12: import {OwnerManager} from "@safe/base/OwnerManager.sol";

13: import {ModuleManager} from "@safe/base/ModuleManager.sol";

181:             block.timestamp > recoveryInitiated + delay,

196:         for (uint256 i = 0; i < owners.length; i++) {

198:             assembly ("memory-safe") {

214:         for (uint256 i = 0; i < v.length; i++) {

218:             assembly ("memory-safe") {

252:             new IMulticall3.Call3[](owners.length + existingOwnersLength + 1);

259:         for (uint256 i = 0; i < existingOwnersLength - 1; i++) {

260:             calls3[index++].callData = abi.encodeWithSelector(

268:         calls3[index++].callData = abi.encodeWithSelector(

271:             existingOwners[existingOwnersLength - 1],

276:         for (uint256 i = 1; i < owners.length; i++) {

277:             calls3[index++].callData = abi.encodeWithSelector(

283:         calls3[index++].callData = abi.encodeWithSelector(

291:         for (uint256 i = 0; i < calls3.length; i++) {

```

```solidity
File: ./src/RecoverySpellFactory.sol

3: import {RecoverySpell} from "@src/RecoverySpell.sol";

4: import {calculateCreate2Address} from "src/utils/Create2Helper.sol";

56:         require(safe.code.length != 0, "RecoverySpell: safe non-existent");

59:         for (uint256 i = 0; i < owners.length; i++) {

64:             assembly ("memory-safe") {

97:         for (uint256 i = 0; i < owners.length; i++) {

98:             for (uint256 j = i + 1; j < owners.length; j++) {

135:         for (uint256 i = 0; i < owners.length; i++) {

```

```solidity
File: ./src/Timelock.sol

6: } from "@openzeppelin-contracts/contracts/access/AccessControl.sol";

8:     "@openzeppelin-contracts/contracts/access/extensions/AccessControlEnumerable.sol";

10:     "@openzeppelin-contracts/contracts/token/ERC1155/IERC1155Receiver.sol";

12:     "@openzeppelin-contracts/contracts/token/ERC721/IERC721Receiver.sol";

16: } from "@openzeppelin-contracts/contracts/utils/introspection/ERC165.sol";

18:     "@openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol";

19: import {Safe} from "@safe/Safe.sol";

21: import {BytesHelper} from "src/BytesHelper.sol";

22: import {ConfigurablePause} from "src/ConfigurablePause.sol";

23: import {_DONE_TIMESTAMP, MIN_DELAY, MAX_DELAY} from "src/utils/Constants.sol";

306:         for (uint256 i = 0; i < hotSigners.length; i++) {

403:             && timestamp + expirationPeriod > block.timestamp;

418:         require(timestamp != 0, "Timelock: operation non-existent");

421:         return block.timestamp >= timestamp + expirationPeriod;

461:         for (uint256 i = 0; i < indexes.length; i++) {

488:         for (uint256 i = 0; i < calldataChecks.length; i++) {

572:         for (uint256 i = 0; i < targets.length; i++) {

639:         for (uint256 i = 0; i < targets.length; i++) {

692:         for (uint256 i = 0; i < proposals.length; i++) {

743:         for (uint256 i = 0; i < targets.length; i++) {

907:                 calldataChecks[calldataChecks.length - 1];

913:             for (uint256 i = 0; i < dataHashes.length; i++) {

949:         for (uint256 i = 0; i < contractAddresses.length; i++) {

1004:         timestamps[id] = block.timestamp + delay;

1093:             for (uint256 i = 0; i < indexes.length; i++) {

1133:         for (uint256 i = 0; i < data.length; i++) {

1136:                 data[i].length == endIndex - startIndex,

1178:         for (uint256 i = 0; i < contractAddresses.length; i++) {

1214:         for (uint256 i = 0; i < removedDataHashes.length; i++) {

1222:                 calldataChecks[calldataChecks.length - 1];

1228:             for (uint256 i = 0; i < dataHashes.length; i++) {

1265:                 calldataChecks[checksLength - 1];

1277:             for (uint256 i = 0; i < dataHashes.length; i++) {

1281:             checksLength--;

```

```solidity
File: ./src/TimelockFactory.sol

3: import {Timelock} from "src/Timelock.sol";

4: import {calculateCreate2Address} from "src/utils/Create2Helper.sol";

```

```solidity
File: ./src/deploy/SystemDeploy.s.sol

4:     "@forge-proposal-simulator/src/proposals/MultisigProposal.sol";

5: import {Addresses} from "@forge-proposal-simulator/addresses/Addresses.sol";

7: import {Guard} from "src/Guard.sol";

8: import {Timelock} from "src/Timelock.sol";

9: import {TimelockFactory} from "src/TimelockFactory.sol";

10: import {InstanceDeployer} from "src/InstanceDeployer.sol";

11: import {AddressCalculation} from "src/views/AddressCalculation.sol";

12: import {RecoverySpellFactory} from "src/RecoverySpellFactory.sol";

27:         addresses = new Addresses("./addresses", chainIds);

```

```solidity
File: ./src/views/AddressCalculation.sol

3: import {SafeProxyFactory} from "@safe/proxies/SafeProxyFactory.sol";

4: import {SafeProxy} from "@safe/proxies/SafeProxy.sol";

6: import {Timelock} from "src/Timelock.sol";

7: import {TimelockFactory, DeploymentParams} from "src/TimelockFactory.sol";

10: } from "src/utils/Create2Helper.sol";

15: } from "src/InstanceDeployer.sol";

36:         for (uint256 i = 0; i < instance.recoverySpells.length; i++) {

```

### <a name="GAS-4"></a>[GAS-4] Use Custom Errors instead of Revert Strings to save Gas
Custom errors are available from solidity version 0.8.4. Custom errors save [**~50 gas**](https://gist.github.com/IllIllI000/ad1bd0d29a0101b25e57c293b4b0c746) each time they're hit by [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas

Additionally, custom errors can be used inside and outside of contracts (including interfaces and libraries).

Source: <https://blog.soliditylang.org/2021/04/21/custom-errors/>:

> Starting from [Solidity v0.8.4](https://github.com/ethereum/solidity/releases/tag/v0.8.4), there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., `revert("Insufficient funds.");`), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Consider replacing **all revert strings** with custom errors in the solution, and particularly those that have multiple occurrences:

*Instances (32)*:
```solidity
File: ./src/BytesHelper.sol

12:         require(toSlice.length >= 4, "No function signature");

23:         require(toSlice.length >= 32, "Length less than 32 bytes");

48:         require(start < end, "Start index not less than end index");

```

```solidity
File: ./src/ConfigurablePause.sol

60:         require(!paused(), "Pausable: paused");

```

```solidity
File: ./src/Guard.sol

59:             require(data.length == 0 && value == 0, "Guard: no self calls");

```

```solidity
File: ./src/RecoverySpellFactory.sol

56:         require(safe.code.length != 0, "RecoverySpell: safe non-existent");

70:             require(!found, "RecoverySpell: Duplicate owner");

133:         require(threshold != 0, "RecoverySpell: Threshold must be gt 0");

134:         require(delay <= 365 days, "RecoverySpell: Delay must be lte a year");

136:             require(owners[i] != address(0), "RecoverySpell: Owner cannot be 0");

```

```solidity
File: ./src/Timelock.sol

323:         require(!initialized, "Timelock: already initialized");

339:         require(msg.sender == safe, "Timelock: caller is not the safe");

418:         require(timestamp != 0, "Timelock: operation non-existent");

419:         require(timestamp != 1, "Timelock: operation already executed");

532:         require(_liveProposals.add(id), "Timelock: duplicate id");

566:         require(_liveProposals.add(id), "Timelock: duplicate id");

599:         require(_liveProposals.remove(id), "Timelock: proposal does not exist");

600:         require(isOperationReady(id), "Timelock: operation is not ready");

636:         require(_liveProposals.remove(id), "Timelock: proposal does not exist");

637:         require(isOperationReady(id), "Timelock: operation is not ready");

672:         require(isOperationExpired(id), "Timelock: operation not expired");

767:         require(role != DEFAULT_ADMIN_ROLE, "Timelock: cannot grant admin role");

973:         require(newPeriod >= MIN_DELAY, "Timelock: delay out of bounds");

1001:         require(!isOperation(id), "Timelock: operation already scheduled");

1003:         require(delay >= minDelay, "Timelock: insufficient delay");

1013:         require(isOperationReady(id), "Timelock: operation is not ready");

1025:         require(success, "Timelock: underlying transaction reverted");

1045:         require(selector != bytes4(0), "CalldataList: Selector cannot be empty");

1056:         require(contractAddress != safe, "CalldataList: Address cannot be safe");

1078:             require(data.length == 0, "CalldataList: Data must be empty");

1086:             require(data.length != 0, "CalldataList: Data empty");

1260:         require(checksLength > 0, "CalldataList: No calldata checks to remove");

```

### <a name="GAS-5"></a>[GAS-5] Avoid contract existence checks by using low level calls
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence

*Instances (3)*:
```solidity
File: ./src/InstanceDeployer.sol

387:                 Enum.Operation.DelegateCall,

```

```solidity
File: ./src/RecoverySpell.sol

215:             address recoveredAddress = ECDSA.recover(digest, v[i], r[i], s[i]);

311:                 Enum.Operation.DelegateCall

```

### <a name="GAS-6"></a>[GAS-6] Functions guaranteed to revert when called by normal users can be marked `payable`
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

*Instances (5)*:
```solidity
File: ./src/Timelock.sol

657:     function cancel(bytes32 id) external onlySafe whenNotPaused {

802:     function revokeHotSigner(address deprecatedHotSigner) external onlySafe {

815:     function setGuardian(address newGuardian) public onlyTimelock {

960:     function updateDelay(uint256 newDelay) external onlyTimelock {

972:     function updateExpirationPeriod(uint256 newPeriod) external onlyTimelock {

```

### <a name="GAS-7"></a>[GAS-7] `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`)
Pre-increments and pre-decrements are cheaper.

For a `uint256 i` variable, the following is true with the Optimizer enabled at 10k:

**Increment:**

- `i += 1` is the most expensive form
- `i++` costs 6 gas less than `i += 1`
- `++i` costs 5 gas less than `i++` (11 gas less than `i += 1`)

**Decrement:**

- `i -= 1` is the most expensive form
- `i--` costs 11 gas less than `i -= 1`
- `--i` costs 5 gas less than `i--` (16 gas less than `i -= 1`)

Note that post-increments (or post-decrements) return the old value before incrementing or decrementing, hence the name *post-increment*:

```solidity
uint i = 1;  
uint j = 2;
require(j == i++, "This will be false as i is incremented after the comparison");
```
  
However, pre-increments (or pre-decrements) return the new value:
  
```solidity
uint i = 1;  
uint j = 2;
require(j == ++i, "This will be true as i is incremented before the comparison");
```

In the pre-increment case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`.

Consider using pre-increments and pre-decrements where they are relevant (meaning: not where post-increments/decrements logic are relevant).

*Saves 5 gas per instance*

*Instances (30)*:
```solidity
File: ./src/BytesHelper.sol

53:         for (uint256 i = 0; i < length; i++) {

```

```solidity
File: ./src/InstanceDeployer.sol

258:             for (uint256 i = 0; i < calls3.length; i++) {

275:             for (uint256 i = 0; i < instance.recoverySpells.length; i++) {

307:             for (uint256 i = 1; i < instance.owners.length - 1; i++) {

```

```solidity
File: ./src/RecoverySpell.sol

196:         for (uint256 i = 0; i < owners.length; i++) {

214:         for (uint256 i = 0; i < v.length; i++) {

259:         for (uint256 i = 0; i < existingOwnersLength - 1; i++) {

276:         for (uint256 i = 1; i < owners.length; i++) {

291:         for (uint256 i = 0; i < calls3.length; i++) {

```

```solidity
File: ./src/RecoverySpellFactory.sol

59:         for (uint256 i = 0; i < owners.length; i++) {

97:         for (uint256 i = 0; i < owners.length; i++) {

98:             for (uint256 j = i + 1; j < owners.length; j++) {

135:         for (uint256 i = 0; i < owners.length; i++) {

```

```solidity
File: ./src/Timelock.sol

306:         for (uint256 i = 0; i < hotSigners.length; i++) {

461:         for (uint256 i = 0; i < indexes.length; i++) {

488:         for (uint256 i = 0; i < calldataChecks.length; i++) {

572:         for (uint256 i = 0; i < targets.length; i++) {

639:         for (uint256 i = 0; i < targets.length; i++) {

692:         for (uint256 i = 0; i < proposals.length; i++) {

743:         for (uint256 i = 0; i < targets.length; i++) {

913:             for (uint256 i = 0; i < dataHashes.length; i++) {

949:         for (uint256 i = 0; i < contractAddresses.length; i++) {

1093:             for (uint256 i = 0; i < indexes.length; i++) {

1133:         for (uint256 i = 0; i < data.length; i++) {

1178:         for (uint256 i = 0; i < contractAddresses.length; i++) {

1214:         for (uint256 i = 0; i < removedDataHashes.length; i++) {

1228:             for (uint256 i = 0; i < dataHashes.length; i++) {

1277:             for (uint256 i = 0; i < dataHashes.length; i++) {

1281:             checksLength--;

```

```solidity
File: ./src/views/AddressCalculation.sol

36:         for (uint256 i = 0; i < instance.recoverySpells.length; i++) {

```

### <a name="GAS-8"></a>[GAS-8] Using `private` rather than `public` for constants, saves gas
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

*Instances (6)*:
```solidity
File: ./src/ConfigurablePause.sol

32:     uint256 public constant MIN_PAUSE_DURATION = 1 days;

35:     uint256 public constant MAX_PAUSE_DURATION = 30 days;

```

```solidity
File: ./src/RecoverySpell.sol

69:     address public constant MULTICALL3 =

73:     address public constant SENTINEL = address(0x1);

76:     bytes32 public constant RECOVERY_TYPEHASH = keccak256(

```

```solidity
File: ./src/Timelock.sol

88:     bytes32 public constant HOT_SIGNER_ROLE = keccak256("HOT_SIGNER_ROLE");

```

### <a name="GAS-9"></a>[GAS-9] Splitting require() statements that use && saves gas

*Instances (1)*:
```solidity
File: ./src/Guard.sol

59:             require(data.length == 0 && value == 0, "Guard: no self calls");

```

### <a name="GAS-10"></a>[GAS-10] Increments/decrements can be unchecked in for-loops
In Solidity 0.8+, there's a default overflow check on unsigned integers. It's possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.

[ethereum/solidity#10695](https://github.com/ethereum/solidity/issues/10695)

The change would be:

```diff
- for (uint256 i; i < numIterations; i++) {
+ for (uint256 i; i < numIterations;) {
 // ...  
+   unchecked { ++i; }
}  
```

These save around **25 gas saved** per instance.

The same can be applied with decrements (which should use `break` when `i == 0`).

The risk of overflow is non-existent for `uint256`.

*Instances (29)*:
```solidity
File: ./src/BytesHelper.sol

53:         for (uint256 i = 0; i < length; i++) {

```

```solidity
File: ./src/InstanceDeployer.sol

258:             for (uint256 i = 0; i < calls3.length; i++) {

275:             for (uint256 i = 0; i < instance.recoverySpells.length; i++) {

307:             for (uint256 i = 1; i < instance.owners.length - 1; i++) {

```

```solidity
File: ./src/RecoverySpell.sol

196:         for (uint256 i = 0; i < owners.length; i++) {

214:         for (uint256 i = 0; i < v.length; i++) {

259:         for (uint256 i = 0; i < existingOwnersLength - 1; i++) {

276:         for (uint256 i = 1; i < owners.length; i++) {

291:         for (uint256 i = 0; i < calls3.length; i++) {

```

```solidity
File: ./src/RecoverySpellFactory.sol

59:         for (uint256 i = 0; i < owners.length; i++) {

97:         for (uint256 i = 0; i < owners.length; i++) {

98:             for (uint256 j = i + 1; j < owners.length; j++) {

135:         for (uint256 i = 0; i < owners.length; i++) {

```

```solidity
File: ./src/Timelock.sol

306:         for (uint256 i = 0; i < hotSigners.length; i++) {

461:         for (uint256 i = 0; i < indexes.length; i++) {

488:         for (uint256 i = 0; i < calldataChecks.length; i++) {

572:         for (uint256 i = 0; i < targets.length; i++) {

639:         for (uint256 i = 0; i < targets.length; i++) {

692:         for (uint256 i = 0; i < proposals.length; i++) {

743:         for (uint256 i = 0; i < targets.length; i++) {

913:             for (uint256 i = 0; i < dataHashes.length; i++) {

949:         for (uint256 i = 0; i < contractAddresses.length; i++) {

1093:             for (uint256 i = 0; i < indexes.length; i++) {

1133:         for (uint256 i = 0; i < data.length; i++) {

1178:         for (uint256 i = 0; i < contractAddresses.length; i++) {

1214:         for (uint256 i = 0; i < removedDataHashes.length; i++) {

1228:             for (uint256 i = 0; i < dataHashes.length; i++) {

1277:             for (uint256 i = 0; i < dataHashes.length; i++) {

```

```solidity
File: ./src/views/AddressCalculation.sol

36:         for (uint256 i = 0; i < instance.recoverySpells.length; i++) {

```

### <a name="GAS-11"></a>[GAS-11] Use != 0 instead of > 0 for unsigned integer comparison

*Instances (3)*:
```solidity
File: ./src/Timelock.sol

393:         return timestamps[id] > 0;

485:             calldataChecks.length > 0, "CalldataList: No calldata checks found"

1260:         require(checksLength > 0, "CalldataList: No calldata checks to remove");

```


## Non Critical Issues


| |Issue|Instances|
|-|:-|:-:|
| [NC-1](#NC-1) | Replace `abi.encodeWithSignature` and `abi.encodeWithSelector` with `abi.encodeCall` which keeps the code typo/type safe | 15 |
| [NC-2](#NC-2) | `require()` should be used instead of `assert()` | 13 |
| [NC-3](#NC-3) | Use `string.concat()` or `bytes.concat()` instead of `abi.encodePacked` | 10 |
| [NC-4](#NC-4) | `constant`s should be defined rather than using magic numbers | 12 |
| [NC-5](#NC-5) | Control structures do not follow the Solidity Style Guide | 2 |
| [NC-6](#NC-6) | Default Visibility for constants | 3 |
| [NC-7](#NC-7) | Functions should not be longer than 50 lines | 39 |
| [NC-8](#NC-8) | Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor | 2 |
| [NC-9](#NC-9) | `address`s shouldn't be hard-coded | 1 |
| [NC-10](#NC-10) | Variable names that consist of all capital letters should be reserved for `constant`/`immutable` variables | 12 |
| [NC-11](#NC-11) | Avoid the use of sensitive terms | 2 |
| [NC-12](#NC-12) | Use Underscores for Number Literals (add an underscore every 3 digits) | 3 |
| [NC-13](#NC-13) | Variables need not be initialized to zero | 28 |
### <a name="NC-1"></a>[NC-1] Replace `abi.encodeWithSignature` and `abi.encodeWithSelector` with `abi.encodeCall` which keeps the code typo/type safe
When using `abi.encodeWithSignature`, it is possible to include a typo for the correct function signature.
When using `abi.encodeWithSignature` or `abi.encodeWithSelector`, it is also possible to provide parameters that are not of the correct type for the function.

To avoid these pitfalls, it would be best to use [`abi.encodeCall`](https://solidity-by-example.org/abi-encode/) instead.

*Instances (15)*:
```solidity
File: ./src/InstanceDeployer.sol

146:         bytes memory safeInitdata = abi.encodeWithSignature(

264:                 abi.encodeWithSelector(GuardManager.setGuard.selector, guard);

269:             calls3[index++].callData = abi.encodeWithSelector(

287:                 calls3[index++].callData = abi.encodeWithSelector(

293:             calls3[index++].callData = abi.encodeWithSelector(

308:                 calls3[index++].callData = abi.encodeWithSelector(

323:                 calls3[index++].callData = abi.encodeWithSelector(

386:                 abi.encodeWithSelector(IMulticall3.aggregate3.selector, calls3),

```

```solidity
File: ./src/RecoverySpell.sol

260:             calls3[index++].callData = abi.encodeWithSelector(

268:         calls3[index++].callData = abi.encodeWithSelector(

277:             calls3[index++].callData = abi.encodeWithSelector(

283:         calls3[index++].callData = abi.encodeWithSelector(

287:         calls3[index].callData = abi.encodeWithSelector(

310:                 abi.encodeWithSelector(IMulticall3.aggregate3.selector, calls3),

```

```solidity
File: ./src/views/AddressCalculation.sol

70:         bytes memory safeInitdata = abi.encodeWithSignature(

```

### <a name="NC-2"></a>[NC-2] `require()` should be used instead of `assert()`
Prior to solidity version 0.8.0, hitting an assert consumes the **remainder of the transaction's available gas** rather than returning it, as `require()`/`revert()` do. `assert()` should be avoided even past solidity version 0.8.0 as its [documentation](https://docs.soliditylang.org/en/v0.8.14/control-structures.html#panic-via-assert-and-error-via-require) states that "The assert function creates an error of type Panic(uint256). ... Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix. Additionally, a require statement (or a custom error) are more friendly in terms of understanding what happened."

*Instances (13)*:
```solidity
File: ./src/InstanceDeployer.sol

218:         assert(Safe(payable(walletInstance.safe)).isOwner(address(this)));

219:         assert(Safe(payable(walletInstance.safe)).getOwners().length == 1);

247:         assert(address(walletInstance.timelock).code.length != 0);

248:         assert(address(walletInstance.safe).code.length != 0);

332:             assert(calls3.length == index);

```

```solidity
File: ./src/Timelock.sol

674:         assert(_liveProposals.remove(id));

696:             assert(_liveProposals.remove(id));

914:                 assert(indexCheck.dataHashes.add(dataHashes[i]));

915:                 assert(lastIndexCheck.dataHashes.remove(dataHashes[i]));

1215:             assert(indexCheck.dataHashes.remove(removedDataHashes[i]));

1229:                 assert(indexCheck.dataHashes.add(dataHashes[i]));

1230:                 assert(lastIndexCheck.dataHashes.remove(dataHashes[i]));

1278:                 assert(removedCalldataCheck.dataHashes.remove(dataHashes[i]));

```

### <a name="NC-3"></a>[NC-3] Use `string.concat()` or `bytes.concat()` instead of `abi.encodePacked`
Solidity version 0.8.4 introduces `bytes.concat()` (vs `abi.encodePacked(<bytes>,<bytes>)`)

Solidity version 0.8.12 introduces `string.concat()` (vs `abi.encodePacked(<str>,<str>), which catches concatenation errors (in the event of a `bytes` data mixed in the concatenation)`)

*Instances (10)*:
```solidity
File: ./src/InstanceDeployer.sol

193:                 abi.encodePacked(keccak256(safeInitdata), creationSalt)

```

```solidity
File: ./src/RecoverySpell.sol

138:             abi.encodePacked(

```

```solidity
File: ./src/TimelockFactory.sol

43:                 salt: keccak256(abi.encodePacked(params.salt, msg.sender))

```

```solidity
File: ./src/utils/Create2Helper.sol

20:                     abi.encodePacked(

25:                             abi.encodePacked(creationCode, constructorParams)

42:                     abi.encodePacked(

47:                             abi.encodePacked(

```

```solidity
File: ./src/views/AddressCalculation.sol

110:                 abi.encodePacked(keccak256(safeInitdata), creationSalt)

120:                         abi.encodePacked(

150:             abi.encodePacked(instance.timelockParams.salt, instanceDeployer)

```

### <a name="NC-4"></a>[NC-4] `constant`s should be defined rather than using magic numbers
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*Instances (12)*:
```solidity
File: ./src/BytesHelper.sol

12:         require(toSlice.length >= 4, "No function signature");

23:         require(toSlice.length >= 32, "Length less than 32 bytes");

```

```solidity
File: ./src/InstanceDeployer.sol

253:             2 + instance.recoverySpells.length + instance.owners.length

350:                 mstore(0x40, add(ptr, 97))

355:                 mstore(ptr, 65)

```

```solidity
File: ./src/RecoverySpellFactory.sol

134:         require(delay <= 365 days, "RecoverySpell: Delay must be lte a year");

```

```solidity
File: ./src/Timelock.sol

1047:             startIndex >= 4, "CalldataList: Start index must be greater than 3"

1071:                 startIndex == 4,

1072:                 "CalldataList: End index equals start index only when 4"

```

```solidity
File: ./src/deploy/SystemDeploy.s.sol

24:         chainIds[1] = 8453;

25:         chainIds[2] = 84532;

26:         chainIds[3] = 11155420;

```

### <a name="NC-5"></a>[NC-5] Control structures do not follow the Solidity Style Guide
See the [control structures](https://docs.soliditylang.org/en/latest/style-guide.html#control-structures) section of the Solidity Style Guide

*Instances (2)*:
```solidity
File: ./src/Timelock.sol

1076:                 "CalldataList: Add wildcard only if no existing check"

1094:                 if (

```

### <a name="NC-6"></a>[NC-6] Default Visibility for constants
Some constants are using the default visibility. For readability, consider explicitly declaring them as `internal`.

*Instances (3)*:
```solidity
File: ./src/utils/Constants.sol

4: uint256 constant _DONE_TIMESTAMP = uint256(1);

7: uint256 constant MIN_DELAY = 1 days;

10: uint256 constant MAX_DELAY = 30 days;

```

### <a name="NC-7"></a>[NC-7] Functions should not be longer than 50 lines
Overly complex code can make understanding functionality more difficult, try to further modularize your code to ensure readability 

*Instances (39)*:
```solidity
File: ./src/BytesHelper.sol

7:     function getFunctionSignature(bytes memory toSlice)

35:     function sliceBytes(bytes memory toSlice, uint256 start, uint256 end)

```

```solidity
File: ./src/ConfigurablePause.sol

104:     function _updatePauseDuration(uint128 newPauseDuration) internal {

123:     function _setPauseTime(uint128 newPauseStartTime) internal {

134:     function _grantGuardian(address newPauseGuardian) internal {

```

```solidity
File: ./src/Guard.sol

74:     function checkAfterExecution(bytes32, bool) external pure {}

```

```solidity
File: ./src/InstanceDeployer.sol

139:     function createSystemInstance(NewInstance memory instance)

```

```solidity
File: ./src/RecoverySpell.sol

130:     function getOwners() external view returns (address[] memory) {

136:     function getDigest() public view returns (bytes32) {

```

```solidity
File: ./src/Timelock.sol

360:     function getAllProposals() external view returns (bytes32[] memory) {

365:     function atIndex(uint256 index) external view returns (bytes32) {

371:     function positionOf(bytes32 value) external view returns (uint256) {

392:     function isOperation(bytes32 id) public view returns (bool) {

399:     function isOperationReady(bytes32 id) public view returns (bool) {

407:     function isOperationDone(bytes32 id) public view returns (bool) {

413:     function isOperationExpired(bytes32 id) public view returns (bool) {

453:     function getCalldataChecks(address contractAddress, bytes4 selector)

475:     function checkCalldata(address contractAddress, bytes memory data)

657:     function cancel(bytes32 id) external onlySafe whenNotPaused {

671:     function cleanup(bytes32 id) external whenNotPaused {

774:     function revokeRole(bytes32 role, address account)

787:     function renounceRole(bytes32 role, address account)

802:     function revokeHotSigner(address deprecatedHotSigner) external onlySafe {

815:     function setGuardian(address newGuardian) public onlyTimelock {

960:     function updateDelay(uint256 newDelay) external onlyTimelock {

972:     function updateExpirationPeriod(uint256 newPeriod) external onlyTimelock {

982:     function updatePauseDuration(uint128 newPauseDuration)

999:     function _schedule(bytes32 id, uint256 delay) private {

1021:     function _execute(address target, uint256 value, bytes calldata data)

1252:     function _removeAllCalldataChecks(address contractAddress, bytes4 selector)

1295:     function onERC721Received(address, address, uint256, bytes memory)

1305:     function onERC1155Received(address, address, uint256, uint256, bytes memory)

```

```solidity
File: ./src/TimelockFactory.sol

37:     function createTimelock(address safe, DeploymentParams memory params)

57:     function timelockCreationCode() external pure returns (bytes memory) {

```

```solidity
File: ./src/deploy/SystemDeploy.s.sol

30:     function name() public pure override returns (string memory) {

34:     function description() public pure override returns (string memory) {

```

```solidity
File: ./src/utils/Create2Helper.sol

34: function calculateCreate2Address(Create2Params memory params)

```

```solidity
File: ./src/views/AddressCalculation.sol

28:     function calculateAddress(NewInstance memory instance)

62:     function calculateAddressUnsafe(NewInstance memory instance)

```

### <a name="NC-8"></a>[NC-8] Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor
If a function is supposed to be access-controlled, a `modifier` should be used instead of a `require/if` statement for more readability.

*Instances (2)*:
```solidity
File: ./src/Guard.sol

56:         if (to == msg.sender) {

```

```solidity
File: ./src/Timelock.sol

339:         require(msg.sender == safe, "Timelock: caller is not the safe");

```

### <a name="NC-9"></a>[NC-9] `address`s shouldn't be hard-coded
It is often better to declare `address`es as `immutable`, and assign them via constructor arguments. This allows the code to remain the same across deployments on different networks, and avoids recompilation when addresses need to change.

*Instances (1)*:
```solidity
File: ./src/RecoverySpell.sol

70:         0xcA11bde05977b3631167028862bE2a173976CA11;

```

### <a name="NC-10"></a>[NC-10] Variable names that consist of all capital letters should be reserved for `constant`/`immutable` variables
If the variable needs to be different based on which class it comes from, a `view`/`pure` *function* should be used instead (e.g. like [this](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/76eee35971c2541585e05cbf258510dda7b2fbc6/contracts/token/ERC20/extensions/draft-IERC20Permit.sol#L59)).

*Instances (12)*:
```solidity
File: ./src/BytesHelper.sol

1: pragma solidity 0.8.25;

```

```solidity
File: ./src/ConfigurablePause.sol

1: pragma solidity 0.8.25;

```

```solidity
File: ./src/Guard.sol

1: pragma solidity 0.8.25;

```

```solidity
File: ./src/InstanceDeployer.sol

1: pragma solidity 0.8.25;

```

```solidity
File: ./src/RecoverySpell.sol

1: pragma solidity 0.8.25;

```

```solidity
File: ./src/RecoverySpellFactory.sol

1: pragma solidity 0.8.25;

```

```solidity
File: ./src/Timelock.sol

1: pragma solidity 0.8.25;

```

```solidity
File: ./src/TimelockFactory.sol

1: pragma solidity 0.8.25;

```

```solidity
File: ./src/deploy/SystemDeploy.s.sol

1: pragma solidity 0.8.25;

```

```solidity
File: ./src/utils/Constants.sol

1: pragma solidity 0.8.25;

```

```solidity
File: ./src/utils/Create2Helper.sol

1: pragma solidity 0.8.25;

```

```solidity
File: ./src/views/AddressCalculation.sol

1: pragma solidity 0.8.25;

```

### <a name="NC-11"></a>[NC-11] Avoid the use of sensitive terms
Use [alternative variants](https://www.zdnet.com/article/mysql-drops-master-slave-and-blacklist-whitelist-terminology/), e.g. allowlist/denylist instead of whitelist/blacklist

*Instances (2)*:
```solidity
File: ./src/Timelock.sol

715:     function executeWhitelisted(

733:     function executeWhitelistedBatch(

```

### <a name="NC-12"></a>[NC-12] Use Underscores for Number Literals (add an underscore every 3 digits)

*Instances (3)*:
```solidity
File: ./src/deploy/SystemDeploy.s.sol

24:         chainIds[1] = 8453;

25:         chainIds[2] = 84532;

26:         chainIds[3] = 11155420;

```

### <a name="NC-13"></a>[NC-13] Variables need not be initialized to zero
The default value for variables is zero, so initializing them to zero is superfluous.

*Instances (28)*:
```solidity
File: ./src/BytesHelper.sol

53:         for (uint256 i = 0; i < length; i++) {

```

```solidity
File: ./src/InstanceDeployer.sol

256:             uint256 index = 0;

258:             for (uint256 i = 0; i < calls3.length; i++) {

275:             for (uint256 i = 0; i < instance.recoverySpells.length; i++) {

```

```solidity
File: ./src/RecoverySpell.sol

196:         for (uint256 i = 0; i < owners.length; i++) {

214:         for (uint256 i = 0; i < v.length; i++) {

254:         uint256 index = 0;

259:         for (uint256 i = 0; i < existingOwnersLength - 1; i++) {

291:         for (uint256 i = 0; i < calls3.length; i++) {

```

```solidity
File: ./src/RecoverySpellFactory.sol

59:         for (uint256 i = 0; i < owners.length; i++) {

97:         for (uint256 i = 0; i < owners.length; i++) {

135:         for (uint256 i = 0; i < owners.length; i++) {

```

```solidity
File: ./src/Timelock.sol

306:         for (uint256 i = 0; i < hotSigners.length; i++) {

461:         for (uint256 i = 0; i < indexes.length; i++) {

488:         for (uint256 i = 0; i < calldataChecks.length; i++) {

572:         for (uint256 i = 0; i < targets.length; i++) {

639:         for (uint256 i = 0; i < targets.length; i++) {

692:         for (uint256 i = 0; i < proposals.length; i++) {

743:         for (uint256 i = 0; i < targets.length; i++) {

913:             for (uint256 i = 0; i < dataHashes.length; i++) {

949:         for (uint256 i = 0; i < contractAddresses.length; i++) {

1093:             for (uint256 i = 0; i < indexes.length; i++) {

1133:         for (uint256 i = 0; i < data.length; i++) {

1178:         for (uint256 i = 0; i < contractAddresses.length; i++) {

1214:         for (uint256 i = 0; i < removedDataHashes.length; i++) {

1228:             for (uint256 i = 0; i < dataHashes.length; i++) {

1277:             for (uint256 i = 0; i < dataHashes.length; i++) {

```

```solidity
File: ./src/views/AddressCalculation.sol

36:         for (uint256 i = 0; i < instance.recoverySpells.length; i++) {

```


## Low Issues


| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) | `domainSeparator()` isn't protected against replay attacks in case of a future chain split  | 1 |
| [L-2](#L-2) | External call recipient may consume all transaction gas | 12 |
| [L-3](#L-3) | Initializers could be front-run | 2 |
| [L-4](#L-4) | Signature use at deadlines should be allowed | 2 |
| [L-5](#L-5) | Upgradeable contract not initialized | 5 |
### <a name="L-1"></a>[L-1] `domainSeparator()` isn't protected against replay attacks in case of a future chain split 
Severity: Low.
Description: See <https://eips.ethereum.org/EIPS/eip-2612#security-considerations>.
Remediation: Consider using the [implementation](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/cryptography/EIP712.sol#L77-L90) from OpenZeppelin, which recalculates the domain separator if the current `block.chainid` is not the cached chain ID.
Past occurrences of this issue:
- [Reality Cards Contest](https://github.com/code-423n4/2021-06-realitycards-findings/issues/166)
- [Swivel Contest](https://github.com/code-423n4/2021-09-swivel-findings/issues/98)
- [Malt Finance Contest](https://github.com/code-423n4/2021-11-malt-findings/issues/349)

*Instances (1)*:
```solidity
File: ./src/RecoverySpell.sol

140:                 _domainSeparatorV4(),

```

### <a name="L-2"></a>[L-2] External call recipient may consume all transaction gas
There is no limit specified on the amount of gas used, so the recipient can use up all of the transaction's gas, causing it to revert. Use `addr.call{gas: <amount>}("")` or [this](https://github.com/nomad-xyz/ExcessivelySafeCall) library instead.

*Instances (12)*:
```solidity
File: ./src/InstanceDeployer.sol

263:             calls3[index++].callData =

269:             calls3[index++].callData = abi.encodeWithSelector(

287:                 calls3[index++].callData = abi.encodeWithSelector(

293:             calls3[index++].callData = abi.encodeWithSelector(

308:                 calls3[index++].callData = abi.encodeWithSelector(

323:                 calls3[index++].callData = abi.encodeWithSelector(

```

```solidity
File: ./src/RecoverySpell.sol

260:             calls3[index++].callData = abi.encodeWithSelector(

268:         calls3[index++].callData = abi.encodeWithSelector(

277:             calls3[index++].callData = abi.encodeWithSelector(

283:         calls3[index++].callData = abi.encodeWithSelector(

287:         calls3[index].callData = abi.encodeWithSelector(

```

```solidity
File: ./src/Timelock.sol

1024:         (bool success,) = target.call{value: value}(data);

```

### <a name="L-3"></a>[L-3] Initializers could be front-run
Initializers could be front-run, allowing an attacker to either set their own values, take ownership of the contract, and in the best case forcing a re-deployment

*Instances (2)*:
```solidity
File: ./src/InstanceDeployer.sol

238:         walletInstance.timelock.initialize(

```

```solidity
File: ./src/Timelock.sol

316:     function initialize(

```

### <a name="L-4"></a>[L-4] Signature use at deadlines should be allowed
According to [EIP-2612](https://github.com/ethereum/EIPs/blob/71dc97318013bf2ac572ab63fab530ac9ef419ca/EIPS/eip-2612.md?plain=1#L58), signatures used on exactly the deadline timestamp are supposed to be allowed. While the signature may or may not be used for the exact EIP-2612 use case (transfer approvals), for consistency's sake, all deadlines should follow this semantic. If the timestamp is an expiration rather than a deadline, consider whether it makes more sense to include the expiration timestamp as a valid timestamp, as is done for deadlines.

*Instances (2)*:
```solidity
File: ./src/Timelock.sol

402:         return timestamp > _DONE_TIMESTAMP && timestamp <= block.timestamp

403:             && timestamp + expirationPeriod > block.timestamp;

```

### <a name="L-5"></a>[L-5] Upgradeable contract not initialized
Upgradeable contracts are initialized via an initializer function rather than by a constructor. Leaving such a contract uninitialized may lead to it being taken over by a malicious user

*Instances (5)*:
```solidity
File: ./src/InstanceDeployer.sol

238:         walletInstance.timelock.initialize(

```

```solidity
File: ./src/Timelock.sol

97:     bool public initialized;

316:     function initialize(

323:         require(!initialized, "Timelock: already initialized");

324:         initialized = true;

```


## Medium Issues


| |Issue|Instances|
|-|:-|:-:|
| [M-1](#M-1) | Centralization Risk for trusted owners | 7 |
| [M-2](#M-2) | Direct `supportsInterface()` calls may cause caller to revert | 1 |
### <a name="M-1"></a>[M-1] Centralization Risk for trusted owners

#### Impact:
Contracts have owners with privileged rights to perform admin tasks and need to be trusted to not perform malicious updates or drain funds.

*Instances (7)*:
```solidity
File: ./src/Timelock.sol

4:     AccessControl,

70:     AccessControlEnumerable,

719:     ) external payable onlyRole(HOT_SIGNER_ROLE) whenNotPaused {

737:     ) external payable onlyRole(HOT_SIGNER_ROLE) whenNotPaused {

765:         override(AccessControl, IAccessControl)

776:         override(AccessControl, IAccessControl)

789:         override(AccessControl, IAccessControl)

```

### <a name="M-2"></a>[M-2] Direct `supportsInterface()` calls may cause caller to revert
Calling `supportsInterface()` on a contract that doesn't implement the ERC-165 standard will result in the call reverting. Even if the caller does support the function, the contract may be malicious and consume all of the transaction's available gas. Call it via a low-level [staticcall()](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/f959d7e4e6ee0b022b41e5b644c79369869d8411/contracts/utils/introspection/ERC165Checker.sol#L119), with a fixed amount of gas, and check the return code, or use OpenZeppelin's [`ERC165Checker.supportsInterface()`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/f959d7e4e6ee0b022b41e5b644c79369869d8411/contracts/utils/introspection/ERC165Checker.sol#L36-L39).

*Instances (1)*:
```solidity
File: ./src/Timelock.sol

386:             || super.supportsInterface(interfaceId);

```
