##  CACHE THE LENGTH OF ARRAYS IN LOOPS
```solidity
contracts/crowdfund/CollectionBuyCrowdfund.sol::62 => for (uint256 i; i < hosts.length; i++) {
contracts/crowdfund/Crowdfund.sol::180 => for (uint256 i = 0; i < contributors.length; ++i) {
contracts/crowdfund/Crowdfund.sol::300 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
contracts/distribution/TokenDistributor.sol::230 => for (uint256 i = 0; i < infos.length; ++i) {
contracts/distribution/TokenDistributor.sol::239 => for (uint256 i = 0; i < infos.length; ++i) {
contracts/party/PartyGovernance.sol::306 => for (uint256 i=0; i < opts.hosts.length; ++i) {
contracts/proposals/ArbitraryCallsProposal.sol::52 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
contracts/proposals/ArbitraryCallsProposal.sol::61 => for (uint256 i = 0; i < calls.length; ++i) {
contracts/proposals/ArbitraryCallsProposal.sol::78 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
contracts/proposals/LibProposal.sol::14 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
contracts/proposals/LibProposal.sol::32 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
contracts/proposals/ListOnOpenseaProposal.sol::291 => for (uint256 i = 0; i < fees.length; ++i) {
```

&nbsp;
&nbsp;

##  `++I` COSTS LESS GAS THAN `I++`, ESPECIALLY WHEN IT’S USED IN `FOR`-LOOPS (`--I`/`I--` TOO)

```solidity
contracts/crowdfund/CollectionBuyCrowdfund.sol::62 =>         for (uint256 i; i < hosts.length; i++) {
```

&nbsp;
&nbsp;

##  `<X> -= <Y>`COSTS MORE GAS THAN `<X> = <X> - <Y>` FOR STATE VARIABLES

```solidity
contracts/distribution/TokenDistributor.sol
381:        _storedBalances[balanceId] -= amount;
```



&nbsp;
&nbsp;

##  Don't Initialize Variables with Default Value

```solidity
contracts/crowdfund/Crowdfund.sol::180 => for (uint256 i = 0; i < contributors.length; ++i) {
contracts/crowdfund/Crowdfund.sol::242 => for (uint256 i = 0; i < numContributions; ++i) {
contracts/crowdfund/Crowdfund.sol::300 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
contracts/crowdfund/Crowdfund.sol::348 => for (uint256 i = 0; i < numContributions; ++i) {
contracts/distribution/TokenDistributor.sol::230 => for (uint256 i = 0; i < infos.length; ++i) {
contracts/distribution/TokenDistributor.sol::239 => for (uint256 i = 0; i < infos.length; ++i) {
contracts/party/PartyGovernance.sol::306 => for (uint256 i=0; i < opts.hosts.length; ++i) {
contracts/party/PartyGovernance.sol::432 => uint256 low = 0;
contracts/proposals/ArbitraryCallsProposal.sol::52 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
contracts/proposals/ArbitraryCallsProposal.sol::61 => for (uint256 i = 0; i < calls.length; ++i) {
contracts/proposals/ArbitraryCallsProposal.sol::78 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
contracts/proposals/LibProposal.sol::14 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
contracts/proposals/LibProposal.sol::32 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
contracts/proposals/ListOnOpenseaProposal.sol::291 => for (uint256 i = 0; i < fees.length; ++i) {
```

&nbsp;
&nbsp;

##  COMPARISONS: != IS MORE EFFICIENT THAN > IN REQUIRE 

```solidity
contracts/crowdfund/Crowdfund.sol::144 => if (initialBalance > 0) {
contracts/crowdfund/Crowdfund.sol::471 => if (votingPower > 0) {
```

&nbsp;
&nbsp;


##  USING BOOLS FOR STORAGE INCURS OVERHEAD
```solidity
contracts/crowdfund/Crowdfund.sol
106:    bool private _splitRecipientHasBurned;

contracts/distribution/TokenDistributor.sol
28:        bool wasFeeClaimed;
30:        mapping(uint256 => bool) hasPartyTokenClaimed;
62:    bool public emergencyActionsDisabled;

contracts/globals/Global.sol
12:    mapping(uint256 => mapping(bytes32 => bool)) private _includedWordValues;

contracts/party/PartyGovernance.sol
108:        bool isDelegated;
197:    bool public emergencyExecuteDisabled;
207:    mapping(address => bool) public isHost;

contracts/vendor/markets/IZoraAuctionHouse.sol
15:        bool approved;
```


&nbsp;
&nbsp;

## `ABI.ENCODE()` IS LESS EFFICIENT THAN `ABI.ENCODEPACKED()`
```
contracts/proposals/ListOnOpenseaProposal.sol
164:                    return abi.encode(ListOnOpenseaStep.ListedOnZora, ZoraProgressData({
219:            return abi.encode(ListOnOpenseaStep.ListedOnOpenSea, orderHash, expiry);

contracts/proposals/ListOnZoraProposal.sol
115:            return abi.encode(ZoraStep.ListedOnZora, ZoraProgressData({

contracts/utils/ReadOnlyDelegateCall.sol
23:        abi.encode(s, r).rawRevert();
```

&nbsp;
&nbsp;

## DIVISION BY TWO SHOULD USE BIT SHIFTING

```solidity
contracts/party/PartyGovernance.sol::434 => uint256 mid = (low + high) / 2;
```



&nbsp;
&nbsp;

## MULTIPLE `ADDRESS` MAPPINGS CAN BE COMBINED INTO A SINGLE `MAPPING` OF AN `ADDRESS` TO A `STRUCT`, WHERE APPROPRIATE
```solidity
contracts/crowdfund/Crowdfund.sol
112:    mapping(address => address) public delegationsByContributor;
115:    mapping (address => Contribution[]) private _contributionsByContributor;

contracts/party/PartyGovernance.sol
148:        mapping (address => bool) hasVoted;
207:    mapping(address => bool) public isHost;
209:    mapping(address => address) public delegationsByVoter;
215:    mapping(address => VotingPowerSnapshot[]) private _votingPowerSnapshotsByVoter;
```