# PARTY-CONTRACTS-C4

## Gas Optimizations Report

### The usage of `++i` will cost less gas than `i++`. The same change can be applied to `i--` as well.

This change would save up to 6 gas per instance/loop.

_There are **1** instances of this issue:_

```solidity
File: contracts/crowdfund/CollectionBuyCrowdfund.sol

62:   for (uint256 i; i < hosts.length; i++) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol

### State variables should be cached in stack variables rather than re-reading them.

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

_There are **4** instances of this issue:_

```solidity
File: contracts/crowdfund/AuctionCrowdfund.sol

      /// @audit Cache `market`. Used 3 times in `bid`
162:  if (market.isFinalized(auctionId_)) {
170:  uint96 bidAmount = market.getMinimumBid(auctionId_).safeCastUint256ToUint96();
178:  (bool s, bytes memory r) = address(market).delegatecall(abi.encodeCall(
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol

```solidity
File: contracts/crowdfund/Crowdfund.sol

      /// @audit Cache `gateKeeper`. Used 3 times in `_contribute`
392:  if (gateKeeper != IGateKeeper(address(0))) {
393:  if (!gateKeeper.isAllowed(contributor, gateKeeperId, gateData)) {
396:  gateKeeper,
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

```solidity
File: contracts/proposals/ListOnOpenseaProposal.sol

      /// @audit Cache `_GLOBALS`. Used 4 times in `_executeListOnOpensea`
152:  uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_ZORA_AUCTION_TIMEOUT));
154:  uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_ZORA_AUCTION_DURATION));
202:  uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MIN_ORDER_DURATION));
203:  uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MAX_ORDER_DURATION));
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol

```solidity
File: contracts/proposals/ListOnZoraProposal.sol

      /// @audit Cache `_GLOBALS`. Used 3 times in `_executeListOnZora`
94:   uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MIN_AUCTION_DURATION));
95:   uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_DURATION));
102:  uint40 maxTimeout = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_TIMEOUT));
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol

### `internal` and `private` functions that are called only once should be inlined.

The execution of a non-inlined function would cost up to 40 more gas because of two extra `jump`s as well as some other instructions.

_There are **14** instances of this issue:_

```solidity
File: contracts/party/PartyGovernance.sol

922:  function _rebalanceDelegates(

1001: function _getProposalFlags(ProposalStateValues memory pv)

1069: function _areVotesPassing(

1082: function _setPreciousList(

1094: function _isPreciousListCorrect(
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

```solidity
File: contracts/proposals/ArbitraryCallsProposal.sol

93:   function _executeSingleArbitraryCall(

142:  function _isCallAllowed(

203:  function _decodeApproveCallDataArgs(bytes memory callData)

221:  function _decodeSetApprovalForAllCallDataArgs(bytes memory callData)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol

```solidity
File: contracts/proposals/ListOnOpenseaProposal.sol

238:  function _listOnOpensea(

328:  function _getOrderHash(IOpenseaExchange.OrderParameters memory orderParams)

350:  function _cleanUpListing(
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol

```solidity
File: contracts/proposals/ProposalExecutionEngine.sol

228:  function _extractProposalType(bytes memory proposalData)

251:  function _executeUpgradeProposalsImplementation(bytes memory proposalData)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol

### Using `!= 0` on `uints` costs less gas than `> 0`.

This change saves 3 gas per instance/loop

_There are **2** instances of this issue:_

```solidity
File: contracts/crowdfund/Crowdfund.sol

144:  if (initialBalance > 0) {

471:  if (votingPower > 0) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

### It costs more gas to initialize non-`constant`/non-`immutable` variables to zero than to let the default of zero be applied

Not overwriting the default for stack variables saves 8 gas. Storage and memory variables have larger savings

_There are **14** instances of this issue:_

```solidity
File: contracts/crowdfund/Crowdfund.sol

180:  for (uint256 i = 0; i < contributors.length; ++i) {

242:  for (uint256 i = 0; i < numContributions; ++i) {

300:  for (uint256 i = 0; i < preciousTokens.length; ++i) {

348:  for (uint256 i = 0; i < numContributions; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

```solidity
File: contracts/distribution/TokenDistributor.sol

230:  for (uint256 i = 0; i < infos.length; ++i) {

239:  for (uint256 i = 0; i < infos.length; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol

```solidity
File: contracts/party/PartyGovernance.sol

306:  for (uint256 i=0; i < opts.hosts.length; ++i) {

432:  uint256 low = 0;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

```solidity
File: contracts/proposals/ArbitraryCallsProposal.sol

52:   for (uint256 i = 0; i < hadPreciouses.length; ++i) {

61:   for (uint256 i = 0; i < calls.length; ++i) {

78:   for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol

```solidity
File: contracts/proposals/LibProposal.sol

14:   for (uint256 i = 0; i < preciousTokens.length; ++i) {

32:   for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol

```solidity
File: contracts/proposals/ListOnOpenseaProposal.sol

291:  for (uint256 i = 0; i < fees.length; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol

### Functions that are access-restricted from most users may be marked as `payable`

Marking a function as `payable` reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **1** instances of this issue:_

```solidity
File: contracts/utils/ReadOnlyDelegateCall.sol

18:   function delegateCallAndRevert(address impl, bytes memory callData) external {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol

### `++i`/`i++` should be `unchecked{++I}`/`unchecked{I++}` in `for`-loops

When an increment or any arithmetic operation is not possible to overflow it should be placed in `unchecked{}` block. \This is because of the default compiler overflow and underflow safety checks since Solidity version 0.8.0. \In for-loops it saves around 30-40 gas **per loop**

_There are **14** instances of this issue:_

```solidity
File: contracts/crowdfund/CollectionBuyCrowdfund.sol

62:   for (uint256 i; i < hosts.length; i++) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol

```solidity
File: contracts/crowdfund/Crowdfund.sol

180:  for (uint256 i = 0; i < contributors.length; ++i) {

242:  for (uint256 i = 0; i < numContributions; ++i) {

300:  for (uint256 i = 0; i < preciousTokens.length; ++i) {

348:  for (uint256 i = 0; i < numContributions; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

```solidity
File: contracts/distribution/TokenDistributor.sol

230:  for (uint256 i = 0; i < infos.length; ++i) {

239:  for (uint256 i = 0; i < infos.length; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol

```solidity
File: contracts/party/PartyGovernance.sol

306:  for (uint256 i=0; i < opts.hosts.length; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

```solidity
File: contracts/proposals/ArbitraryCallsProposal.sol

52:   for (uint256 i = 0; i < hadPreciouses.length; ++i) {

61:   for (uint256 i = 0; i < calls.length; ++i) {

78:   for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol

```solidity
File: contracts/proposals/LibProposal.sol

14:   for (uint256 i = 0; i < preciousTokens.length; ++i) {

32:   for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol

```solidity
File: contracts/proposals/ListOnOpenseaProposal.sol

291:  for (uint256 i = 0; i < fees.length; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol

### `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **2** instances of this issue:_

```solidity
File: contracts/crowdfund/Crowdfund.sol

411:  totalContributions += amount;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

```solidity
File: contracts/distribution/TokenDistributor.sol

381:  _storedBalances[balanceId] -= amount;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol

### Usage of `uint`s/`int`s smaller than 32 bytes (256 bits) incurs overhead

'When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.' \ https://docs.soliditylang.org/en/v0.8.15/internals/layout_in_storage.html \ Use a larger size then downcast where needed

_There are **34** instances of this issue:_

```solidity
File: contracts/crowdfund/AuctionCrowdfund.sol

56:   uint16 splitBps;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol

```solidity
File: contracts/crowdfund/BuyCrowdfund.sol

39:   uint16 splitBps;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol

```solidity
File: contracts/crowdfund/BuyCrowdfundBase.sol

36:   uint16 splitBps;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol

```solidity
File: contracts/crowdfund/CollectionBuyCrowdfund.sol

40:   uint16 splitBps;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol

```solidity
File: contracts/crowdfund/Crowdfund.sol

43:   uint16 passThresholdBps;

45:   uint16 feeBps;

55:   uint16 splitBps;

80:   error InvalidBpsError(uint16 bps);

104:  uint16 public splitBps;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

```solidity
File: contracts/distribution/ITokenDistributor.sol

28:   uint128 memberSupply;

30:   uint128 fee;

65:   uint16 feeBps

85:   uint16 feeBps

98:   returns (uint128 amountClaimed);
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol

```solidity
File: contracts/distribution/TokenDistributor.sol

24:   uint128 remainingMemberSupply;

40:   uint16 feeBps;

50:   error InvalidDistributionSupplyError(uint128 supply);

52:   error InvalidFeeBpsError(uint16 feeBps);

101:  uint16 feeBps

122:  uint16 feeBps

140:  returns (uint128 amountClaimed)

166:  uint128 remainingMemberSupply = state.remainingMemberSupply;

338:  uint128 supply;

352:  uint128 fee = supply * args.feeBps / 1e4;

353:  uint128 memberSupply = supply - fee;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol

```solidity
File: contracts/party/PartyGovernance.sol

81:   uint16 passThresholdBps;

85:   uint16 feeBps;

95:   uint16 passThresholdBps;

187:  error InvalidBpsError(uint16 bps);

199:  uint16 public feeBps;

1072: uint16 passThresholdBps
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

```solidity
File: contracts/proposals/ProposalExecutionEngine.sol

29:   error UnsupportedProposalTypeError(uint32 proposalType);
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol

```solidity
File: contracts/vendor/markets/IZoraAuctionHouse.sol

25:   uint8 curatorFeePercentage;

44:   uint8 curatorFeePercentages,
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/markets/IZoraAuctionHouse.sol

### Use `calldata` instead of `memory` for function parameters

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

_There are **88** instances of this issue:_

```solidity
File: contracts/crowdfund/AuctionCrowdfund.sol

110:  function initialize(AuctionCrowdfundOptions memory opts)

196:  function finalize(FixedGovernanceOpts memory governanceOpts)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol

```solidity
File: contracts/crowdfund/BuyCrowdfund.sol

68:   function initialize(BuyCrowdfundOptions memory opts)

102:  FixedGovernanceOpts memory governanceOpts
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol

```solidity
File: contracts/crowdfund/BuyCrowdfundBase.sol

70:   function _initialize(BuyCrowdfundBaseOptions memory opts)

96:   FixedGovernanceOpts memory governanceOpts
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol

```solidity
File: contracts/crowdfund/CollectionBuyCrowdfund.sol

83:   function initialize(CollectionBuyCrowdfundOptions memory opts)

118:  FixedGovernanceOpts memory governanceOpts
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol

```solidity
File: contracts/crowdfund/Crowdfund.sol

124:  function _initialize(CrowdfundOptions memory opts)

191:  function contribute(address delegate, bytes memory gateData)

264:  FixedGovernanceOpts memory governanceOpts,

265:  IERC721[] memory preciousTokens,

266:  uint256[] memory preciousTokenIds

308:  FixedGovernanceOpts memory governanceOpts,

322:  function _hashFixedGovernanceOpts(FixedGovernanceOpts memory opts)

383:  bytes memory gateData
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

```solidity
File: contracts/crowdfund/CrowdfundFactory.sol

36:   BuyCrowdfund.BuyCrowdfundOptions memory opts,

37:   bytes memory createGateCallData

62:   AuctionCrowdfund.AuctionCrowdfundOptions memory opts,

63:   bytes memory createGateCallData

88:   CollectionBuyCrowdfund.CollectionBuyCrowdfundOptions memory opts,

89:   bytes memory createGateCallData

110:  bytes memory createGateCallData
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol

```solidity
File: contracts/crowdfund/CrowdfundNFT.sol

      /// @audit Store `name_` in calldata.
39:   function _initialize(string memory name_, string memory symbol_)

      /// @audit Store `symbol_` in calldata.
39:   function _initialize(string memory name_, string memory symbol_)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol

```solidity
File: contracts/distribution/TokenDistributor.sol

331:  function _createDistribution(CreateDistributionArgs memory args)

390:  function _getDistributionHash(DistributionInfo memory info)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol

```solidity
File: contracts/party/Party.sol

33:   function initialize(PartyInitData memory initData)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol

```solidity
File: contracts/party/PartyFactory.sol

28:   Party.PartyOptions memory opts,

29:   IERC721[] memory preciousTokens,

30:   uint256[] memory preciousTokenIds
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol

```solidity
File: contracts/party/PartyGovernance.sol

272:  GovernanceOpts memory opts,

273:  IERC721[] memory preciousTokens,

274:  uint256[] memory preciousTokenIds

394:  function getProposalHash(Proposal memory proposal)

526:  function propose(Proposal memory proposal, uint256 latestSnapIndex)

655:  Proposal memory proposal,

656:  IERC721[] memory preciousTokens,

657:  uint256[] memory preciousTokenIds,

806:  Proposal memory proposal,

807:  IERC721[] memory preciousTokens,

808:  uint256[] memory preciousTokenIds,

810:  bytes memory progressData,

811:  bytes memory extraData

926:  VotingPowerSnapshot memory oldSnap,

927:  VotingPowerSnapshot memory newSnap

973:  function _insertVotingPowerSnapshot(address voter, VotingPowerSnapshot memory snap)

1001: function _getProposalFlags(ProposalStateValues memory pv)

1012: function _getProposalStatus(ProposalStateValues memory pv)

1083: IERC721[] memory preciousTokens,

1084: uint256[] memory preciousTokenIds

1095: IERC721[] memory preciousTokens,

1096: uint256[] memory preciousTokenIds

1106: IERC721[] memory preciousTokens,

1107: uint256[] memory preciousTokenIds

1127: function _validateProposalHash(Proposal memory proposal, bytes32 expectedHash)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

```solidity
File: contracts/party/PartyGovernanceNFT.sol

50:   string memory name_,

51:   string memory symbol_,

52:   PartyGovernance.GovernanceOpts memory governanceOpts,

53:   IERC721[] memory preciousTokens,

54:   uint256[] memory preciousTokenIds,
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol

```solidity
File: contracts/proposals/ArbitraryCallsProposal.sol

38:   IProposalExecutionEngine.ExecuteProposalParams memory params

95:   ArbitraryCall memory call,

96:   IERC721[] memory preciousTokens,

97:   uint256[] memory preciousTokenIds,

143:  ArbitraryCall memory call,

145:  IERC721[] memory preciousTokens,

146:  uint256[] memory preciousTokenIds

203:  function _decodeApproveCallDataArgs(bytes memory callData)

221:  function _decodeSetApprovalForAllCallDataArgs(bytes memory callData)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol

```solidity
File: contracts/proposals/FractionalizeProposal.sol

40:   IProposalExecutionEngine.ExecuteProposalParams memory params
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol

```solidity
File: contracts/proposals/LibProposal.sol

9:    function isTokenPrecious(IERC721 token, IERC721[] memory preciousTokens)

25:   IERC721[] memory preciousTokens,

26:   uint256[] memory preciousTokenIds
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol

```solidity
File: contracts/proposals/ListOnOpenseaProposal.sol

124:  IProposalExecutionEngine.ExecuteProposalParams memory params

243:  uint256[] memory fees,

244:  address payable[] memory feeRecipients

328:  function _getOrderHash(IOpenseaExchange.OrderParameters memory orderParams)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol

```solidity
File: contracts/proposals/ListOnZoraProposal.sol

79:   IProposalExecutionEngine.ExecuteProposalParams memory params
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol

```solidity
File: contracts/proposals/ProposalExecutionEngine.sol

116:  function executeProposal(ExecuteProposalParams memory params)

205:  function _execute(ProposalType pt, ExecuteProposalParams memory params)

228:  function _extractProposalType(bytes memory proposalData)

251:  function _executeUpgradeProposalsImplementation(bytes memory proposalData)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol

```solidity
File: contracts/proposals/ProposalStorage.sol

33:   function _initProposalImpl(IProposalExecutionEngine impl, bytes memory initData)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol

```solidity
File: contracts/utils/LibRawResult.sol

6:    function rawRevert(bytes memory b)

14:   function rawReturn(bytes memory b)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibRawResult.sol

```solidity
File: contracts/utils/ReadOnlyDelegateCall.sol

18:   function delegateCallAndRevert(address impl, bytes memory callData) external {

27:   function _readOnlyDelegateCall(address impl, bytes memory callData) internal view {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol

### Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for `>=` or `<=`. When using greater than or equal, two operations are performed: `>` and `=`. Using strict comparison operators hence saves gas

_There are **12** instances of this issue:_

```solidity
File: contracts/crowdfund/AuctionCrowdfund.sol

276:  if (block.timestamp >= expiry) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol

```solidity
File: contracts/crowdfund/BuyCrowdfundBase.sol

159:  if (block.timestamp >= expiry) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol

```solidity
File: contracts/crowdfund/Crowdfund.sol

350:  if (c.previousTotalContributions >= totalEthUsed) {

353:  } else if (c.previousTotalContributions + c.amount <= totalEthUsed) {

423:  if (numContributions >= 1) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

```solidity
File: contracts/party/PartyGovernance.sol

860:  snaps[hintIndex].timestamp <= timestamp &&

1040: if (pv.passedTime + gv.executionDelay <= t) {

1051: if (pv.proposedTime + gv.voteDuration <= t) {

1066: return acceptanceRatio >= 0.9999e4;

1079: / uint256(totalVotingPower) >= uint256(passThresholdBps);
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

```solidity
File: contracts/proposals/ArbitraryCallsProposal.sol

156:  if (call.data.length >= 4) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol

```solidity
File: contracts/proposals/ListOnOpenseaProposal.sol

364:  } else if (expiry <= block.timestamp) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol

### Tight variable packing

Solidity contracts have contiguous 32 bytes (256 bits) slots used in storage. By arranging the variables, it is possible to minimize the number of slots used within a contract’s storage and therefore reduce deployment costs.

_There are **1** instances of this issue:_

```solidity
File: contracts/crowdfund/Crowdfund.sol

      87:        /// @audit Currently storage variables are packed in 8 slots but could fit in 7
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

### Use `immutable` & `constant` for state variables that do not change their value

_There are **12** instances of this issue:_

```solidity
File: contracts/crowdfund/AuctionCrowdfund.sol

85:   IERC721 public nftContract;

87:   uint256 public nftTokenId;

90:   IMarketWrapper public market;

92:   uint256 public auctionId;

94:   uint96 public maximumBid;

99:   uint40 public expiry;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol

```solidity
File: contracts/crowdfund/BuyCrowdfund.sol

57:   uint256 public nftTokenId;

59:   IERC721 public nftContract;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol

```solidity
File: contracts/crowdfund/BuyCrowdfundBase.sol

60:   uint40 public expiry;

62:   uint96 public maximumPrice;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol

```solidity
File: contracts/crowdfund/CollectionBuyCrowdfund.sol

58:   IERC721 public nftContract;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol

```solidity
File: contracts/utils/Implementation.sol

11:   constructor() { IMPL = address(this); }
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Implementation.sol

### Use custom errors rather than `revert()`/`require()` strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

_There are **1** instances of this issue:_

```solidity
File: contracts/crowdfund/CrowdfundNFT.sol

29:   revert('ALWAYS FAILING');
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol

### Using bools for storage variables incurs overhead

Use `uint256(1)` and `uint256(2)` for `true`/`false` to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from 'false' to 'true', after having been 'true' in the past

_There are **3** instances of this issue:_

```solidity
File: contracts/crowdfund/Crowdfund.sol

106:  bool private _splitRecipientHasBurned;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

```solidity
File: contracts/distribution/TokenDistributor.sol

62:   bool public emergencyActionsDisabled;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol

```solidity
File: contracts/party/PartyGovernance.sol

197:  bool public emergencyExecuteDisabled;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

### Use a more recent version of solidity

Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

_There are **46** instances of this issue:_

```solidity
File: contracts/crowdfund/AuctionCrowdfund.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol

```solidity
File: contracts/crowdfund/BuyCrowdfund.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol

```solidity
File: contracts/crowdfund/BuyCrowdfundBase.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol

```solidity
File: contracts/crowdfund/CollectionBuyCrowdfund.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol

```solidity
File: contracts/crowdfund/Crowdfund.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

```solidity
File: contracts/crowdfund/CrowdfundFactory.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol

```solidity
File: contracts/crowdfund/CrowdfundNFT.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol

```solidity
File: contracts/distribution/ITokenDistributor.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol

```solidity
File: contracts/distribution/ITokenDistributorParty.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributorParty.sol

```solidity
File: contracts/distribution/TokenDistributor.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol

```solidity
File: contracts/gatekeepers/IGateKeeper.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/gatekeepers/IGateKeeper.sol

```solidity
File: contracts/globals/Globals.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol

```solidity
File: contracts/globals/IGlobals.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/IGlobals.sol

```solidity
File: contracts/market-wrapper/IMarketWrapper.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/market-wrapper/IMarketWrapper.sol

```solidity
File: contracts/party/IPartyFactory.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/IPartyFactory.sol

```solidity
File: contracts/party/Party.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol

```solidity
File: contracts/party/PartyFactory.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol

```solidity
File: contracts/party/PartyGovernance.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

```solidity
File: contracts/party/PartyGovernanceNFT.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol

```solidity
File: contracts/proposals/ArbitraryCallsProposal.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol

```solidity
File: contracts/proposals/FractionalizeProposal.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol

```solidity
File: contracts/proposals/IProposalExecutionEngine.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/IProposalExecutionEngine.sol

```solidity
File: contracts/proposals/LibProposal.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol

```solidity
File: contracts/proposals/ListOnOpenseaProposal.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol

```solidity
File: contracts/proposals/ListOnZoraProposal.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol

```solidity
File: contracts/proposals/ProposalExecutionEngine.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol

```solidity
File: contracts/proposals/ProposalStorage.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol

```solidity
File: contracts/proposals/vendor/FractionalV1.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/FractionalV1.sol

```solidity
File: contracts/proposals/vendor/IOpenseaConduitController.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaConduitController.sol

```solidity
File: contracts/proposals/vendor/IOpenseaExchange.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaExchange.sol

```solidity
File: contracts/tokens/ERC1155Receiver.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC1155Receiver.sol

```solidity
File: contracts/tokens/ERC721Receiver.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC721Receiver.sol

```solidity
File: contracts/tokens/IERC1155.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC1155.sol

```solidity
File: contracts/tokens/IERC20.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC20.sol

```solidity
File: contracts/tokens/IERC721.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721.sol

```solidity
File: contracts/tokens/IERC721Receiver.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721Receiver.sol

```solidity
File: contracts/utils/EIP165.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/EIP165.sol

```solidity
File: contracts/utils/Implementation.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Implementation.sol

```solidity
File: contracts/utils/LibAddress.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibAddress.sol

```solidity
File: contracts/utils/LibERC20Compat.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibERC20Compat.sol

```solidity
File: contracts/utils/LibRawResult.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibRawResult.sol

```solidity
File: contracts/utils/LibSafeCast.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibSafeCast.sol

```solidity
File: contracts/utils/LibSafeERC721.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibSafeERC721.sol

```solidity
File: contracts/utils/Proxy.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol

```solidity
File: contracts/utils/ReadOnlyDelegateCall.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol

```solidity
File: contracts/vendor/markets/IZoraAuctionHouse.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/markets/IZoraAuctionHouse.sol
