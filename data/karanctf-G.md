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


## [G-5]  If public is not used in same file  then use external instead
```solidity
party/PartyGovernanceNFT.sol:88:    function tokenURI(uint256) public override view returns (string memory) {
```

## [G-06] Use `!= 0` instead of `> 0` in HolyPaladinToken.sol
```solidity
crowdfund/Crowdfund.sol:144:        if (initialBalance > 0) {
crowdfund/Crowdfund.sol:471:        if (votingPower > 0) {
```
## [G-7] Declare `uint256 mid `outside of while loop
```solidity
party/PartyGovernance.sol-433-        while (low < high) {
party/PartyGovernance.sol:434:            uint256 mid = (low + high) / 2;

```
## [G-8] <x> += <y>` costs more gas than `<x> = <x> + <y>`
```solidity
crowdfund/Crowdfund.sol:243:            ethContributed += contributions[i].amount;
crowdfund/Crowdfund.sol:352:                    ethOwed += c.amount;
crowdfund/Crowdfund.sol:355:                    ethUsed += c.amount;
crowdfund/Crowdfund.sol:359:                    ethUsed += partialEthUsed;
crowdfund/Crowdfund.sol:374:            votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up
crowdfund/Crowdfund.sol:411:            totalContributions += amount;
crowdfund/Crowdfund.sol:427:                    lastContribution.amount += amount;
proposals/ArbitraryCallsProposal.sol:72:            ethAvailable -= calls[i].value;
distribution/TokenDistributor.sol:381:        _storedBalances[balanceId] -= amount;
party/PartyGovernance.sol:595:        values.votes += votingPower;
party/PartyGovernance.sol:959:                newDelegateDelegatedVotingPower -= oldSnap.intrinsicVotingPower;

```