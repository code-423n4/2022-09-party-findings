# G001 - Don't Initialize Variables with Default Value

### Description

Uninitialized variables are assigned with the types default value.

Explicitly initializing a variable with it's default value costs unnecessary gas.

```solidity
for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52)

```solidity
for (uint256 i = 0; i < calls.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61)

```solidity
for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L78](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L78)

```solidity
for (uint256 i = 0; i < contributors.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L180](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L180)

```solidity
for (uint256 i = 0; i < numContributions; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L242](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L242)

```solidity
for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L300](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L300)

```solidity
for (uint256 i = 0; i < numContributions; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L348](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L348)

```solidity
for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14)

```solidity
for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L32](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L32)

```solidity
for (uint256 i = 0; i < fees.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291)

```solidity
for (uint256 i=0; i < opts.hosts.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306)

```solidity
for (uint256 i = 0; i < infos.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L230](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L230)

```solidity
for (uint256 i = 0; i < infos.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L239](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L239)


# ****G002 - Cache Array Length Outside of Loop****

### **Description**

The overheads outlined below are *PER LOOP*, excluding the first loop

- storage arrays incur a Gwarmaccess (**100 gas**)
- memory arrays use `MLOAD` (**3 gas**)
- calldata arrays use `CALLDATALOAD` (**3 gas**)

Caching the length changes each of these to a `DUP<N>` (**3 gas**), and gets rid of the extra `DUP<N>` needed to store the stack offset

```solidity
for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52)

```solidity
for (uint256 i = 0; i < calls.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61)

```solidity
for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L78](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L78)

```solidity
for (uint256 i = 0; i < contributors.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L180](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L180)

```solidity
for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L300](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L300)

```solidity
for (uint256 i = 0; i < numContributions; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L348](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L348)

```solidity
for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14)

```solidity
for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L32](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L32)

```solidity
for (uint256 i = 0; i < fees.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291)

```solidity
for (uint256 i=0; i < opts.hosts.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306)

```solidity
for (uint256 i = 0; i < infos.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L230](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L230)

```solidity
for (uint256 i = 0; i < infos.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L239](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L239)

Background info

[https://github.com/code-423n4/2021-11-badgerzaps-findings/issues/36](https://github.com/code-423n4/2021-11-badgerzaps-findings/issues/36)

# G003 `++I`/`I++` SHOULD BE `UNCHECKED{++I}`/`UNCHECKED{I++}` WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN `FOR`AND `WHILE`LOOPS

The `unchecked` keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves **30-40 gas [per loop](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)**

```solidity
for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52)

```solidity
for (uint256 i = 0; i < calls.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61)

```solidity
for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L78](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L78)

```solidity
for (uint256 i = 0; i < contributors.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L180](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L180)

```solidity
for (uint256 i = 0; i < numContributions; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L242](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L242)

```solidity
for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L300](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L300)

```solidity
for (uint256 i = 0; i < numContributions; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L348](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L348)

```solidity
for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14)

```solidity
for (uint256 i = 0; i < preciousTokens.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L32](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L32)

```solidity
for (uint256 i = 0; i < fees.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291)

```solidity
for (uint256 i=0; i < opts.hosts.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306)

```solidity
for (uint256 i = 0; i < infos.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L230](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L230)

```solidity
for (uint256 i = 0; i < infos.length; ++i) {
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L239](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L239)