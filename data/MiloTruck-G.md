# Gas Optimizations

|No.|Issue|Instances|
|:-|:-|:-:|
|1|For-loops: Index initialized with default value|13|
|2|For-Loops: Cache array length outside of loops|12|
|3|For-Loops: Index increments can be left unchecked|14|
|4|Arithmetics: `++i` costs less gas compared to `i++` or `i += 1`|1|
|5|Arithmetics: Use Shift Right/Left instead of Division/Multiplication if possible|1|
|6|Visibility: `public` functions can be set to `external`|3|
|7|Errors: Use custom errors instead of revert strings|3|
|8|Unnecessary initialization of variables with default values|1|
|9|State variables should be cached in stack variables rather than re-reading them from storage|11|
|10|`<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables|1|
|11|Using `bool` for storage incurs overhead|5|
|12|Use `calldata` instead of `memory` for read-only arguments in external functions|16|
|13|Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead|110|
|14|Not using the named return variables when a function returns, wastes deployment gas|50|
|15|`abi.encode()` is less efficient than `abi.encodePacked()`|4|
|16|`internal` functions only called once can be inlined to save gas|4|

Total: **249** instances over **16** issues

## For-loops: Index initialized with default value
Uninitialized `uint` variables are assigned with a default value of `0`. 

Thus, in for-loops, explicitly initializing an index with `0` costs unnecesary gas. For example, the following code:
```js
for (uint256 i = 0; i < length; ++i) {
```
can be written without explicitly setting the index to `0`:  
```js
for (uint256 i; i < length; ++i) {
```

_There are **13** instances of this issue:_  
```solidity
contracts/proposals/LibProposal.sol:
  14:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
  32:        for (uint256 i = 0; i < preciousTokens.length; ++i) {

contracts/proposals/ArbitraryCallsProposal.sol:
  52:        for (uint256 i = 0; i < hadPreciouses.length; ++i) {
  61:        for (uint256 i = 0; i < calls.length; ++i) {
  78:        for (uint256 i = 0; i < hadPreciouses.length; ++i) {

contracts/proposals/ListOnOpenseaProposal.sol:
 291:        for (uint256 i = 0; i < fees.length; ++i) {

contracts/party/PartyGovernance.sol:
 306:        for (uint256 i=0; i < opts.hosts.length; ++i) {

contracts/distribution/TokenDistributor.sol:
 230:        for (uint256 i = 0; i < infos.length; ++i) {
 239:        for (uint256 i = 0; i < infos.length; ++i) {

contracts/crowdfund/Crowdfund.sol:
 180:        for (uint256 i = 0; i < contributors.length; ++i) {
 242:        for (uint256 i = 0; i < numContributions; ++i) {
 300:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
 348:        for (uint256 i = 0; i < numContributions; ++i) {
```

## For-Loops: Cache array length outside of loops
Reading an array's length at each iteration has the following gas overheads:
* storage arrays incur a Gwarmaccess (**100 gas**)
* memory arrays use `mload` (**3 gas**)
* calldata arrays use `calldataload` (**3 gas**)

Caching the length changes each of these to a `DUP<N>` (**3 gas**), and gets rid of the extra `DUP<N>` needed to store the stack offset. This would save around **3 gas** per iteration.

For example:
```js
for (uint256 i; i < arr.length; ++i) {}
```
can be changed to:
```js
uint256 len = arr.length;
for (uint256 i; i < len; ++i) {}
```

_There are **12** instances of this issue:_  
```solidity
contracts/proposals/LibProposal.sol:
  14:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
  32:        for (uint256 i = 0; i < preciousTokens.length; ++i) {

contracts/proposals/ArbitraryCallsProposal.sol:
  52:        for (uint256 i = 0; i < hadPreciouses.length; ++i) {
  61:        for (uint256 i = 0; i < calls.length; ++i) {
  78:        for (uint256 i = 0; i < hadPreciouses.length; ++i) {

contracts/proposals/ListOnOpenseaProposal.sol:
 291:        for (uint256 i = 0; i < fees.length; ++i) {

contracts/party/PartyGovernance.sol:
 306:        for (uint256 i=0; i < opts.hosts.length; ++i) {

contracts/distribution/TokenDistributor.sol:
 230:        for (uint256 i = 0; i < infos.length; ++i) {
 239:        for (uint256 i = 0; i < infos.length; ++i) {

contracts/crowdfund/Crowdfund.sol:
 180:        for (uint256 i = 0; i < contributors.length; ++i) {
 300:        for (uint256 i = 0; i < preciousTokens.length; ++i) {

contracts/crowdfund/CollectionBuyCrowdfund.sol:
  62:        for (uint256 i; i < hosts.length; i++) {
```


## For-Loops: Index increments can be left unchecked
From Solidity v0.8 onwards, all arithmetic operations come with implicit overflow and underflow checks. 

In for-loops, as it is impossible for the index to overflow, index increments can be left unchecked to save **[30-40 gas](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)** per loop iteration. 

For example, the code below:
```js
for (uint256 i; i < numIterations; ++i) {  
    // ...  
}  
```
can be changed to:
```js
for (uint256 i; i < numIterations;) {  
    // ...  
    unchecked { ++i; }  
}  
```

_There are **14** instances of this issue:_  
```solidity
contracts/proposals/LibProposal.sol:
  14:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
  32:        for (uint256 i = 0; i < preciousTokens.length; ++i) {

contracts/proposals/ArbitraryCallsProposal.sol:
  52:        for (uint256 i = 0; i < hadPreciouses.length; ++i) {
  61:        for (uint256 i = 0; i < calls.length; ++i) {
  78:        for (uint256 i = 0; i < hadPreciouses.length; ++i) {

contracts/proposals/ListOnOpenseaProposal.sol:
 291:        for (uint256 i = 0; i < fees.length; ++i) {

contracts/party/PartyGovernance.sol:
 306:        for (uint256 i=0; i < opts.hosts.length; ++i) {

contracts/distribution/TokenDistributor.sol:
 230:        for (uint256 i = 0; i < infos.length; ++i) {
 239:        for (uint256 i = 0; i < infos.length; ++i) {

contracts/crowdfund/Crowdfund.sol:
 180:        for (uint256 i = 0; i < contributors.length; ++i) {
 242:        for (uint256 i = 0; i < numContributions; ++i) {
 300:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
 348:        for (uint256 i = 0; i < numContributions; ++i) {

contracts/crowdfund/CollectionBuyCrowdfund.sol:
  62:        for (uint256 i; i < hosts.length; i++) {
```

## Arithmetics: `++i` costs less gas compared to `i++` or `i += 1`
`++i` costs less gas compared to `i++` or `i += 1` for unsigned integers, as pre-increment is cheaper (about **5 gas** per iteration). This statement is true even with the optimizer enabled.

`i++` increments `i` and returns the initial value of `i`. Which means:
```js
uint i = 1;  
i++; // == 1 but i == 2  
```
But `++i` returns the actual incremented value:
```js
uint i = 1;  
++i; // == 2 and i == 2 too, so no need for a temporary variable  
```

In the first case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`, thus it costs more gas.

The same logic applies for `--i` and `i--`.

_There is **1** instance of this issue:_  
```solidity
contracts/crowdfund/CollectionBuyCrowdfund.sol:
  62:        for (uint256 i; i < hosts.length; i++) {
```

## Arithmetics: Use Shift Right/Left instead of Division/Multiplication if possible
A division/multiplication by any number `x` being a power of 2 can be calculated by shifting `log2(x)` to the right/left.

While the `MUL` and `DIV` opcodes use 5 gas, the `SHL` and `SHR` opcodes only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

For example, the following code:
```js
uint256 b = a / 2;
uint256 c = a / 4;
uint256 d = a * 8;
```
can be changed to:
```js
uint256 b = a >> 1;
uint256 c = a >> 2;
uint256 d = a << 3;
```

_There is **1** instance of this issue:_  
```solidity
contracts/party/PartyGovernance.sol:
 434:        uint256 mid = (low + high) / 2;
```

## Visibility: `public` functions can be set to `external`
Calls to `external` functions are cheaper than `public` functions. Thus, if a function is not used internally in any contract, it should be set to `external` to save gas and improve code readability.

_There are **3** instances of this issue:_  
```solidity
contracts/crowdfund/CrowdfundFactory.sol:
  35:        function createBuyCrowdfund(
  36:            BuyCrowdfund.BuyCrowdfundOptions memory opts,
  37:            bytes memory createGateCallData
  38:        )
  39:            public
  40:            payable
  41:            returns (BuyCrowdfund inst)
  42:        {


  61:        function createAuctionCrowdfund(
  62:            AuctionCrowdfund.AuctionCrowdfundOptions memory opts,
  63:            bytes memory createGateCallData
  64:        )
  65:            public
  66:            payable
  67:            returns (AuctionCrowdfund inst)
  68:        {


  87:        function createCollectionBuyCrowdfund(
  88:            CollectionBuyCrowdfund.CollectionBuyCrowdfundOptions memory opts,
  89:            bytes memory createGateCallData
  90:        )
  91:            public
  92:            payable
  93:            returns (CollectionBuyCrowdfund inst)
  94:        {
```

## Errors: Use custom errors instead of revert strings
Since Solidity v0.8.4, custom errors can be used rather than `require` statements.

Taken from [Custom Errors in Solidity](https://blog.soliditylang.org/2021/04/21/custom-errors/):
> Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., `revert("Insufficient funds.");`), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors reduce runtime gas costs as they save about **50 gas** each time they are hit by [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Furthermore, not definiting error strings also help to reduce deployment gas costs. 

_There are **3** instances of this issue:_  
```solidity
contracts/proposals/ProposalExecutionEngine.sol:
 246:        require(proposalType != ProposalType.Invalid);
 247:        require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));

contracts/utils/ReadOnlyDelegateCall.sol:
  20:        require(msg.sender == address(this));
```

## Unnecessary initialization of variables with default values
Uninitialized variables are assigned with a default value depending on its type:
* `uint`: `0`
* `bool`: `false`
* `address`: `address(0)`

Thus, explicitly initializing a variable with its default value costs unnecesary gas. For example, the following code:
```js
bool b = false;
address c = address(0);
uint256 a = 0;
```
can be changed to:
```js
uint256 a;
bool b;
address c;
```

_There is **1** instance of this issue:_  
```solidity
contracts/party/PartyGovernance.sol:
 432:        uint256 low = 0;
```

## State variables should be cached in stack variables rather than re-reading them from storage
If a state variable is read from storage multiple times in a function, it should be cached in a stack variable.

Caching of a state variable replaces each Gwarmaccess (**100 gas**) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

_There are **11** instances of this issue:_  
`feeRecipient` in `distribute()`:
```solidity
contracts/party/PartyGovernance.sol:
 501:        { value: address(this).balance }(this, feeRecipient, feeBps);
 512:        feeRecipient,
```
`feeBps` in `distribute()`:
```solidity
contracts/party/PartyGovernance.sol:
 501:        { value: address(this).balance }(this, feeRecipient, feeBps);
 513:        feeBps
```
`mintAuthority` in `onlyMinter()`:
```solidity
contracts/party/PartyGovernanceNFT.sol:
  36:        if (msg.sender != mintAuthority) {
  37:        revert OnlyMintAuthorityError(msg.sender, mintAuthority);
```
`maximumPrice` in `_buy()`:
```solidity
contracts/crowdfund/BuyCrowdfundBase.sol:
 116:        uint96 maximumPrice_ = maximumPrice;
 118:        revert MaximumPriceError(callValue, maximumPrice);
```
`party` in `_createParty()`:
```solidity
contracts/crowdfund/Crowdfund.sol:
 271:        if (party != Party(payable(0))) {
 272:        revert PartyAlreadyExistsError(party);
```
`governanceOptsHash` in `_createParty()`:
```solidity
contracts/crowdfund/Crowdfund.sol:
 276:        if (governanceOptsHash_ != governanceOptsHash) {
 277:        revert InvalidGovernanceOptionsError(governanceOptsHash_, governanceOptsHash);
```
`gateKeeper` in `_contribute()`:
```solidity
contracts/crowdfund/Crowdfund.sol:
 392:        if (gateKeeper != IGateKeeper(address(0))) {

 394:        revert NotAllowedByGateKeeperError(
 395:            contributor,
 396:            gateKeeper,
 397:            gateKeeperId,
 398:            gateData
 399:        );
```
`gateKeeperId` in `_contribute()`:
```solidity
contracts/crowdfund/Crowdfund.sol:
 393:        if (!gateKeeper.isAllowed(contributor, gateKeeperId, gateData)) {

 394:        revert NotAllowedByGateKeeperError(
 395:            contributor,
 396:            gateKeeper,
 397:            gateKeeperId,
 398:            gateData
 399:        );
```
`splitRecipient` in `_burn()`:
```solidity
contracts/crowdfund/Crowdfund.sol:
 457:        if (contributor == splitRecipient) {
 464:        if (splitRecipient != contributor || _doesTokenExistFor(contributor)) {
```
`maximumBid` in `bid()`:
```solidity
contracts/crowdfund/AuctionCrowdfund.sol:
 172:        if (bidAmount > maximumBid) {
 173:        revert ExceedsMaximumBidError(bidAmount, maximumBid);
```
`nftTokenId` in `finalize()`:
```solidity
contracts/crowdfund/AuctionCrowdfund.sol:
 233:        if (nftContract.safeOwnerOf(nftTokenId) == address(this)) {
 248:        nftTokenId
```

## `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables
The above also applies to state variables that use `-=`.

_There is **1** instance of this issue:_  
```solidity
contracts/crowdfund/Crowdfund.sol:
 411:        totalContributions += amount;
```

## Using `bool` for storage incurs overhead
Declaring storage variables as `bool` is more expensive compared to `uint256`, as explained [here](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/ReentrancyGuard.sol#L23-L27):

> Booleans are more expensive than `uint256` or any type that takes up a full word because each write operation emits an extra `SLOAD` to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

Use `uint256(1)` and `uint256(2)` rather than true/false to avoid a Gwarmaccess ([**100 gas**](https://gist.github.com/IllIllI000/1b70014db712f8572a72378321250058)) for the extra `SLOAD`, and to avoid Gsset (**20000 gas**) when changing from 'false' to 'true', after having been 'true' in the past.

_There are **5** instances of this issue:_  
```solidity
contracts/globals/Globals.sol:
  12:        mapping(uint256 => mapping(bytes32 => bool)) private _includedWordValues;

contracts/party/PartyGovernance.sol:
 197:        bool public emergencyExecuteDisabled;
 207:        mapping(address => bool) public isHost;

contracts/distribution/TokenDistributor.sol:
  62:        bool public emergencyActionsDisabled;

contracts/crowdfund/Crowdfund.sol:
 106:        bool private _splitRecipientHasBurned;
```

## Use `calldata` instead of `memory` for read-only arguments in external functions
When an external function with a `memory` array is called, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. `60 * <mem_array>.length`). 

Using `calldata` directly instead of `memory` helps to save gas as values are read directly from `calldata` using `calldataload`, thus removing the need for such a loop in the contract code during runtime execution. 

Also, structs have the same overhead as an array of length one.

_There are **16** instances of this issue:_  
```solidity
contracts/proposals/ProposalExecutionEngine.sol:
 116:        function executeProposal(ExecuteProposalParams memory params)

contracts/utils/ReadOnlyDelegateCall.sol:
  18:        function delegateCallAndRevert(address impl, bytes memory callData) external {

contracts/party/PartyFactory.sol:
  28:        Party.PartyOptions memory opts,
  29:        IERC721[] memory preciousTokens,
  30:        uint256[] memory preciousTokenIds

contracts/party/PartyGovernance.sol:
 526:        function propose(Proposal memory proposal, uint256 latestSnapIndex)
 655:        Proposal memory proposal,
 656:        IERC721[] memory preciousTokens,
 657:        uint256[] memory preciousTokenIds,

contracts/party/Party.sol:
  33:        function initialize(PartyInitData memory initData)

contracts/crowdfund/BuyCrowdfund.sol:
  68:        function initialize(BuyCrowdfundOptions memory opts)
 102:        FixedGovernanceOpts memory governanceOpts

contracts/crowdfund/CollectionBuyCrowdfund.sol:
  83:        function initialize(CollectionBuyCrowdfundOptions memory opts)
 118:        FixedGovernanceOpts memory governanceOpts

contracts/crowdfund/AuctionCrowdfund.sol:
 110:        function initialize(AuctionCrowdfundOptions memory opts)
 196:        function finalize(FixedGovernanceOpts memory governanceOpts)
```

## Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead
As seen from [here](https://docs.soliditylang.org/en/v0.8.15/internals/layout_in_storage.html):
> When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

However, this does not apply to storage values as using reduced-size types might be beneficial to pack multiple elements into a single storage slot. Thus, where appropriate, use `uint256`/`int256` and downcast when needed.

_There are **110** instances of this issue:_  
```solidity
contracts/proposals/ListOnZoraProposal.sol:
  34:        uint40 timeout;
  36:        uint40 duration;
  50:        uint40 duration,
  51:        uint40 timeoutTime
  94:        uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MIN_AUCTION_DURATION));
  95:        uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_DURATION));
 102:        uint40 maxTimeout = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_TIMEOUT));
 133:        uint40 timeout,
 135:        uint40 duration,
 166:        uint40 minExpiry,

contracts/proposals/ListOnOpenseaProposal.sol:
  36:        uint40 duration;
  52:        uint40 expiry;
 151:        uint40 zoraTimeout =
 153:        uint40 zoraDuration =
 202:        uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MIN_ORDER_DURATION));
 203:        uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MAX_ORDER_DURATION));

contracts/utils/LibSafeCast.sol:
  12:        function safeCastUint256ToUint96(uint256 v) internal pure returns (uint96) {
  19:        function safeCastUint256ToUint128(uint256 v) internal pure returns (uint128) {
  26:        function safeCastUint256ToInt192(uint256 v) internal pure returns (int192) {
  33:        function safeCastUint96ToInt192(uint96 v) internal pure returns (int192) {
  33:        function safeCastUint96ToInt192(uint96 v) internal pure returns (int192) {
  37:        function safeCastInt192ToUint96(int192 i192) internal pure returns (uint96) {
  37:        function safeCastInt192ToUint96(int192 i192) internal pure returns (uint96) {
  47:        returns (int128)
  58:        returns (uint40)

contracts/party/PartyGovernance.sol:
  75:        uint40 voteDuration;
  78:        uint40 executionDelay;
  81:        uint16 passThresholdBps;
  83:        uint96 totalVotingPower;
  85:        uint16 feeBps;
  93:        uint40 voteDuration;
  94:        uint40 executionDelay;
  95:        uint16 passThresholdBps;
  96:        uint96 totalVotingPower;
 102:        uint40 timestamp;
 104:        uint96 delegatedVotingPower;
 106:        uint96 intrinsicVotingPower;
 116:        uint40 maxExecutableTime;
 119:        uint40 cancelDelay;
 130:        uint40 proposedTime;
 132:        uint40 passedTime;
 134:        uint40 executedTime;
 136:        uint40 completedTime;
 138:        uint96 votes; // -1 == vetoed
 347:        function getVotingPowerAt(address voter, uint40 timestamp)
 350:        returns (uint96 votingPower)
 361:        function getVotingPowerAt(address voter, uint40 timestamp, uint256 snapIndex)
 364:        returns (uint96 votingPower)
 422:        function findVotingPowerSnapshotIndex(address voter, uint40 timestamp)
 594:        uint96 votingPower = getVotingPowerAt(msg.sender, values.proposedTime, snapIndex);
 848:        function _getVotingPowerSnapshotAt(address voter, uint40 timestamp, uint256 hintIndex)
 884:        int192 powerI192 = power.safeCastUint256ToInt192();
 890:        function _adjustVotingPower(address voter, int192 votingPower, address delegate)
 953:        uint96 newDelegateDelegatedVotingPower =
1036:        uint40 t = uint40(block.timestamp);
1057:        function _isUnanimousVotes(uint96 totalVotes, uint96 totalVotingPower)
1057:        function _isUnanimousVotes(uint96 totalVotes, uint96 totalVotingPower)
1070:        uint96 voteCount,
1071:        uint96 totalVotingPower,
1072:        uint16 passThresholdBps

contracts/distribution/TokenDistributor.sol:
  24:        uint128 remainingMemberSupply;
  40:        uint16 feeBps;
 101:        uint16 feeBps
 122:        uint16 feeBps
 140:        returns (uint128 amountClaimed)
 166:        uint128 remainingMemberSupply = state.remainingMemberSupply;
 252:        returns (uint128)
 295:        returns (uint128)
 338:        uint128 supply;
 352:        uint128 fee = supply * args.feeBps / 1e4;
 353:        uint128 memberSupply = supply - fee;

contracts/distribution/ITokenDistributor.sol:
  28:        uint128 memberSupply;
  30:        uint128 fee;
  65:        uint16 feeBps
  85:        uint16 feeBps
  98:        returns (uint128 amountClaimed);
 134:        returns (uint128);
 168:        returns (uint128);

contracts/crowdfund/BuyCrowdfundBase.sol:
  27:        uint40 duration;
  30:        uint96 maximumPrice;
  36:        uint16 splitBps;
  94:        uint96 callValue,
 114:        uint96 settledPrice_;
 116:        uint96 maximumPrice_ = maximumPrice;

contracts/crowdfund/BuyCrowdfund.sol:
  30:        uint40 duration;
  33:        uint96 maximumPrice;
  39:        uint16 splitBps;
 100:        uint96 callValue,

contracts/crowdfund/Crowdfund.sol:
  37:        uint40 voteDuration;
  40:        uint40 executionDelay;
  43:        uint16 passThresholdBps;
  45:        uint16 feeBps;
  55:        uint16 splitBps;
  67:        uint96 previousTotalContributions;
  69:        uint96 amount;
 143:        uint96 initialBalance = address(this).balance.safeCastUint256ToUint96();
 380:        uint96 amount,
 382:        uint96 previousTotalContributions,

contracts/crowdfund/CollectionBuyCrowdfund.sol:
  31:        uint40 duration;
  34:        uint96 maximumPrice;
  40:        uint16 splitBps;
 116:        uint96 callValue,

contracts/crowdfund/AuctionCrowdfund.sol:
  48:        uint40 duration;
  50:        uint96 maximumBid;
  56:        uint16 splitBps;
 170:        uint96 bidAmount = market.getMinimumBid(auctionId_).safeCastUint256ToUint96();
 210:        uint96 lastBid_ = lastBid;

contracts/vendor/markets/IZoraAuctionHouse.sol:
  25:        uint8 curatorFeePercentage;
  44:        uint8 curatorFeePercentages,
  52:        function minBidIncrementPercentage() external view returns (uint8);
```

## Not using the named return variables when a function returns, wastes deployment gas  

_There are **50** instances of this issue:_  
```solidity
contracts/proposals/FractionalizeProposal.sol:
  78:        return "";

contracts/proposals/ListOnZoraProposal.sol:
 115:        return abi.encode(ZoraStep.ListedOnZora, ZoraProgressData({
 116:            auctionId: auctionId,
 117:            minExpiry: (block.timestamp + data.timeout).safeCastUint256ToUint40()
 118:        }));

 125:        return "";
 181:        return false;
 193:        return false;
 203:        return true;

contracts/proposals/ArbitraryCallsProposal.sol:
  90:        return '';
 154:        return false;

 177:        return !LibProposal.isTokenIdPrecious(
 178:            IERC721(call.target),
 179:            tokenId,
 180:            preciousTokens,
 181:            preciousTokenIds
 182:        );

 189:        return !LibProposal.isTokenPrecious(IERC721(call.target), preciousTokens);
 195:        return false;
 199:        return true;

contracts/proposals/ProposalExecutionEngine.sol:
 112:        return _getStorage().currentInProgressProposalId;

contracts/proposals/ProposalStorage.sol:
  26:        return _getSharedProposalStorage().engineImpl;

contracts/proposals/ListOnOpenseaProposal.sol:
 164:        return abi.encode(ListOnOpenseaStep.ListedOnZora, ZoraProgressData({
 165:            auctionId: auctionId,
 166:            minExpiry: (block.timestamp + zoraTimeout).safeCastUint256ToUint40()
 167:        }));

 185:        return "";
 219:        return abi.encode(ListOnOpenseaStep.ListedOnOpenSea, orderHash, expiry);
 235:        return "";

contracts/utils/LibSafeERC721.sol:
  26:        return address(0);
  29:        return abi.decode(r, (address));

contracts/party/PartyGovernance.sol:
 352:        return getVotingPowerAt(voter, timestamp, type(uint256).max);
 367:        return (snap.isDelegated ? 0 : snap.intrinsicVotingPower) + snap.delegatedVotingPower;
 386:        return _governanceValues;
 445:        return high == 0 ? type(uint256).max : high - 1;

 500:        return distributor.createNativeDistribution
 501:            { value: address(this).balance }(this, feeRecipient, feeBps);


 509:        return distributor.createErc20Distribution(
 510:            IERC20(token),
 511:            this,
 512:            feeRecipient,
 513:            feeBps
 514:        );

 608:        return values.votes;
 844:        return nextProgressData.length == 0;
 864:        return snaps[hintIndex];
 871:        return snaps[hintIndex];
1019:        return ProposalStatus.Invalid;
1024:        return ProposalStatus.InProgress;
1028:        return ProposalStatus.Cancelled;
1030:        return ProposalStatus.Complete;
1034:        return ProposalStatus.Defeated;
1041:        return ProposalStatus.Ready;
1045:        return ProposalStatus.Ready;
1048:        return ProposalStatus.Passed;
1052:        return ProposalStatus.Defeated;
1054:        return ProposalStatus.Voting;

contracts/party/PartyGovernanceNFT.sol:
  72:        return ERC721.ownerOf(tokenId);

contracts/distribution/TokenDistributor.sol:
 409:        return bytes32(uint256(uint160(NATIVE_TOKEN_ADDRESS)));
 412:        return bytes32(uint256(uint160(token)));

contracts/crowdfund/CrowdfundNFT.sol:
 134:        return _doesTokenExistFor(owner) ? 1 : 0;

contracts/crowdfund/CrowdfundFactory.sol:
 121:        return gateKeeperId;
 129:        return abi.decode(r, (bytes12));

contracts/crowdfund/BuyCrowdfund.sol:
 107:        return _buy(
 108:            nftContract,
 109:            nftTokenId,
 110:            callTarget,
 111:            callValue,
 112:            callData,
 113:            governanceOpts
 114:        );

contracts/crowdfund/Crowdfund.sol:
 319:        return _createParty(partyFactory, governanceOpts, tokens, tokenIds);

contracts/crowdfund/CollectionBuyCrowdfund.sol:
 124:        return _buy(
 125:            nftContract,
 126:            tokenId,
 127:            callTarget,
 128:            callValue,
 129:            callData,
 130:            governanceOpts
 131:        );

contracts/crowdfund/AuctionCrowdfund.sol:
 289:        return lastBid;
```

## `abi.encode()` is less efficient than `abi.encodePacked()`
`abi.encode()` pads its parameters to 32 bytes, regardless of their type. 

In comparison, `abi.encodePacked()` encodes its parameters using the minimal space required by their types. For example, encoding a `uint8` it will use only 1 byte. Thus, `abi.encodePacked()` should be used instead of `abi.encode()` when possible as it saves space, thus reducing gas costs.

_There are **4** instances of this issue:_  
```solidity
contracts/proposals/ListOnZoraProposal.sol:
 115:        return abi.encode(ZoraStep.ListedOnZora, ZoraProgressData({
 116:            auctionId: auctionId,
 117:            minExpiry: (block.timestamp + data.timeout).safeCastUint256ToUint40()
 118:        }));

contracts/proposals/ListOnOpenseaProposal.sol:
 164:        return abi.encode(ListOnOpenseaStep.ListedOnZora, ZoraProgressData({
 165:            auctionId: auctionId,
 166:            minExpiry: (block.timestamp + zoraTimeout).safeCastUint256ToUint40()
 167:        }));

 219:        return abi.encode(ListOnOpenseaStep.ListedOnOpenSea, orderHash, expiry);

contracts/utils/ReadOnlyDelegateCall.sol:
  23:        abi.encode(s, r).rawRevert();
```

## `internal` functions only called once can be inlined to save gas
As compared to `internal` functions, a non-inlined function costs **20 to 40 gas** extra because of two extra `JUMP` instructions and additional stack operations needed for function calls. Thus, consider inlining `internal` functions that are only called once in the function that calls them.

_There are **4** instances of this issue:_  
```solidity
contracts/proposals/FractionalizeProposal.sol:
  39:        function _executeFractionalize(
  40:            IProposalExecutionEngine.ExecuteProposalParams memory params
  41:        )
  42:            internal
  43:            returns (bytes memory nextProgressData)
  44:        {

contracts/proposals/ListOnZoraProposal.sol:
  78:        function _executeListOnZora(
  79:            IProposalExecutionEngine.ExecuteProposalParams memory params
  80:        )
  81:            internal
  82:            returns (bytes memory nextProgressData)
  83:        {

contracts/proposals/ArbitraryCallsProposal.sol:
  37:        function _executeArbitraryCalls(
  38:            IProposalExecutionEngine.ExecuteProposalParams memory params
  39:        )
  40:            internal
  41:            returns (bytes memory nextProgressData)
  42:        {

contracts/crowdfund/CrowdfundNFT.sol:
 141:        function _mint(address owner) internal returns (uint256 tokenId)
 142:        {
```