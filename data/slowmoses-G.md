## (1) Use pre increment instead of post increment

Pre-increment uses a little less gas.

***

for (uint256 i; i < hosts.length; i++) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62

## (2) Cache Array Length

Array length should be cached instead of requesting it over and over in a for loop.
The cost per iteration depends on the array type, 100 gas for storage, 3 each for memory and calldata.
Caching the value costs only 3 gas.

***

for (uint256 i; i < hosts.length; i++) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62

for (uint256 i = 0; i < hadPreciouses.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L52

for (uint256 i = 0; i < calls.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L61

for (uint256 i = 0; i < hadPreciouses.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L78

for (uint256 i = 0; i < infos.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L230

for (uint256 i = 0; i < infos.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L239

for (uint256 i = 0; i < fees.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L291

for (uint256 i = 0; i < contributors.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L180

for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L300

for (uint256 i=0; i < opts.hosts.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L306

for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L14

for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L32

## (3) Boolean variables should not be used for storage

Using UINT256(1) for true and UINT256(2) for false costs less gas than using boolean variables.

***

bool public emergencyExecuteDisabled;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L197

mapping(address => bool) public isHost;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L207

## (4) Do not Initialize to Default Value

Non-const variables should not be initialized to their default value.
Just using the default costs less gas.
For example use: uint256 abc;
Instead of: uint256 abc=0;

***

for (uint256 i = 0; i < hadPreciouses.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L52

for (uint256 i = 0; i < calls.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L61

for (uint256 i = 0; i < hadPreciouses.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L78

for (uint256 i = 0; i < infos.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L230

for (uint256 i = 0; i < infos.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L239

for (uint256 i = 0; i < fees.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L291

for (uint256 i = 0; i < contributors.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L180

for (uint256 i = 0; i < numContributions; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L242

for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L300

for (uint256 i = 0; i < numContributions; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L348

for (uint256 i=0; i < opts.hosts.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L306

uint256 low = 0;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L432

for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L14

for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L32

