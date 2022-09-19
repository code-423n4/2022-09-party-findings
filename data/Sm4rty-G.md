

## 1. Pre-increment costs less gas as compared to Post-increment :

++i costs less gas as compared to i++ for unsigned integer, as per-increment is cheaper(its about 5 gas per iteration cheaper)

i++ increments i and returns initial value of i. Which means

```
uint i =  1;
i++; // ==1 but i ==2
```

But ++i returns the actual incremented value:

```
uint i = 1;
++i; // ==2 and i ==2 , no need for temporary variable here
```

In the first case, the compiler has create a temporary variable (when used) for returning 1 instead of 2.

### Instances:

```
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol:62:        for (uint256 i; i < hosts.length; i++) {
``` 

### Recommendations:
Change post-increment to pre-increment.

-----

## 2. ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas [PER LOOP](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)

### Instances:

```
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol:62:        for (uint256 i; i < hosts.length; i++) {
``` 

### Reference:

[https://code4rena.com/reports/2022-05-cally#g-11-ii-should-be-uncheckediuncheckedi-when-it-is-not-possible-for-them-to-overflow-as-is-the-case-when-used-in-for--and-while-loops](https://code4rena.com/reports/2022-05-cally#g-11-ii-should-be-uncheckediuncheckedi-when-it-is-not-possible-for-them-to-overflow-as-is-the-case-when-used-in-for--and-while-loops)

-----


## 3. An array's length should be cached to save gas in for-loops

Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.
Caching the array length in the stack saves around 3 gas per iteration.

### Instances:

```
party-contracts-c4/contracts/party/PartyGovernance.sol:306:        for (uint256 i=0; i < opts.hosts.length; ++i) {
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol:62:        for (uint256 i; i < hosts.length; i++) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:180:        for (uint256 i = 0; i < contributors.length; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:300:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol:230:        for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol:239:        for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4/contracts/proposals/LibProposal.sol:14:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/proposals/LibProposal.sol:32:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:291:            for (uint256 i = 0; i < fees.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol:52:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol:61:        for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol:78:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
``` 


### Remediation:
Here, I suggest storing the array's length in a variable before the for-loop, and use it instead.

-----


## 4. Usage of assert() instead of require()

Between solc 0.4.10 and 0.8.0, require() used REVERT (0xfd) opcode which refunded the remaining gas on failure while assert() used INVALID (0xfe) opcode which consumed all the supplied gas. (see [https://docs.soliditylang.org/en/v0.8.1/control-structures.html#error-handling-assert-require-revert-and-exceptions](https://docs.soliditylang.org/en/v0.8.1/control-structures.html#error-handling-assert-require-revert-and-exceptions)).
require() should be used for checking error conditions on inputs and return values while assert() should be used for invariant checking (properly functioning code should never reach a failing assert statement, unless there is a bug in your contract you should fix).
From the current usage of assert, my guess is that they can be replaced with require unless a Panic really is intended.

### Instances:

```
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol:30:            assert(false);
party-contracts-c4/contracts/party/PartyGovernance.sol:504:        assert(tokenType == ITokenDistributor.TokenType.Erc20);
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol:179:        assert(false); // Will not be reached.
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol:166:        assert(false); // Will not be reached.
party-contracts-c4/contracts/distribution/TokenDistributor.sol:385:            assert(tokenType == TokenType.Erc20);
party-contracts-c4/contracts/distribution/TokenDistributor.sol:411:        assert(tokenType == TokenType.Erc20);
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol:67:        assert(vault.balanceOf(address(this)) == supply);
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol:142:                assert(currentInProgressProposalId == 0);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:221:        assert(step == ListOnOpenseaStep.ListedOnOpenSea);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:302:        assert(SEAPORT.validate(orders));
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol:120:        assert(step == ZoraStep.ListedOnZora);
``` 

### References:

[https://code4rena.com/reports/2022-05-bunker/#g-08-usage-of-assert-instead-of-require](https://code4rena.com/reports/2022-05-bunker/#g-08-usage-of-assert-instead-of-require)

-----

## 5. Use Shift Right/Left instead of Division/Multiplication 2X

A division by 2 can be calculated by shifting one to the right.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity’s division operation also includes a division-by-0 prevention which is bypassed using shifting.

I suggest replacing  `/ 2` with  `>> 1` here:

### Instances

```
party-contracts-c4/contracts/party/PartyGovernance.sol:434:            uint256 mid = (low + high) / 2;

``` 
### References:

[https://code4rena.com/reports/2022-04-badger-citadel/#g-32-shift-right-instead-of-dividing-by-2](https://code4rena.com/reports/2022-04-badger-citadel/#g-32-shift-right-instead-of-dividing-by-2)


-----


## 6. Strict inequalities (>) are more expensive than non-strict ones (>=)

Strict inequalities (>) are more expensive than non-strict ones (>=). This is due to some supplementary checks (ISZERO, 3 gas. I suggest using >= instead of > to avoid some opcodes here:

### Instances:

```
party-contracts-c4/contracts/party/PartyGovernance.sol:862:                (hintIndex == snapsLength - 1 || snaps[hintIndex+1].timestamp > timestamp)
party-contracts-c4/contracts/distribution/TokenDistributor.sol:167:        amountClaimed = amountClaimed > remainingMemberSupply
``` 
### References:

[https://code4rena.com/reports/2022-04-badger-citadel/#g-31--is-cheaper-than](https://code4rena.com/reports/2022-04-badger-citadel/#g-31--is-cheaper-than)


-----

## 7. x += y costs more gas than x = x + y for state variables

### Instances:

```
party-contracts-c4/contracts/party/PartyGovernance.sol:595:        values.votes += votingPower;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:243:            ethContributed += contributions[i].amount;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:352:                    ethOwed += c.amount;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:355:                    ethUsed += c.amount;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:359:                    ethUsed += partialEthUsed;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:374:            votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:411:            totalContributions += amount;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:427:                    lastContribution.amount += amount;
party-contracts-c4/contracts/party/PartyGovernance.sol:959:                newDelegateDelegatedVotingPower -= oldSnap.intrinsicVotingPower;
party-contracts-c4/contracts/distribution/TokenDistributor.sol:381:        _storedBalances[balanceId] -= amount;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol:72:            ethAvailable -= calls[i].value;
``` 
### References:

[https://github.com/code-423n4/2022-05-backd-findings/issues/108](https://github.com/code-423n4/2022-05-backd-findings/issues/108)


-----

## 8. Variables: No need to explicitly initialize variables with default values

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

We can use `uint number;` instead of `uint number = 0;`

### Instances:

```
party-contracts-c4/contracts/party/PartyGovernance.sol:432:        uint256 low = 0;
``` 
### Recommendation:

I suggest removing explicit initializations for default values.


-----

## 9. Expressions for constant values such as a call to keccak256(), should use immutable rather than constant

This results in the keccak operation being performed whenever the variable is used, increasing gas costs relative to just storing the output hash. Changing to immutable will only perform hashing on contract deployment which will save gas.

### Instances:

```
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol:80:    uint256 private constant _STORAGE_SLOT = uint256(keccak256('ProposalExecutionEngine.Storage'));
party-contracts-c4/contracts/proposals/ProposalStorage.sol:19:    uint256 private constant SHARED_STORAGE_SLOT = uint256(keccak256("ProposalStorage.SharedProposalStorage"));
``` 
### References:

[https://github.com/code-423n4/2021-10-slingshot-findings/issues/3](https://github.com/code-423n4/2021-10-slingshot-findings/issues/3)


-----

## 10. abi.encode() is less efficient than abi.encodepacked()

Use abi.encodepacked() instead of abi.encode()

### Instances:

```
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol:23:        abi.encode(s, r).rawRevert();
party-contracts-c4/contracts/party/PartyGovernance.sol:400:        // keccak256(abi.encode(
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:164:                    return abi.encode(ListOnOpenseaStep.ListedOnZora, ZoraProgressData({
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:219:            return abi.encode(ListOnOpenseaStep.ListedOnOpenSea, orderHash, expiry);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol:115:            return abi.encode(ZoraStep.ListedOnZora, ZoraProgressData({
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol:23:        abi.encode(s, r).rawRevert();
party-contracts-c4/contracts/party/PartyGovernance.sol:400:        // keccak256(abi.encode(
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:164:                    return abi.encode(ListOnOpenseaStep.ListedOnZora, ZoraProgressData({
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:219:            return abi.encode(ListOnOpenseaStep.ListedOnOpenSea, orderHash, expiry);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol:115:            return abi.encode(ZoraStep.ListedOnZora, ZoraProgressData({
``` 
### Reference:

[https://code4rena.com/reports/2022-06-notional-coop#15-abiencode-is-less-efficient-than-abiencodepacked](https://code4rena.com/reports/2022-06-notional-coop#15-abiencode-is-less-efficient-than-abiencodepacked)


-----


## 11. Use calldata instead of memory in external functions

Use calldata instead of memory in external functions to save gas.

### Instances:

```
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol:18:    function delegateCallAndRevert(address impl, bytes memory callData) external {
``` 
### Reference:

[https://code4rena.com/reports/2022-04-badger-citadel/#g-12-stakedcitadeldepositall-use-calldata-instead-of-memory](https://code4rena.com/reports/2022-04-badger-citadel/#g-12-stakedcitadeldepositall-use-calldata-instead-of-memory)


-----

## 12. Using bools for storage incurs overhead

```
// Booleans are more expensive than uint256 or any type that takes up a full
// word because each write operation emits an extra SLOAD to first read the
// slot's contents, replace the bits taken up by the boolean, and then write
// back. This is the compiler's defense against contract upgrades and
// pointer aliasing, and it cannot be disabled.
```

[Refer Here](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27) Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

### Instances:

```
party-contracts-c4/contracts/globals/Globals.sol:12:    mapping(uint256 => mapping(bytes32 => bool)) private _includedWordValues;
party-contracts-c4/contracts/party/PartyGovernance.sol:108:        bool isDelegated;
party-contracts-c4/contracts/party/PartyGovernance.sol:148:        mapping (address => bool) hasVoted;
party-contracts-c4/contracts/party/PartyGovernance.sol:197:    bool public emergencyExecuteDisabled;
party-contracts-c4/contracts/party/PartyGovernance.sol:207:    mapping(address => bool) public isHost;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol:61:        bool isHost;
party-contracts-c4/contracts/distribution/TokenDistributor.sol:28:        bool wasFeeClaimed;
party-contracts-c4/contracts/distribution/TokenDistributor.sol:30:        mapping(uint256 => bool) hasPartyTokenClaimed;
party-contracts-c4/contracts/distribution/TokenDistributor.sol:62:    bool public emergencyActionsDisabled;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol:50:        bool[] memory hadPreciouses = new bool[](params.preciousTokenIds.length);
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol:15:        bool approved;
``` 
### References:

[https://code4rena.com/reports/2022-06-notional-coop#8-using-bools-for-storage-incurs-overhead](https://code4rena.com/reports/2022-06-notional-coop#8-using-bools-for-storage-incurs-overhead)


-----

## 13. Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html)
Use a larger size then downcast where needed

### Instances:

```

party-contracts-c4/contracts/party/PartyGovernance.sol:81:        uint16 passThresholdBps;
party-contracts-c4/contracts/party/PartyGovernance.sol:85:        uint16 feeBps;
party-contracts-c4/contracts/party/PartyGovernance.sol:95:        uint16 passThresholdBps;
party-contracts-c4/contracts/party/PartyGovernance.sol:199:    uint16 public feeBps;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol:36:        uint16 splitBps;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol:56:        uint16 splitBps;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol:40:        uint16 splitBps;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol:39:        uint16 splitBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:43:        uint16 passThresholdBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:45:        uint16 feeBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:55:        uint16 splitBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:104:    uint16 public splitBps;
party-contracts-c4/contracts/distribution/ITokenDistributor.sol:65:        uint16 feeBps
party-contracts-c4/contracts/distribution/ITokenDistributor.sol:85:        uint16 feeBps
party-contracts-c4/contracts/distribution/TokenDistributor.sol:40:        uint16 feeBps;
party-contracts-c4/contracts/distribution/TokenDistributor.sol:101:        uint16 feeBps
party-contracts-c4/contracts/distribution/TokenDistributor.sol:122:        uint16 feeBps
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol:25:        uint8 curatorFeePercentage;
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol:44:        uint8 curatorFeePercentages,
``` 
### References:

[https://code4rena.com/reports/2022-04-phuture#g-14-usage-of-uintsints-smaller-than-32-bytes-256-bits-incurs-overhead](https://code4rena.com/reports/2022-04-phuture#g-14-usage-of-uintsints-smaller-than-32-bytes-256-bits-incurs-overhead)


-----

## 14. Use a more recent version of solidity

Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

### Instances:

```
party-contracts-c4/contracts/globals/Globals.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/globals/IGlobals.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/Proxy.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibAddress.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibERC20Compat.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibRawResult.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/EIP165.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/Implementation.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibSafeERC721.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibSafeCast.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyGovernance.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyFactory.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/party/IPartyFactory.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/party/Party.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/market-wrapper/IMarketWrapper.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/gatekeepers/IGateKeeper.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/ITokenDistributorParty.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/ITokenDistributor.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/TokenDistributor.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/IProposalExecutionEngine.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/LibProposal.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalStorage.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/IOpenseaExchange.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/IOpenseaConduitController.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/FractionalV1.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC721Receiver.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/ERC721Receiver.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/ERC1155Receiver.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC20.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC721.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC1155.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol:2:pragma solidity ^0.8;
``` 
### References:

[https://code4rena.com/reports/2022-06-notional-coop/#10-use-a-more-recent-version-of-solidity](https://code4rena.com/reports/2022-06-notional-coop/#10-use-a-more-recent-version-of-solidity)


-----