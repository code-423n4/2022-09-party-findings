# [G-01] Prefix increment costs less gas than postfix increment

There is 1 instance of this issue.

```
File: contracts/crowdfund/CollectionBuyCrowdfund.sol
62: for (uint256 i; i < hosts.length; i++) {
```

# [G-02] The increment in for loop post condition can be made unchecked to save gas

There are 14 instances of this issue.

```
File: contracts/crowdfund/CollectionBuyCrowdfund.sol
62: for (uint256 i; i < hosts.length; i++) {
```

```
File: contracts/crowdfund/Crowdfund.sol
180: for (uint256 i = 0; i < contributors.length; ++i) {
242: for (uint256 i = 0; i < numContributions; ++i) {
300: for (uint256 i = 0; i < preciousTokens.length; ++i) {
348: for (uint256 i = 0; i < numContributions; ++i) {
```

```
File: contracts/distribution/TokenDistributor.sol
230: for (uint256 i = 0; i < infos.length; ++i) {
239: for (uint256 i = 0; i < infos.length; ++i) {
```

```
File: contracts/party/PartyGovernance.sol
306: for (uint256 i=0; i < opts.hosts.length; ++i) {
```

```
File: contracts/proposals/ArbitraryCallsProposal.sol
52: for (uint256 i = 0; i < hadPreciouses.length; ++i) {
61: for (uint256 i = 0; i < calls.length; ++i) {
78: for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```

```
File: contracts/proposals/LibProposal.sol
14: for (uint256 i = 0; i < preciousTokens.length; ++i) {
32: for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

```
File: contracts/proposals/ListOnOpenseaProposal.sol
291: for (uint256 i = 0; i < fees.length; ++i) {
```

# [G-03] Cache the length of the array before the loop

There are 12 instances of this issue.

```
File: contracts/crowdfund/CollectionBuyCrowdfund.sol
62: for (uint256 i; i < hosts.length; i++) {
```

```
File: contracts/crowdfund/Crowdfund.sol
180: for (uint256 i = 0; i < contributors.length; ++i) {
300: for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

```
File: contracts/distribution/TokenDistributor.sol
230: for (uint256 i = 0; i < infos.length; ++i) {
239: for (uint256 i = 0; i < infos.length; ++i) {
```

```
File: contracts/party/PartyGovernance.sol
306: for (uint256 i=0; i < opts.hosts.length; ++i) {
```

```
File: contracts/proposals/ArbitraryCallsProposal.sol
52: for (uint256 i = 0; i < hadPreciouses.length; ++i) {
61: for (uint256 i = 0; i < calls.length; ++i) {
78: for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```

```
File: contracts/proposals/LibProposal.sol
14: for (uint256 i = 0; i < preciousTokens.length; ++i) {
32: for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

```
File: contracts/proposals/ListOnOpenseaProposal.sol
291: for (uint256 i = 0; i < fees.length; ++i) {
```

# [G-04] Initializing a variable with the default value wastes gas

There are 14 instances of this issue.

```
File: contracts/crowdfund/Crowdfund.sol
180: for (uint256 i = 0; i < contributors.length; ++i) {
242: for (uint256 i = 0; i < numContributions; ++i) {
300: for (uint256 i = 0; i < preciousTokens.length; ++i) {
348: for (uint256 i = 0; i < numContributions; ++i) {
```

```
File: contracts/distribution/TokenDistributor.sol
230: for (uint256 i = 0; i < infos.length; ++i) {
239: for (uint256 i = 0; i < infos.length; ++i) {
```

```
File: contracts/party/PartyGovernance.sol
306: for (uint256 i=0; i < opts.hosts.length; ++i) {
432: uint256 low = 0;
```

```
File: contracts/proposals/ArbitraryCallsProposal.sol
52: for (uint256 i = 0; i < hadPreciouses.length; ++i) {
61: for (uint256 i = 0; i < calls.length; ++i) {
78: for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```

```
File: contracts/proposals/LibProposal.sol
14: for (uint256 i = 0; i < preciousTokens.length; ++i) {
32: for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

```
File: contracts/proposals/ListOnOpenseaProposal.sol
291: for (uint256 i = 0; i < fees.length; ++i) {
```

# [G-05] Use != 0 instead of > 0 to save gas.

Replace `> 0` with `!= 0` for unsigned integers.

There are 2 instances of this issue.

```
File: contracts/crowdfund/Crowdfund.sol
144: if (initialBalance > 0) {
471: if (votingPower > 0) {
```

# [G-06] Use custom errors rather than revert strings to save gas

There is 1 instance of this issue.

```
File: contracts/crowdfund/CrowdfundNFT.sol
29: revert('ALWAYS FAILING');
```

# [G-07] Use right/left shift instead of division/multiplication to save gas

There is 1 instance of this issue.

```
File: contracts/party/PartyGovernance.sol
434: uint256 mid = (low + high) / 2;
```

# [G-08] x += y costs more gas than x = x + y for state variables

There are 2 instances of this issue.

```
File: contracts/crowdfund/Crowdfund.sol
411: totalContributions += amount;
```

```
File: contracts/distribution/TokenDistributor.sol
381: _storedBalances[balanceId] -= amount;
```
