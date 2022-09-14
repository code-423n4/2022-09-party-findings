### [GO01] Don't Initialize Variables with Default Value

#### Impact

Uninitialized variables are assigned with the types default value.
Explicitly initializing a variable with it's default value costs unnecessary gas.

```solidity
// from
uint i = 0;
bool b = false;

// to
uint i;
bool b;
```

#### Findings:
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

### [GO02] Cache Array Length Outside of Loop

#### Impact

Cache the length of arrays in loops (~6 gas per iteration)
    The overheads outlined below are PER LOOP, excluding the first loop
    - storage arrays incur a Gwarmaccess (100 gas)
    - memory arrays use MLOAD (3 gas)
    - calldata arrays use CALLDATALOAD (3 gas)

Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset

```solidity
// from
for(uint i; i < someArray.length; ++i){

// to
uint256 length = someArray.length;
for(uint i; i < length; ++i){
```

#### Findings:
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


### [GO03] Use != 0 instead of > 0 for Unsigned Integer Comparison

#### Impact

When dealing with unsigned integer types, comparisons with `!= 0` are cheaper then with `> 0`.

```solidity
// from
 (amount > 0);

// to
(amount != 0);
```

#### Findings:
```solidity
contracts/crowdfund/Crowdfund.sol::144 => if (initialBalance > 0) {
contracts/crowdfund/Crowdfund.sol::471 => if (votingPower > 0) {
```
