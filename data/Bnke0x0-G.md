

### [G01] State variables only set in the constructor should be declared `immutable`

#### Impact
Avoids a Gsset (20000 gas)
#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::85 => IERC721 public nftContract;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::87 => uint256 public nftTokenId;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::90 => IMarketWrapper public market;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::92 => uint256 public auctionId;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::94 => uint96 public maximumBid;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::96 => uint96 public lastBid;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::99 => uint40 public expiry;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::101 => AuctionCrowdfundStatus private _bidStatus;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::57 => uint256 public nftTokenId;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::59 => IERC721 public nftContract;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::60 => uint40 public expiry;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::62 => uint96 public maximumPrice;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::64 => uint96 public settledPrice;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::91 => Party public party;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::93 => uint96 public totalContributions;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::96 => IGateKeeper public gateKeeper;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::98 => bytes12 public gateKeeperId;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::104 => uint16 public splitBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::110 => bytes32 public governanceOptsHash;
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::21 => string public name;
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::24 => string public symbol;
party-contracts-c4/contracts/globals/Globals.sol::8 => address public multiSig;
party-contracts-c4/contracts/party/PartyGovernance.sol::199 => uint16 public feeBps;
party-contracts-c4/contracts/party/PartyGovernance.sol::203 => bytes32 public preciousListHash;
party-contracts-c4/contracts/party/PartyGovernance.sol::205 => uint256 public lastProposalId;
party-contracts-c4/contracts/party/PartyGovernance.sol::211 => GovernanceValues private _governanceValues;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::28 => address public mintAuthority;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::31 => uint256 public tokenCount;
```

### [G02] State variables can be packed into fewer storage slots

#### Impact
If variables occupying the same slot are both written the same 
function or by the constructor, avoids a separate Gsset (20000 gas). 
Reads of the variables are also cheaper
#### Findings:
```
party-contracts-c4/contracts/globals/Globals.sol::8 => address public multiSig;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::28 => address public mintAuthority;
party-contracts-c4/contracts/utils/Implementation.sol::9 => address public immutable IMPL;
```


### [G03] `<array>.length` should not be looked up in every loop of a `for` loop

#### Impact
Even memory arrays incur the overhead of bit tests and bit shifts to 
calculate the array length. Storage array length checks incur an extra 
Gwarmaccess (100 gas) PER-LOOP.
#### Findings:
```
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::62 => for (uint256 i; i < hosts.length; i++) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::180 => for (uint256 i = 0; i < contributors.length; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::300 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::230 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::239 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4/contracts/party/PartyGovernance.sol::306 => for (uint256 i=0; i < opts.hosts.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::52 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::61 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::78 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::291 => for (uint256 i = 0; i < fees.length; ++i) {
```




### [G04] `++i/i++` should be `unchecked{++i}`/`unchecked{++i}` when it is not possible for them to overflow, as is the case when used in `for` and `while` loops


#### Findings:
```
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::62 => for (uint256 i; i < hosts.length; i++) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::180 => for (uint256 i = 0; i < contributors.length; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::242 => for (uint256 i = 0; i < numContributions; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::300 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::348 => for (uint256 i = 0; i < numContributions; ++i) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::230 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::239 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4/contracts/party/PartyGovernance.sol::306 => for (uint256 i=0; i < opts.hosts.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::52 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::61 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::78 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::291 => for (uint256 i = 0; i < fees.length; ++i) {
```


### [G05] Not using the named return variables when a function returns, wastes deployment gas


#### Findings:
```
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::107 => return _buy(
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::124 => return _buy(
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::170 => return _burn(contributor, getCrowdfundLifecycle(), party);
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::319 => return _createParty(partyFactory, governanceOpts, tokens, tokenIds);
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::116 => return _delegateToRenderer();
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::121 => return _delegateToRenderer();
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::134 => return _doesTokenExistFor(owner) ? 1 : 0;
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::138 => return _owners[uint256(uint160(owner))] != address(0);
party-contracts-c4/contracts/distribution/TokenDistributor.sol::273 => return _distributionStates[party][distributionId].wasFeeClaimed;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::285 => return _distributionStates[party][distributionId].hasPartyTokenClaimed[partyTokenId];
party-contracts-c4/contracts/distribution/TokenDistributor.sol::297 => return _distributionStates[party][distributionId].remainingMemberSupply;
party-contracts-c4/contracts/globals/Globals.sol::32 => return _wordValues[key];
party-contracts-c4/contracts/globals/Globals.sol::48 => return _includedWordValues[key][value];
party-contracts-c4/contracts/globals/Globals.sol::52 => return _includedWordValues[key][bytes32(value)];
party-contracts-c4/contracts/globals/Globals.sol::56 => return _includedWordValues[key][bytes32(uint256(uint160(value)))];
party-contracts-c4/contracts/party/PartyGovernance.sol::340 => return _getProposalExecutionEngine();
party-contracts-c4/contracts/party/PartyGovernance.sol::386 => return _governanceValues;
party-contracts-c4/contracts/party/PartyGovernance.sol::917 => return _governanceValues.totalVotingPower;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::112 => return _getStorage().currentInProgressProposalId;
party-contracts-c4/contracts/proposals/ProposalStorage.sol::26 => return _getSharedProposalStorage().engineImpl;
```




### [G06] Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement


#### Findings:
```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::144 => if (initialBalance > 0) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::471 => if (votingPower > 0) {
```




### [G07] It costs more gas to initialize variables to zero than to let the default of zero be applied


#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::254 => lastBid = 0;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::180 => for (uint256 i = 0; i < contributors.length; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::242 => for (uint256 i = 0; i < numContributions; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::300 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::348 => for (uint256 i = 0; i < numContributions; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::368 => splitBps_ = 0;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::230 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::239 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4/contracts/party/PartyGovernance.sol::432 => uint256 low = 0;
party-contracts-c4/contracts/party/PartyGovernance.sol::708 => proposalState.values.completedTime = 0;
party-contracts-c4/contracts/party/PartyGovernance.sol::844 => return nextProgressData.length == 0;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::52 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::61 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::78 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::269 => orderParams.salt = 0;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::288 => cons.identifierOrCriteria = 0;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::291 => for (uint256 i = 0; i < fees.length; ++i) {
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::295 => cons.identifierOrCriteria = 0;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::343 => orderParams.totalOriginalConsiderationItems = 0;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::174 => stor.currentInProgressProposalId = 0;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::175 => stor.nextProgressDataHash = 0;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::200 => stor.currentInProgressProposalId = 0;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::201 => stor.nextProgressDataHash = 0;
```




### [G08] `++i` costs less gas than `i++`, especially when it’s used in forloops (`--i`/`i--` too)


#### Findings:
```
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::62 => for (uint256 i; i < hosts.length; i++) {
```





### [G09] Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

#### Impact
> When using elements that are smaller than 32 bytes, your 
contract’s gas usage may be higher. This is because the EVM operates on 
32 bytes at a time. Therefore, if the element is smaller than that, the 
EVM must use more operations in order to reduce the size of the element 
from 32 bytes to the desired size.
> 

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html)
Use a larger size then downcast where needed
#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::48 => uint40 duration;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::50 => uint96 maximumBid;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::56 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::94 => uint96 public maximumBid;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::96 => uint96 public lastBid;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::99 => uint40 public expiry;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::118 => expiry = uint40(opts.duration + block.timestamp);
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::210 => uint96 lastBid_ = lastBid;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::30 => uint40 duration;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::33 => uint96 maximumPrice;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::39 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::100 => uint96 callValue,
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::27 => uint40 duration;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::30 => uint96 maximumPrice;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::36 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::54 => error MaximumPriceError(uint96 callValue, uint96 maximumPrice);
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::60 => uint40 public expiry;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::62 => uint96 public maximumPrice;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::64 => uint96 public settledPrice;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::73 => expiry = uint40(opts.duration + block.timestamp);
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::94 => uint96 callValue,
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::114 => uint96 settledPrice_;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::116 => uint96 maximumPrice_ = maximumPrice;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::31 => uint40 duration;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::34 => uint96 maximumPrice;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::40 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::116 => uint96 callValue,
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::37 => uint40 voteDuration;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::40 => uint40 executionDelay;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::43 => uint16 passThresholdBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::45 => uint16 feeBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::55 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::67 => uint96 previousTotalContributions;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::69 => uint96 amount;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::80 => error InvalidBpsError(uint16 bps);
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::93 => uint96 public totalContributions;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::104 => uint16 public splitBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::143 => uint96 initialBalance = address(this).balance.safeCastUint256ToUint96();
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::380 => uint96 amount,
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::382 => uint96 previousTotalContributions,
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::28 => uint128 memberSupply;
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::30 => uint128 fee;
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::65 => uint16 feeBps
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::85 => uint16 feeBps
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::98 => returns (uint128 amountClaimed);
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::113 => returns (uint128[] memory amountsClaimed);
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::134 => returns (uint128);
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::168 => returns (uint128);
party-contracts-c4/contracts/distribution/TokenDistributor.sol::24 => uint128 remainingMemberSupply;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::40 => uint16 feeBps;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::50 => error InvalidDistributionSupplyError(uint128 supply);
party-contracts-c4/contracts/distribution/TokenDistributor.sol::52 => error InvalidFeeBpsError(uint16 feeBps);
party-contracts-c4/contracts/distribution/TokenDistributor.sol::101 => uint16 feeBps
party-contracts-c4/contracts/distribution/TokenDistributor.sol::122 => uint16 feeBps
party-contracts-c4/contracts/distribution/TokenDistributor.sol::140 => returns (uint128 amountClaimed)
party-contracts-c4/contracts/distribution/TokenDistributor.sol::166 => uint128 remainingMemberSupply = state.remainingMemberSupply;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::227 => returns (uint128[] memory amountsClaimed)
party-contracts-c4/contracts/distribution/TokenDistributor.sol::229 => amountsClaimed = new uint128[](infos.length);
party-contracts-c4/contracts/distribution/TokenDistributor.sol::252 => returns (uint128)
party-contracts-c4/contracts/distribution/TokenDistributor.sol::295 => returns (uint128)
party-contracts-c4/contracts/distribution/TokenDistributor.sol::338 => uint128 supply;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::352 => uint128 fee = supply * args.feeBps / 1e4;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::353 => uint128 memberSupply = supply - fee;
party-contracts-c4/contracts/party/PartyGovernance.sol::36 => using LibSafeCast for uint96;
party-contracts-c4/contracts/party/PartyGovernance.sol::75 => uint40 voteDuration;
party-contracts-c4/contracts/party/PartyGovernance.sol::78 => uint40 executionDelay;
party-contracts-c4/contracts/party/PartyGovernance.sol::81 => uint16 passThresholdBps;
party-contracts-c4/contracts/party/PartyGovernance.sol::83 => uint96 totalVotingPower;
party-contracts-c4/contracts/party/PartyGovernance.sol::85 => uint16 feeBps;
party-contracts-c4/contracts/party/PartyGovernance.sol::93 => uint40 voteDuration;
party-contracts-c4/contracts/party/PartyGovernance.sol::94 => uint40 executionDelay;
party-contracts-c4/contracts/party/PartyGovernance.sol::95 => uint16 passThresholdBps;
party-contracts-c4/contracts/party/PartyGovernance.sol::96 => uint96 totalVotingPower;
party-contracts-c4/contracts/party/PartyGovernance.sol::102 => uint40 timestamp;
party-contracts-c4/contracts/party/PartyGovernance.sol::104 => uint96 delegatedVotingPower;
party-contracts-c4/contracts/party/PartyGovernance.sol::106 => uint96 intrinsicVotingPower;
party-contracts-c4/contracts/party/PartyGovernance.sol::116 => uint40 maxExecutableTime;
party-contracts-c4/contracts/party/PartyGovernance.sol::119 => uint40 cancelDelay;
party-contracts-c4/contracts/party/PartyGovernance.sol::130 => uint40 proposedTime;
party-contracts-c4/contracts/party/PartyGovernance.sol::132 => uint40 passedTime;
party-contracts-c4/contracts/party/PartyGovernance.sol::134 => uint40 executedTime;
party-contracts-c4/contracts/party/PartyGovernance.sol::136 => uint40 completedTime;
party-contracts-c4/contracts/party/PartyGovernance.sol::138 => uint96 votes; // -1 == vetoed
party-contracts-c4/contracts/party/PartyGovernance.sol::176 => error ExecutionTimeExceededError(uint40 maxExecutableTime, uint40 timestamp);
party-contracts-c4/contracts/party/PartyGovernance.sol::186 => error ProposalCannotBeCancelledYetError(uint40 currentTime, uint40 cancelTime);
party-contracts-c4/contracts/party/PartyGovernance.sol::187 => error InvalidBpsError(uint16 bps);
party-contracts-c4/contracts/party/PartyGovernance.sol::190 => uint96 constant private VETO_VALUE = uint96(int96(-1));
party-contracts-c4/contracts/party/PartyGovernance.sol::199 => uint16 public feeBps;
party-contracts-c4/contracts/party/PartyGovernance.sol::347 => function getVotingPowerAt(address voter, uint40 timestamp)
party-contracts-c4/contracts/party/PartyGovernance.sol::350 => returns (uint96 votingPower)
party-contracts-c4/contracts/party/PartyGovernance.sol::364 => returns (uint96 votingPower)
party-contracts-c4/contracts/party/PartyGovernance.sol::422 => function findVotingPowerSnapshotIndex(address voter, uint40 timestamp)
party-contracts-c4/contracts/party/PartyGovernance.sol::539 => proposedTime: uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol::594 => uint96 votingPower = getVotingPowerAt(msg.sender, values.proposedTime, snapIndex);
party-contracts-c4/contracts/party/PartyGovernance.sol::605 => info.values.passedTime = uint40(block.timestamp);
party-contracts-c4/contracts/party/PartyGovernance.sol::684 => uint40(block.timestamp)
party-contracts-c4/contracts/party/PartyGovernance.sol::687 => proposalState.values.executedTime = uint40(block.timestamp);
party-contracts-c4/contracts/party/PartyGovernance.sol::695 => proposalState.values.completedTime = uint40(block.timestamp);
party-contracts-c4/contracts/party/PartyGovernance.sol::755 => uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol::756 => uint40(cancelTime)
party-contracts-c4/contracts/party/PartyGovernance.sol::762 => proposalState.values.completedTime = uint40(block.timestamp | UINT40_HIGH_BIT);
party-contracts-c4/contracts/party/PartyGovernance.sol::903 => timestamp: uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol::940 => timestamp: uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol::953 => uint96 newDelegateDelegatedVotingPower =
party-contracts-c4/contracts/party/PartyGovernance.sol::963 => timestamp: uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol::1033 => if (pv.votes == uint96(int96(-1))) {
party-contracts-c4/contracts/party/PartyGovernance.sol::1036 => uint40 t = uint40(block.timestamp);
party-contracts-c4/contracts/party/PartyGovernance.sol::1057 => function _isUnanimousVotes(uint96 totalVotes, uint96 totalVotingPower)
party-contracts-c4/contracts/party/PartyGovernance.sol::1070 => uint96 voteCount,
party-contracts-c4/contracts/party/PartyGovernance.sol::1071 => uint96 totalVotingPower,
party-contracts-c4/contracts/party/PartyGovernance.sol::1072 => uint16 passThresholdBps
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::52 => uint40 expiry;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::151 => uint40 zoraTimeout =
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::152 => uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_ZORA_AUCTION_TIMEOUT));
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::153 => uint40 zoraDuration =
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::154 => uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_ZORA_AUCTION_DURATION));
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::179 => abi.decode(params.progressData, (uint8, ZoraProgressData));
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::202 => uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MIN_ORDER_DURATION));
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::203 => uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MAX_ORDER_DURATION));
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::225 => abi.decode(params.progressData, (uint8, OpenseaProgressData));
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::34 => uint40 timeout;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::36 => uint40 duration;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::50 => uint40 duration,
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::51 => uint40 timeoutTime
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::53 => event ZoraAuctionExpired(uint256 auctionId, uint256 expiry);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::54 => event ZoraAuctionSold(uint256 auctionId);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::55 => event ZoraAuctionFailed(uint256 auctionId);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::94 => uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MIN_AUCTION_DURATION));
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::95 => uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_DURATION));
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::102 => uint40 maxTimeout = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_TIMEOUT));
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::133 => uint40 timeout,
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::135 => uint40 duration,
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::158 => uint40(duration),
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::159 => uint40(block.timestamp + timeout)
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::166 => uint40 minExpiry,
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::188 => if (minExpiry > uint40(block.timestamp)) {
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::29 => error UnsupportedProposalTypeError(uint32 proposalType);
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::221 => revert UnsupportedProposalTypeError(uint32(pt));
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::247 => require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol::25 => uint8 curatorFeePercentage;
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol::44 => uint8 curatorFeePercentages,
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol::52 => function minBidIncrementPercentage() external view returns (uint8);
```




### [G10] Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant`

#### Impact
See [this](https://github.com/ethereum/solidity/issues/9232) issue for a detail description of the issue
#### Findings:
```
party-contracts-c4/contracts/party/PartyGovernance.sol::405 => bytes32 dataHash = keccak256(proposal.proposalData);
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::121 => bytes32 resultHash = keccak256(r);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::184 => bytes32 errHash = keccak256(errData);
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::150 => bytes32 progressDataHash = keccak256(params.progressData);
```




### [G11] Duplicated `require()`/`revert()` checks should be refactored to a modifier or function


#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::154 => revert WrongLifecycleError(lc);
party-contracts-c4/contracts/party/PartyGovernance.sol::675 => revert BadProposalStatusError(status);
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::227 => revert InvalidApprovalCallLength(callData.length);
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::144 => revert ProposalProgressDataInvalidError(
```

### [G12] Use custom errors rather than `revert()`/`require()` strings to save deployment gas


#### Findings:
```
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::29 => revert('ALWAYS FAILING');
```




### [G13] Functions guaranteed to revert when called by normal users can be marked `payable`

#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::149 => function bid() external onlyDelegateCall {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::327 => function disableEmergencyActions() onlyPartyDao external {
party-contracts-c4/contracts/globals/Globals.sol::27 => function transferMultiSig(address newMultiSig) external onlyMultisig {
party-contracts-c4/contracts/globals/Globals.sol::59 => function setBytes32(uint256 key, bytes32 value) external onlyMultisig {
party-contracts-c4/contracts/globals/Globals.sol::63 => function setUint256(uint256 key, uint256 value) external onlyMultisig {
party-contracts-c4/contracts/globals/Globals.sol::67 => function setAddress(uint256 key, address value) external onlyMultisig {
party-contracts-c4/contracts/globals/Globals.sol::71 => function setIncludesBytes32(uint256 key, bytes32 value, bool isIncluded) external onlyMultisig {
party-contracts-c4/contracts/globals/Globals.sol::75 => function setIncludesUint256(uint256 key, uint256 value, bool isIncluded) external onlyMultisig {
party-contracts-c4/contracts/globals/Globals.sol::79 => function setIncludesAddress(uint256 key, address value, bool isIncluded) external onlyMultisig {
party-contracts-c4/contracts/party/PartyGovernance.sol::451 => function delegateVotingPower(address delegate) external onlyDelegateCall {
party-contracts-c4/contracts/party/PartyGovernance.sol::458 => function abdicate(address newPartyHost) external onlyHost onlyDelegateCall {
party-contracts-c4/contracts/party/PartyGovernance.sol::616 => function veto(uint256 proposalId) external onlyHost onlyDelegateCall {
party-contracts-c4/contracts/party/PartyGovernance.sol::800 => function disableEmergencyExecute() external onlyPartyDaoOrHost onlyDelegateCall {
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::169 => function abdicate() external onlyMinter onlyDelegateCall {
```




### [G14] Use a more recent version of solidity

#### Impact
Use a solidity version of at least 0.8.15 to have external calls skip
 contract existence checks if the external call has a return value
#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/ITokenDistributorParty.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/gatekeepers/IGateKeeper.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/globals/Globals.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/globals/IGlobals.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/market-wrapper/IMarketWrapper.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/IPartyFactory.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/Party.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyFactory.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyGovernance.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/IProposalExecutionEngine.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalStorage.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/FractionalV1.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/IOpenseaConduitController.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/IOpenseaExchange.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/EIP165.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/ERC1155Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/ERC721Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC1155.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC20.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC721.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC721Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/Implementation.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/Proxy.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol::2 => pragma solidity ^0.8;
```




### [G15] Bitshift for divide by 2

#### Impact
When multiply or dividing by a power of two, it is cheaper to bitshift than to use standard math operations.
#### Findings:
```
party-contracts-c4/contracts/party/PartyGovernance.sol::434 => uint256 mid = (low + high) / 2;
```




### [G16] Use simple comparison in trinary logic

#### Impact
The comparison operators >= and <= use more gas than >, 
<, or ==. Replacing the  >= and ≤ operators with a comparison 
operator that has an opcode in the EVM saves gas.
#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::276 => if (block.timestamp >= expiry) {
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::159 => if (block.timestamp >= expiry) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::350 => if (c.previousTotalContributions >= totalEthUsed) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::353 => } else if (c.previousTotalContributions + c.amount <= totalEthUsed) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::423 => if (numContributions >= 1) {
party-contracts-c4/contracts/party/PartyGovernance.sol::860 => snaps[hintIndex].timestamp <= timestamp &&
party-contracts-c4/contracts/party/PartyGovernance.sol::1040 => if (pv.passedTime + gv.executionDelay <= t) {
party-contracts-c4/contracts/party/PartyGovernance.sol::1051 => if (pv.proposedTime + gv.voteDuration <= t) {
party-contracts-c4/contracts/party/PartyGovernance.sol::1066 => return acceptanceRatio >= 0.9999e4;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::156 => if (call.data.length >= 4) {
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::364 => } else if (expiry <= block.timestamp) {
```




### [G17] Use `calldata` instead of `memory` for function parameters

#### Impact
Use calldata instead of memory for function parameters. Having function arguments use calldata instead of memory can save gas.

    There are several cases of function arguments using memory instead of calldata
#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::110 => function initialize(AuctionCrowdfundOptions memory opts)
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::196 => function finalize(FixedGovernanceOpts memory governanceOpts)
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::68 => function initialize(BuyCrowdfundOptions memory opts)
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::70 => function _initialize(BuyCrowdfundBaseOptions memory opts)
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::83 => function initialize(CollectionBuyCrowdfundOptions memory opts)
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::118 => FixedGovernanceOpts memory governanceOpts
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::124 => function _initialize(CrowdfundOptions memory opts)
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::191 => function contribute(address delegate, bytes memory gateData)
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::322 => function _hashFixedGovernanceOpts(FixedGovernanceOpts memory opts)
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::39 => function _initialize(string memory name_, string memory symbol_)
party-contracts-c4/contracts/distribution/TokenDistributor.sol::331 => function _createDistribution(CreateDistributionArgs memory args)
party-contracts-c4/contracts/distribution/TokenDistributor.sol::333 => returns (DistributionInfo memory info)
party-contracts-c4/contracts/distribution/TokenDistributor.sol::390 => function _getDistributionHash(DistributionInfo memory info)
party-contracts-c4/contracts/party/Party.sol::33 => function initialize(PartyInitData memory initData)
party-contracts-c4/contracts/party/PartyGovernance.sol::394 => function getProposalHash(Proposal memory proposal)
party-contracts-c4/contracts/party/PartyGovernance.sol::526 => function propose(Proposal memory proposal, uint256 latestSnapIndex)
party-contracts-c4/contracts/party/PartyGovernance.sol::973 => function _insertVotingPowerSnapshot(address voter, VotingPowerSnapshot memory snap)
party-contracts-c4/contracts/party/PartyGovernance.sol::1001 => function _getProposalFlags(ProposalStateValues memory pv)
party-contracts-c4/contracts/party/PartyGovernance.sol::1012 => function _getProposalStatus(ProposalStateValues memory pv)
party-contracts-c4/contracts/party/PartyGovernance.sol::1127 => function _validateProposalHash(Proposal memory proposal, bytes32 expectedHash)
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::203 => function _decodeApproveCallDataArgs(bytes memory callData)
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::221 => function _decodeSetApprovalForAllCallDataArgs(bytes memory callData)
party-contracts-c4/contracts/proposals/IProposalExecutionEngine.sol::18 => function initialize(address oldImpl, bytes memory initData) external;
party-contracts-c4/contracts/proposals/IProposalExecutionEngine.sol::29 => function executeProposal(ExecuteProposalParams memory params)
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::328 => function _getOrderHash(IOpenseaExchange.OrderParameters memory orderParams)
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::116 => function executeProposal(ExecuteProposalParams memory params)
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::205 => function _execute(ProposalType pt, ExecuteProposalParams memory params)
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::208 => returns (bytes emory nextProgressData)
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::228 => function _extractProposalType(bytes memory proposalData)
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::251 => function _executeUpgradeProposalsImplementation(bytes memory proposalData)
party-contracts-c4/contracts/proposals/ProposalStorage.sol::33 => function _initProposalImpl(IProposalExecutionEngine impl, bytes memory initData)
party-contracts-c4/contracts/tokens/ERC721Receiver.sol::14 => function onERC721Received(address, address, uint256, bytes memory)
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::8 => function delegateCallAndRevert(address impl, bytes memory callData)
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::18 => function delegateCallAndRevert(address impl, bytes memory callData) external {
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::27 => function _readOnlyDelegateCall(address impl, bytes memory callData) internal view {
```







### [G18] Amounts should be checked for 0 before calling a transfer

#### Impact
Checking non-zero transfer values can avoid an expensive external call and save gas.

While this is done at some places, it’s not consistently done in the solution.

I suggest adding a non-zero-value check here
#### Findings:
```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::301 => preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::143 => super.transferFrom(owner, to, tokenId);
```




### [G19] Unnecessary `initialize()` function

#### Impact
The initialize() function isn’t an initializer. 
#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::110 => function initialize(AuctionCrowdfundOptions memory opts)
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::68 => function initialize(BuyCrowdfundOptions memory opts)
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::83 => function initialize(CollectionBuyCrowdfundOptions memory opts)
party-contracts-c4/contracts/party/Party.sol::33 => function initialize(PartyInitData memory initData)
party-contracts-c4/contracts/proposals/IProposalExecutionEngine.sol::18 => function initialize(address oldImpl, bytes memory initData) external;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::99 => function initialize(address oldImpl, bytes calldata initializeData)
```


### [G20] Multiple `address` mappings can be combined into a single `mapping` of an `address` to a `struct`, where appropriate

#### Impact
Saves a storage slot for the mapping. Depending on the circumstances 
    and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. 
    Reads and subsequent writes can also be cheaper when a function requires
     both values and they both fit in the same storage slot
#### Findings:
```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::112 => mapping(address => address) public delegationsByContributor;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::115 => mapping (address => Contribution[]) private _contributionsByContributor;'

party-contracts-c4/contracts/distribution/TokenDistributor.sol::64 => mapping(ITokenDistributorParty => uint256) public lastDistributionIdPerParty;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::71 => mapping(ITokenDistributorParty => mapping(uint256 => DistributionState)) private _distributionStates;

party-contracts-c4/contracts/globals/Globals.sol::10 => mapping(uint256 => bytes32) private _wordValues;
party-contracts-c4/contracts/globals/Globals.sol::12 => mapping(uint256 => mapping(bytes32 => bool)) private _includedWordValues;

party-contracts-c4/contracts/party/PartyGovernance.sol::148 => mapping (address => bool) hasVoted;
party-contracts-c4/contracts/party/PartyGovernance.sol::207 => mapping(address => bool) public isHost;
party-contracts-c4/contracts/party/PartyGovernance.sol::209 => mapping(address => address) public delegationsByVoter;
party-contracts-c4/contracts/party/PartyGovernance.sol::213 => mapping(uint256 => ProposalState) private _proposalStateByProposalId;
party-contracts-c4/contracts/party/PartyGovernance.sol::215 => mapping(address => VotingPowerSnapshot[]) private _votingPowerSnapshotsByVoter;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::33 => mapping (uint256 => uint256) public votingPowerByTokenId;
```


### [G21] Using `bools` for storage incurs overhead


#### Findings:
```

party-contracts-c4/contracts/crowdfund/Crowdfund.sol::106 => bool private _splitRecipientHasBurned;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::30 => mapping(uint256 => bool) hasPartyTokenClaimed;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::62 => bool public emergencyActionsDisabled;
party-contracts-c4/contracts/globals/Globals.sol::12 => mapping(uint256 => mapping(bytes32 => bool)) private _includedWordValues;
party-contracts-c4/contracts/party/PartyGovernance.sol::148 => mapping (address => bool) hasVoted;
party-contracts-c4/contracts/party/PartyGovernance.sol::197 => bool public emergencyExecuteDisabled;
party-contracts-c4/contracts/party/PartyGovernance.sol::207 => mapping(address => bool) public isHost;
```


### [G22] Usage of `assert()` instead of `require()`

#### Impact
Between solc 0.4.10 and 0.8.0, require() used REVERT (0xfd) opcode which refunded remaining gas on failure while assert() used INVALID (0xfe) opcode which consumed all the supplied gas. (see https://docs.soliditylang.org/en/v0.8.1/control-structures.html#error-handling-assert-require-revert-and-exceptions).require() should be used for checking error conditions on inputs and return values while assert() should be used for invariant checking (properly functioning code should never reach a failing assert statement, unless there is a bug in your contract you should fix).
From the current usage of assert, my guess is that they can be replaced with require, unless a Panic really is intended.
#### Findings:
```
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::166 => assert(false); // Will not be reached.
party-contracts-c4/contracts/distribution/TokenDistributor.sol::385 => assert(tokenType == TokenType.Erc20);
party-contracts-c4/contracts/distribution/TokenDistributor.sol::411 => assert(tokenType == TokenType.Erc20);
party-contracts-c4/contracts/party/PartyGovernance.sol::504 => assert(tokenType == ITokenDistributor.TokenType.Erc20);
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::179 => assert(false); // Will not be reached.
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol::67 => assert(vault.balanceOf(address(this)) == supply);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::221 => assert(step == ListOnOpenseaStep.ListedOnOpenSea);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::302 => assert(SEAPORT.validate(orders));
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::120 => assert(step == ZoraStep.ListedOnZora);
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::142 => assert(currentInProgressProposalId == 0);
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::30 => assert(false);
```





### [G23] Empty blocks should be removed or emit something

#### Impact
The code should be refactored such that they no longer exist, or the 
block should do something useful, such as emitting an event or 
reverting. If the contract is meant to be extended, the contract should 
be abstract and the function signatures be added without 
any default implementation. If the block is an empty if-statement block 
to avoid doing subsequent checks in the else-if/else conditions, the 
else-if/else conditions should be nested under the negation of the 
if-statement, because they involve different classes of checks, which 
may lead to the introduction of errors when the code is later modified (`if(x){}else if(y){...}else{...}` => `if(!x){if(y){...}else{...}}')
#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::104 => constructor(IGlobals globals) Crowdfund(globals) {}
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::144 => receive() external payable {}
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::62 => constructor(IGlobals globals) BuyCrowdfundBase(globals) {}
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::67 => constructor(IGlobals globals) Crowdfund(globals) {}
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::77 => constructor(IGlobals globals) BuyCrowdfundBase(globals) {}
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::52 => {}
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::59 => {}
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::66 => {}
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::73 => {}
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::80 => {}
party-contracts-c4/contracts/party/Party.sol::28 => constructor(IGlobals globals) PartyGovernanceNFT(globals) {}
party-contracts-c4/contracts/party/Party.sol::47 => receive() external payable {}
```




### [G24] `abi.encode()` is less efficient than abi.encodePacked()


#### Findings:
```
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::164 => return abi.encode(ListOnOpenseaStep.ListedOnZora, ZoraProgressData({
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::219 => return abi.encode(ListOnOpenseaStep.ListedOnOpenSea, orderHash, expiry);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::115 => return abi.encode(ZoraStep.ListedOnZora, ZoraProgressData({
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::23 => abi.encode(s, r).rawRevert();
```


### [G25] Multiplication/division by two should use bit shifting

#### Impact
<x> * 2 is equivalent to <x> << 1 and <x> / 2 is the same as <x> >> 1. The MUL and DIV opcodes cost 5 gas, whereas SHL and SHR only cost 3 gas
#### Findings:
```
party-contracts-c4/contracts/party/PartyGovernance.sol::434 => uint256 mid = (low + high) / 2;
```




### [G26] Optimize names to save gas

#### Impact
be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9)
 link for an example of how it works. Below are the interfaces/abstract 
contracts that can be optimized so that the most frequently-called 
functions use the least amount of gas possible during method lookup. 
Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, per sorted position shifted
#### Findings:
```
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::15 => abstract contract BuyCrowdfundBase is Implementation, Crowdfund {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::17 => abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
party-contracts-c4/contracts/party/PartyGovernance.sol::24 => abstract contract PartyGovernance is
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::17 => abstract contract ListOnOpenseaProposal is ZoraHelpers {
party-contracts-c4/contracts/utils/Implementation.sol::5 => abstract contract Implementation {
```




### [G27] Use `Use assembly to write storage values


#### Findings:
```
party-contracts-c4/contracts/distribution/TokenDistributor.sol::395-401 =>  

     'assembly {
        hash := and(
            keccak256(info, 0xe0),
            0xffffffffffffffffffffffffffffff0000000000000000000000000000000000
        )
    }
    }'


party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::141-238 => 

     '    function _isCallAllowed(
        ArbitraryCall memory call,
        bool isUnanimous,
        IERC721[] memory preciousTokens,
        uint256[] memory preciousTokenIds
    )
        private
        view
        returns (bool isAllowed)
    {
        // Cannot call ourselves.
        if (call.target == address(this)) {
            return false;
        }
        if (call.data.length >= 4) {
            // Get the function selector of the call (first 4 bytes of calldata).
            bytes4 selector;
            {
                bytes memory callData = call.data;
                assembly {
                    selector := and(
                        mload(add(callData, 32)),
                        0xffffffff00000000000000000000000000000000000000000000000000000000
                    )
                }
            }
            // Non-unanimous proposals restrict what ways some functions can be
            // called on a precious token.
            if (!isUnanimous) {
                // Cannot call `approve()` or `setApprovalForAll()` on the precious
                // unless it's to revoke approvals.
                if (selector == IERC721.approve.selector) {
                    // Can only call `approve()` on the precious if the operator is null.
                    (address op, uint256 tokenId) = _decodeApproveCallDataArgs(call.data);
                    if (op != address(0)) {
                        return !LibProposal.isTokenIdPrecious(
                            IERC721(call.target),
                            tokenId,
                            preciousTokens,
                            preciousTokenIds
                        );
                    }
                // Can only call `setApprovalForAll()` on the precious if
                // toggling off.
                } else if (selector == IERC721.setApprovalForAll.selector) {
                    (, bool isApproved) = _decodeSetApprovalForAllCallDataArgs(call.data);
                    if (isApproved) {
                        return !LibProposal.isTokenPrecious(IERC721(call.target), preciousTokens);
                    }
                }
            }
            // Can never call `onERC721Received()` on any target.
            if (selector == IERC721Receiver.onERC721Received.selector) {
               return false;
           }
        }
        // All other calls are allowed.
        return true;
    }

    // Get the `operator` and `tokenId` from the `approve()` call data.
    function _decodeApproveCallDataArgs(bytes memory callData)
        private
        pure
        returns (address operator, uint256 tokenId)
    {
        if (callData.length < 68) {
            revert InvalidApprovalCallLength(callData.length);
        }
        assembly {
            operator := and(
                mload(add(callData, 36)),
                0xffffffffffffffffffffffffffffffffffffffff
            )
            tokenId := mload(add(callData, 68))
        }
    }

    // Get the `operator` and `tokenId` from the `setApprovalForAll()` call data.
    function _decodeSetApprovalForAllCallDataArgs(bytes memory callData)
        private
        pure
        returns (address operator, bool isApproved)
    {
        if (callData.length < 68) {
            revert InvalidApprovalCallLength(callData.length);
        }
        assembly {
            operator := and(
                mload(add(callData, 36)),
                0xffffffffffffffffffffffffffffffffffffffff
            )
            isApproved := xor(iszero(mload(add(callData, 68))), 1)
        }
    }

}'
```


