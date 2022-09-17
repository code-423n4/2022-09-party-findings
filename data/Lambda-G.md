- In `PartyGovernance.findVotingPowerSnapshotIndex`, a binary search is performed for the (common) case that `snaps[snaps.length - 1].timestamp <= timestamp`, i.e. when the timestamp is more recent than the latest checkpoint. Most other implementations (e.g, Compound) explicitly check this before performing a binary search, because it saves gas in expectation.
- In `Crowdfund._hashFixedGovernanceOpts`, `mload(opts)` is executed multiple times (https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L331) instead of using the cached result.
- In the for loops, the loop iteration can be marked as `unchecked` because an overflow is not possible (as the iterator is bounded):
```
contracts/party/PartyGovernance.sol:306:        for (uint256 i=0; i < opts.hosts.length; ++i) {
contracts/distribution/TokenDistributor.sol:230:        for (uint256 i = 0; i < infos.length; ++i) {
contracts/distribution/TokenDistributor.sol:239:        for (uint256 i = 0; i < infos.length; ++i) {
contracts/crowdfund/Crowdfund.sol:180:        for (uint256 i = 0; i < contributors.length; ++i) {
contracts/crowdfund/Crowdfund.sol:242:        for (uint256 i = 0; i < numContributions; ++i) {
contracts/crowdfund/Crowdfund.sol:300:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
contracts/crowdfund/Crowdfund.sol:348:            for (uint256 i = 0; i < numContributions; ++i) {
contracts/proposals/LibProposal.sol:14:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
contracts/proposals/LibProposal.sol:32:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
contracts/proposals/ArbitraryCallsProposal.sol:52:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
contracts/proposals/ArbitraryCallsProposal.sol:61:        for (uint256 i = 0; i < calls.length; ++i) {
contracts/proposals/ArbitraryCallsProposal.sol:78:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
contracts/proposals/ListOnOpenseaProposal.sol:291:            for (uint256 i = 0; i < fees.length; ++i) {
```
