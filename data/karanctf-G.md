## [G-1] Make increment `unchecked` for post increment:
```solidity
for (uint256 i = 0; i < length;) {
    // change it to this format
    unchecked { ++i; }
}
```
```solidity
crowdfund/Crowdfund.sol:180:        for (uint256 i = 0; i < contributors.length; ++i) {
crowdfund/Crowdfund.sol:242:        for (uint256 i = 0; i < numContributions; ++i) {
crowdfund/Crowdfund.sol:300:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
crowdfund/Crowdfund.sol:348:            for (uint256 i = 0; i < numContributions; ++i) {
proposals/ListOnOpenseaProposal.sol:291:            for (uint256 i = 0; i < fees.length; ++i) {
proposals/ArbitraryCallsProposal.sol:52:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
proposals/ArbitraryCallsProposal.sol:61:        for (uint256 i = 0; i < calls.length; ++i) {
proposals/ArbitraryCallsProposal.sol:78:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
proposals/LibProposal.sol:14:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
proposals/LibProposal.sol:32:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
distribution/TokenDistributor.sol:230:        for (uint256 i = 0; i < infos.length; ++i) {
distribution/TokenDistributor.sol:239:        for (uint256 i = 0; i < infos.length; ++i) {
party/PartyGovernance.sol:306:        for (uint256 i=0; i < opts.hosts.length; ++i) {
```


## [G-2] Use preincremnt to save gas i++ to ++i;
```solidity
crowdfund/CollectionBuyCrowdfund.sol:62:        for (uint256 i; i < hosts.length; i++) {
```
## [G-3] Cache .length in loops

```solidity
crowdfund/Crowdfund.sol:180:        for (uint256 i = 0; i < contributors.length; ++i) {
crowdfund/Crowdfund.sol:241:        uint256 numContributions = contributions.length;
crowdfund/Crowdfund.sol:300:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
crowdfund/Crowdfund.sol:347:            uint256 numContributions = contributions.length;
crowdfund/Crowdfund.sol:422:            uint256 numContributions = contributions.length;
crowdfund/CollectionBuyCrowdfund.sol:62:        for (uint256 i; i < hosts.length; i++) {
proposals/ListOnOpenseaProposal.sol:291:            for (uint256 i = 0; i < fees.length; ++i) {
proposals/ArbitraryCallsProposal.sol:52:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
proposals/ArbitraryCallsProposal.sol:61:        for (uint256 i = 0; i < calls.length; ++i) {
proposals/ArbitraryCallsProposal.sol:78:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
proposals/LibProposal.sol:14:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
proposals/LibProposal.sol:32:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
distribution/TokenDistributor.sol:230:        for (uint256 i = 0; i < infos.length; ++i) {
distribution/TokenDistributor.sol:239:        for (uint256 i = 0; i < infos.length; ++i) {
party/PartyGovernance.sol:306:        for (uint256 i=0; i < opts.hosts.length; ++i) {
```

## [G-4] Don't use default values Explicit initialization with zero is not required for variable declaration of numTokens because `uints are 0` by default.removeing this will reduce contract size and save a bit of gas.

```solidity
crowdfund/Crowdfund.sol:180:        for (uint256 i = 0; i < contributors.length; ++i) {
crowdfund/Crowdfund.sol:242:        for (uint256 i = 0; i < numContributions; ++i) {
crowdfund/Crowdfund.sol:300:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
crowdfund/Crowdfund.sol:348:            for (uint256 i = 0; i < numContributions; ++i) {
proposals/ListOnOpenseaProposal.sol:291:            for (uint256 i = 0; i < fees.length; ++i) {
proposals/ArbitraryCallsProposal.sol:52:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
proposals/ArbitraryCallsProposal.sol:61:        for (uint256 i = 0; i < calls.length; ++i) {
proposals/ArbitraryCallsProposal.sol:78:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
proposals/LibProposal.sol:14:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
proposals/LibProposal.sol:32:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
distribution/TokenDistributor.sol:230:        for (uint256 i = 0; i < infos.length; ++i) {
distribution/TokenDistributor.sol:239:        for (uint256 i = 0; i < infos.length; ++i) {
party/PartyGovernance.sol:306:        for (uint256 i=0; i < opts.hosts.length; ++i) {
party/PartyGovernance.sol:432:        uint256 low = 0;
```


## [G-5] Check if public is used in same file or not if not then use external insted
```solidity
party/PartyGovernanceNFT.sol:88:    function tokenURI(uint256) public override view returns (string memory) {
```

## [G-06] Use `!= 0` instead of `> 0` in HolyPaladinToken.sol
```solidity
crowdfund/Crowdfund.sol:144:        if (initialBalance > 0) {
crowdfund/Crowdfund.sol:471:        if (votingPower > 0) {
```
