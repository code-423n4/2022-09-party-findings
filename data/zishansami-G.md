## Gas Optimizations
 *** 


### G-01: pre-increment `++i/--i` costs less gas than post-increment `i++/i--`
Saves 6 gas per loop in a for loop

Total instances of this issue: 1

instance #1
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62
```solidity
contracts/crowdfund/CollectionBuyCrowdfund.sol

62:        for (uint256 i; i < hosts.length; i++) {

```

 *** 


### G-02: Length of the array (`<array>.length`) need not be looked up in every iteration of a for-loop
Reading array length at each iteration of the loop takes total 6 gas (3 for mload and 3 to place memory_offset) in the stack.
Caching the `array.length` saves around `3 gas` per iteration.

Total instances of this issue: 12

instance #1
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62
```solidity
contracts/crowdfund/CollectionBuyCrowdfund.sol

62:        for (uint256 i; i < hosts.length; i++) {

```

instance #2
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180
```solidity
contracts/crowdfund/Crowdfund.sol

180:        for (uint256 i = 0; i < contributors.length; ++i) {

```

instance #3
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300
```solidity
contracts/crowdfund/Crowdfund.sol

300:        for (uint256 i = 0; i < preciousTokens.length; ++i) {

```

instance #4
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230
```solidity
contracts/distribution/TokenDistributor.sol

230:        for (uint256 i = 0; i < infos.length; ++i) {

```

instance #5
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239
```solidity
contracts/distribution/TokenDistributor.sol

239:        for (uint256 i = 0; i < infos.length; ++i) {

```

instance #6
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L306
```solidity
contracts/party/PartyGovernance.sol

306:        for (uint256 i=0; i < opts.hosts.length; ++i) {

```

instance #7
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52
```solidity
contracts/proposals/ArbitraryCallsProposal.sol

52:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {

```

instance #8
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61
```solidity
contracts/proposals/ArbitraryCallsProposal.sol

61:        for (uint256 i = 0; i < calls.length; ++i) {

```

instance #9
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78
```solidity
contracts/proposals/ArbitraryCallsProposal.sol

78:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {

```

instance #10
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L14
```solidity
contracts/proposals/LibProposal.sol

14:        for (uint256 i = 0; i < preciousTokens.length; ++i) {

```

instance #11
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L32
```solidity
contracts/proposals/LibProposal.sol

32:        for (uint256 i = 0; i < preciousTokens.length; ++i) {

```

instance #12
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291
```solidity
contracts/proposals/ListOnOpenseaProposal.sol

291:            for (uint256 i = 0; i < fees.length; ++i) {

```

 *** 


### G-03: `x += y` costs more gas than `x = x + y` for state variables

Total instances of this issue: 7

instance #1
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L243
```solidity
contracts/crowdfund/Crowdfund.sol

243:            ethContributed += contributions[i].amount;

```

instance #2
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L352
```solidity
contracts/crowdfund/Crowdfund.sol

352:                    ethOwed += c.amount;

```

instance #3
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L355
```solidity
contracts/crowdfund/Crowdfund.sol

355:                    ethUsed += c.amount;

```

instance #4
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L359
```solidity
contracts/crowdfund/Crowdfund.sol

359:                    ethUsed += partialEthUsed;

```

instance #5
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L411
```solidity
contracts/crowdfund/Crowdfund.sol

411:            totalContributions += amount;

```

instance #6
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L427
```solidity
contracts/crowdfund/Crowdfund.sol

427:                    lastContribution.amount += amount;

```

instance #7
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L595
```solidity
contracts/party/PartyGovernance.sol

595:        values.votes += votingPower;

```

 *** 


### G-04: No need to initialize non-constant/non-immutable variables to zero
Since the default value is already zero, overwriting is not required.
Saves `8 gas` per instance.

Total instances of this issue: 14

instance #1
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180
```solidity
contracts/crowdfund/Crowdfund.sol

180:        for (uint256 i = 0; i < contributors.length; ++i) {

```

instance #2
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L242
```solidity
contracts/crowdfund/Crowdfund.sol

242:        for (uint256 i = 0; i < numContributions; ++i) {

```

instance #3
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300
```solidity
contracts/crowdfund/Crowdfund.sol

300:        for (uint256 i = 0; i < preciousTokens.length; ++i) {

```

instance #4
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L348
```solidity
contracts/crowdfund/Crowdfund.sol

348:            for (uint256 i = 0; i < numContributions; ++i) {

```

instance #5
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230
```solidity
contracts/distribution/TokenDistributor.sol

230:        for (uint256 i = 0; i < infos.length; ++i) {

```

instance #6
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239
```solidity
contracts/distribution/TokenDistributor.sol

239:        for (uint256 i = 0; i < infos.length; ++i) {

```

instance #7
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L306
```solidity
contracts/party/PartyGovernance.sol

306:        for (uint256 i=0; i < opts.hosts.length; ++i) {

```

instance #8
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L432
```solidity
contracts/party/PartyGovernance.sol

432:        uint256 low = 0;

```

instance #9
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52
```solidity
contracts/proposals/ArbitraryCallsProposal.sol

52:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {

```

instance #10
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61
```solidity
contracts/proposals/ArbitraryCallsProposal.sol

61:        for (uint256 i = 0; i < calls.length; ++i) {

```

instance #11
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78
```solidity
contracts/proposals/ArbitraryCallsProposal.sol

78:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {

```

instance #12
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L14
```solidity
contracts/proposals/LibProposal.sol

14:        for (uint256 i = 0; i < preciousTokens.length; ++i) {

```

instance #13
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L32
```solidity
contracts/proposals/LibProposal.sol

32:        for (uint256 i = 0; i < preciousTokens.length; ++i) {

```

instance #14
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291
```solidity
contracts/proposals/ListOnOpenseaProposal.sol

291:            for (uint256 i = 0; i < fees.length; ++i) {

```

 *** 


### G-05: Using uints/ints smaller than 256 bits increases overhead
Gas usage becomes higher with uint/int smaller than 256 bits because EVM operates on 32 bytes and uses additional operations to reduce the size from 32 bytes to the target size.

Total instances of this issue: 109

instance #1
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L48
```solidity
contracts/crowdfund/AuctionCrowdfund.sol

48:        uint40 duration;

```

instance #2
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L50
```solidity
contracts/crowdfund/AuctionCrowdfund.sol

50:        uint96 maximumBid;

```

instance #3
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L56
```solidity
contracts/crowdfund/AuctionCrowdfund.sol

56:        uint16 splitBps;

```

instance #4
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L94
```solidity
contracts/crowdfund/AuctionCrowdfund.sol

94:    uint96 public maximumBid;

```

instance #5
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L96
```solidity
contracts/crowdfund/AuctionCrowdfund.sol

96:    uint96 public lastBid;

```

instance #6
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L99
```solidity
contracts/crowdfund/AuctionCrowdfund.sol

99:    uint40 public expiry;

```

instance #7
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L170
```solidity
contracts/crowdfund/AuctionCrowdfund.sol

170:        uint96 bidAmount = market.getMinimumBid(auctionId_).safeCastUint256ToUint96();

```

instance #8
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L210
```solidity
contracts/crowdfund/AuctionCrowdfund.sol

210:        uint96 lastBid_ = lastBid;

```

instance #9
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L30
```solidity
contracts/crowdfund/BuyCrowdfund.sol

30:        uint40 duration;

```

instance #10
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L33
```solidity
contracts/crowdfund/BuyCrowdfund.sol

33:        uint96 maximumPrice;

```

instance #11
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L39
```solidity
contracts/crowdfund/BuyCrowdfund.sol

39:        uint16 splitBps;

```

instance #12
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L98-L106
```solidity
contracts/crowdfund/BuyCrowdfund.sol

98:    function buy(
99:        address payable callTarget,
100:        uint96 callValue,
101:        bytes calldata callData,
102:        FixedGovernanceOpts memory governanceOpts
103:    )
104:        external
105:        returns (Party party_)
106:    {

```

instance #13
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L27
```solidity
contracts/crowdfund/BuyCrowdfundBase.sol

27:        uint40 duration;

```

instance #14
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L30
```solidity
contracts/crowdfund/BuyCrowdfundBase.sol

30:        uint96 maximumPrice;

```

instance #15
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L36
```solidity
contracts/crowdfund/BuyCrowdfundBase.sol

36:        uint16 splitBps;

```

instance #16
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L54
```solidity
contracts/crowdfund/BuyCrowdfundBase.sol

54:    error MaximumPriceError(uint96 callValue, uint96 maximumPrice);

```

instance #17
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L60
```solidity
contracts/crowdfund/BuyCrowdfundBase.sol

60:    uint40 public expiry;

```

instance #18
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L62
```solidity
contracts/crowdfund/BuyCrowdfundBase.sol

62:    uint96 public maximumPrice;

```

instance #19
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L64
```solidity
contracts/crowdfund/BuyCrowdfundBase.sol

64:    uint96 public settledPrice;

```

instance #20
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L90-L101
```solidity
contracts/crowdfund/BuyCrowdfundBase.sol

90:    function _buy(
91:        IERC721 token,
92:        uint256 tokenId,
93:        address payable callTarget,
94:        uint96 callValue,
95:        bytes calldata callData,
96:        FixedGovernanceOpts memory governanceOpts
97:    )
98:        internal
99:        onlyDelegateCall
100:        returns (Party party_)
101:    {

```

instance #21
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L114
```solidity
contracts/crowdfund/BuyCrowdfundBase.sol

114:        uint96 settledPrice_;

```

instance #22
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L116
```solidity
contracts/crowdfund/BuyCrowdfundBase.sol

116:            uint96 maximumPrice_ = maximumPrice;

```

instance #23
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L31
```solidity
contracts/crowdfund/CollectionBuyCrowdfund.sol

31:        uint40 duration;

```

instance #24
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L34
```solidity
contracts/crowdfund/CollectionBuyCrowdfund.sol

34:        uint96 maximumPrice;

```

instance #25
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L40
```solidity
contracts/crowdfund/CollectionBuyCrowdfund.sol

40:        uint16 splitBps;

```

instance #26
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L113-L123
```solidity
contracts/crowdfund/CollectionBuyCrowdfund.sol

113:    function buy(
114:        uint256 tokenId,
115:        address payable callTarget,
116:        uint96 callValue,
117:        bytes calldata callData,
118:        FixedGovernanceOpts memory governanceOpts
119:    )
120:        external
121:        onlyHost(governanceOpts.hosts)
122:        returns (Party party_)
123:    {

```

instance #27
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L37
```solidity
contracts/crowdfund/Crowdfund.sol

37:        uint40 voteDuration;

```

instance #28
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L40
```solidity
contracts/crowdfund/Crowdfund.sol

40:        uint40 executionDelay;

```

instance #29
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L43
```solidity
contracts/crowdfund/Crowdfund.sol

43:        uint16 passThresholdBps;

```

instance #30
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L45
```solidity
contracts/crowdfund/Crowdfund.sol

45:        uint16 feeBps;

```

instance #31
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L55
```solidity
contracts/crowdfund/Crowdfund.sol

55:        uint16 splitBps;

```

instance #32
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L67
```solidity
contracts/crowdfund/Crowdfund.sol

67:        uint96 previousTotalContributions;

```

instance #33
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L69
```solidity
contracts/crowdfund/Crowdfund.sol

69:        uint96 amount;

```

instance #34
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L80
```solidity
contracts/crowdfund/Crowdfund.sol

80:    error InvalidBpsError(uint16 bps);

```

instance #35
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L93
```solidity
contracts/crowdfund/Crowdfund.sol

93:    uint96 public totalContributions;

```

instance #36
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L104
```solidity
contracts/crowdfund/Crowdfund.sol

104:    uint16 public splitBps;

```

instance #37
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L143
```solidity
contracts/crowdfund/Crowdfund.sol

143:        uint96 initialBalance = address(this).balance.safeCastUint256ToUint96();

```

instance #38
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L378-L386
```solidity
contracts/crowdfund/Crowdfund.sol

378:    function _contribute(
379:        address contributor,
380:        uint96 amount,
381:        address delegate,
382:        uint96 previousTotalContributions,
383:        bytes memory gateData
384:    )
385:        internal
386:    {

```

instance #39
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#L28
```solidity
contracts/distribution/ITokenDistributor.sol

28:        uint128 memberSupply;

```

instance #40
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#L30
```solidity
contracts/distribution/ITokenDistributor.sol

30:        uint128 fee;

```

instance #41
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#L62-L69
```solidity
contracts/distribution/ITokenDistributor.sol

62:    function createNativeDistribution(
63:        ITokenDistributorParty party,
64:        address payable feeRecipient,
65:        uint16 feeBps
66:    )
67:        external
68:        payable
69:        returns (DistributionInfo memory info);

```

instance #42
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#L81-L88
```solidity
contracts/distribution/ITokenDistributor.sol

81:    function createErc20Distribution(
82:        IERC20 token,
83:        ITokenDistributorParty party,
84:        address payable feeRecipient,
85:        uint16 feeBps
86:    )
87:        external
88:        returns (DistributionInfo memory info);

```

instance #43
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#L96-L98
```solidity
contracts/distribution/ITokenDistributor.sol

96:    function claim(DistributionInfo calldata info, uint256 partyTokenId)
97:        external
98:        returns (uint128 amountClaimed);

```

instance #44
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L24
```solidity
contracts/distribution/TokenDistributor.sol

24:        uint128 remainingMemberSupply;

```

instance #45
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L40
```solidity
contracts/distribution/TokenDistributor.sol

40:        uint16 feeBps;

```

instance #46
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L50
```solidity
contracts/distribution/TokenDistributor.sol

50:    error InvalidDistributionSupplyError(uint128 supply);

```

instance #47
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L52
```solidity
contracts/distribution/TokenDistributor.sol

52:    error InvalidFeeBpsError(uint16 feeBps);

```

instance #48
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L98-L106
```solidity
contracts/distribution/TokenDistributor.sol

98:    function createNativeDistribution(
99:        ITokenDistributorParty party,
100:        address payable feeRecipient,
101:        uint16 feeBps
102:    )
103:        external
104:        payable
105:        returns (DistributionInfo memory info)
106:    {

```

instance #49
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L118-L126
```solidity
contracts/distribution/TokenDistributor.sol

118:    function createErc20Distribution(
119:        IERC20 token,
120:        ITokenDistributorParty party,
121:        address payable feeRecipient,
122:        uint16 feeBps
123:    )
124:        external
125:        returns (DistributionInfo memory info)
126:    {

```

instance #50
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L138-L141
```solidity
contracts/distribution/TokenDistributor.sol

138:    function claim(DistributionInfo calldata info, uint256 partyTokenId)
139:        public
140:        returns (uint128 amountClaimed)
141:    {

```

instance #51
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L166
```solidity
contracts/distribution/TokenDistributor.sol

166:        uint128 remainingMemberSupply = state.remainingMemberSupply;

```

instance #52
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L338
```solidity
contracts/distribution/TokenDistributor.sol

338:        uint128 supply;

```

instance #53
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L352
```solidity
contracts/distribution/TokenDistributor.sol

352:        uint128 fee = supply * args.feeBps / 1e4;

```

instance #54
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L353
```solidity
contracts/distribution/TokenDistributor.sol

353:        uint128 memberSupply = supply - fee;

```

instance #55
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L75
```solidity
contracts/party/PartyGovernance.sol

75:        uint40 voteDuration;

```

instance #56
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L78
```solidity
contracts/party/PartyGovernance.sol

78:        uint40 executionDelay;

```

instance #57
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L81
```solidity
contracts/party/PartyGovernance.sol

81:        uint16 passThresholdBps;

```

instance #58
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L83
```solidity
contracts/party/PartyGovernance.sol

83:        uint96 totalVotingPower;

```

instance #59
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L85
```solidity
contracts/party/PartyGovernance.sol

85:        uint16 feeBps;

```

instance #60
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L93
```solidity
contracts/party/PartyGovernance.sol

93:        uint40 voteDuration;

```

instance #61
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L94
```solidity
contracts/party/PartyGovernance.sol

94:        uint40 executionDelay;

```

instance #62
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L95
```solidity
contracts/party/PartyGovernance.sol

95:        uint16 passThresholdBps;

```

instance #63
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L96
```solidity
contracts/party/PartyGovernance.sol

96:        uint96 totalVotingPower;

```

instance #64
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L102
```solidity
contracts/party/PartyGovernance.sol

102:        uint40 timestamp;

```

instance #65
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L104
```solidity
contracts/party/PartyGovernance.sol

104:        uint96 delegatedVotingPower;

```

instance #66
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L106
```solidity
contracts/party/PartyGovernance.sol

106:        uint96 intrinsicVotingPower;

```

instance #67
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L116
```solidity
contracts/party/PartyGovernance.sol

116:        uint40 maxExecutableTime;

```

instance #68
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L119
```solidity
contracts/party/PartyGovernance.sol

119:        uint40 cancelDelay;

```

instance #69
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L130
```solidity
contracts/party/PartyGovernance.sol

130:        uint40 proposedTime;

```

instance #70
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L132
```solidity
contracts/party/PartyGovernance.sol

132:        uint40 passedTime;

```

instance #71
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L134
```solidity
contracts/party/PartyGovernance.sol

134:        uint40 executedTime;

```

instance #72
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L136
```solidity
contracts/party/PartyGovernance.sol

136:        uint40 completedTime;

```

instance #73
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L176
```solidity
contracts/party/PartyGovernance.sol

176:    error ExecutionTimeExceededError(uint40 maxExecutableTime, uint40 timestamp);

```

instance #74
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L186
```solidity
contracts/party/PartyGovernance.sol

186:    error ProposalCannotBeCancelledYetError(uint40 currentTime, uint40 cancelTime);

```

instance #75
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L187
```solidity
contracts/party/PartyGovernance.sol

187:    error InvalidBpsError(uint16 bps);

```

instance #76
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L190
```solidity
contracts/party/PartyGovernance.sol

190:    uint96 constant private VETO_VALUE = uint96(int96(-1));

```

instance #77
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L199
```solidity
contracts/party/PartyGovernance.sol

199:    uint16 public feeBps;

```

instance #78
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L347-L351
```solidity
contracts/party/PartyGovernance.sol

347:    function getVotingPowerAt(address voter, uint40 timestamp)
348:        external
349:        view
350:        returns (uint96 votingPower)
351:    {

```

instance #79
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L361-L365
```solidity
contracts/party/PartyGovernance.sol

361:    function getVotingPowerAt(address voter, uint40 timestamp, uint256 snapIndex)
362:        public
363:        view
364:        returns (uint96 votingPower)
365:    {

```

instance #80
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L422-L426
```solidity
contracts/party/PartyGovernance.sol

422:    function findVotingPowerSnapshotIndex(address voter, uint40 timestamp)
423:        public
424:        view
425:        returns (uint256 index)
426:    {

```

instance #81
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L594
```solidity
contracts/party/PartyGovernance.sol

594:        uint96 votingPower = getVotingPowerAt(msg.sender, values.proposedTime, snapIndex);

```

instance #82
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L848-L852
```solidity
contracts/party/PartyGovernance.sol

848:    function _getVotingPowerSnapshotAt(address voter, uint40 timestamp, uint256 hintIndex)
849:        internal
850:        view
851:        returns (VotingPowerSnapshot memory snap)
852:    {

```

instance #83
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L884
```solidity
contracts/party/PartyGovernance.sol

884:        int192 powerI192 = power.safeCastUint256ToInt192();

```

instance #84
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L890-L892
```solidity
contracts/party/PartyGovernance.sol

890:    function _adjustVotingPower(address voter, int192 votingPower, address delegate)
891:        internal
892:    {

```

instance #85
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L953-L954
```solidity
contracts/party/PartyGovernance.sol

953:            uint96 newDelegateDelegatedVotingPower =
954:                newDelegateSnap.delegatedVotingPower + newSnap.intrinsicVotingPower;

```

instance #86
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1036
```solidity
contracts/party/PartyGovernance.sol

1036:        uint40 t = uint40(block.timestamp);

```

instance #87
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1057-L1061
```solidity
contracts/party/PartyGovernance.sol

1057:    function _isUnanimousVotes(uint96 totalVotes, uint96 totalVotingPower)
1058:        private
1059:        pure
1060:        returns (bool)
1061:    {

```

instance #88
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1069-L1077
```solidity
contracts/party/PartyGovernance.sol

1069:    function _areVotesPassing(
1070:        uint96 voteCount,
1071:        uint96 totalVotingPower,
1072:        uint16 passThresholdBps
1073:    )
1074:        private
1075:        pure
1076:        returns (bool)
1077:    {

```

instance #89
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L36
```solidity
contracts/proposals/ListOnOpenseaProposal.sol

36:        uint40 duration;

```

instance #90
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L52
```solidity
contracts/proposals/ListOnOpenseaProposal.sol

52:        uint40 expiry;

```

instance #91
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L151-L152
```solidity
contracts/proposals/ListOnOpenseaProposal.sol

151:                uint40 zoraTimeout =
152:                    uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_ZORA_AUCTION_TIMEOUT));

```

instance #92
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L153-L154
```solidity
contracts/proposals/ListOnOpenseaProposal.sol

153:                uint40 zoraDuration =
154:                    uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_ZORA_AUCTION_DURATION));

```

instance #93
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L202
```solidity
contracts/proposals/ListOnOpenseaProposal.sol

202:                uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MIN_ORDER_DURATION));

```

instance #94
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L203
```solidity
contracts/proposals/ListOnOpenseaProposal.sol

203:                uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MAX_ORDER_DURATION));

```

instance #95
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L34
```solidity
contracts/proposals/ListOnZoraProposal.sol

34:        uint40 timeout;

```

instance #96
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L36
```solidity
contracts/proposals/ListOnZoraProposal.sol

36:        uint40 duration;

```

instance #97
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L43
```solidity
contracts/proposals/ListOnZoraProposal.sol

43:    error ZoraListingNotExpired(uint256 auctionId, uint40 expiry);

```

instance #98
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L45-L52
```solidity
contracts/proposals/ListOnZoraProposal.sol

45:    event ZoraAuctionCreated(
46:        uint256 auctionId,
47:        IERC721 token,
48:        uint256 tokenId,
49:        uint256 startingPrice,
50:        uint40 duration,
51:        uint40 timeoutTime
52:    );

```

instance #99
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L94
```solidity
contracts/proposals/ListOnZoraProposal.sol

94:                uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MIN_AUCTION_DURATION));

```

instance #100
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L95
```solidity
contracts/proposals/ListOnZoraProposal.sol

95:                uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_DURATION));

```

instance #101
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L102
```solidity
contracts/proposals/ListOnZoraProposal.sol

102:                uint40 maxTimeout = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_TIMEOUT));

```

instance #102
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L135-L142
```solidity
contracts/proposals/ListOnZoraProposal.sol

135:        uint40 duration,
136:        IERC721 token,
137:        uint256 tokenId
138:    )
139:        internal
140:        override
141:        returns (uint256 auctionId)
142:    {

```

instance #103
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L164-L173
```solidity
contracts/proposals/ListOnZoraProposal.sol

164:    function _settleZoraAuction(
165:        uint256 auctionId,
166:        uint40 minExpiry,
167:        IERC721 token,
168:        uint256 tokenId
169:    )
170:        internal
171:        override
172:        returns (bool sold)
173:    {

```

instance #104
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L29
```solidity
contracts/proposals/ProposalExecutionEngine.sol

29:    error UnsupportedProposalTypeError(uint32 proposalType);

```

instance #105
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibSafeCast.sol#L7
```solidity
contracts/utils/LibSafeCast.sol

7:    error Int192ToUint96CastOutOfRange(int192 i192);

```

instance #106
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibSafeCast.sol#L33
```solidity
contracts/utils/LibSafeCast.sol

33:    function safeCastUint96ToInt192(uint96 v) internal pure returns (int192) {

```

instance #107
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibSafeCast.sol#L37
```solidity
contracts/utils/LibSafeCast.sol

37:    function safeCastInt192ToUint96(int192 i192) internal pure returns (uint96) {

```

instance #108
Link: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/markets/IZoraAuctionHouse.sol#L25
```solidity
contracts/vendor/markets/IZoraAuctionHouse.sol

25:        uint8 curatorFeePercentage;

```

instance #109
Permalink: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/markets/IZoraAuctionHouse.sol#L38-L46
```solidity
contracts/vendor/markets/IZoraAuctionHouse.sol

38:    function createAuction(
39:        uint256 tokenId,
40:        IERC721 tokenContract,
41:        uint256 duration,
42:        uint256 reservePrice,
43:        address payable curator,
44:        uint8 curatorFeePercentages,
45:        IERC20 auctionCurrency
46:    ) external returns (uint256);

```

