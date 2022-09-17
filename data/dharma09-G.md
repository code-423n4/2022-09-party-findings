## 1.DEFAULT VALUE INITIALIZATION

### PROBLEM
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

### PROOF OF CONCEPT
Instances include:

### ArbitraryCallsProposal.sol
```
ArbitraryCallsProposal.sol#L52
ArbitraryCallsProposal.sol#L61
ArbitraryCallsProposal.sol#L78
```
### TokenDistributor.sol
```
TokenDistributor.sol#L230
TokenDistributor.sol#L239
```
### ListOnOpenseaProposal.sol
```
ListOnOpenseaProposal.sol#L291
```
### Crowdfund.sol
```
Crowdfund.sol#L180
Crowdfund.sol#L242
Crowdfund.sol#L300
Crowdfund.sol#L348
```
### PartyGovernance.sol
```
PartyGovernance.sol#L306
```

## 2.OR CONDITIONS COST LESS THAN THEIR EQUIVALENT, AND CONDITIONS (“NOT(SOMETHING IS FALSE)” COSTS LESS THAN “EVERYTHING IS TRUE”)
the equivalent of `(a && b)` is ` !(!a || !b)`

Even with the 10k Optimizer enabled, `OR` conditions cost less than their equivalent `AND` conditions.

### Proof of Concept.
Compare in Remix this example contract’s 2 diffs (or any test contract of your choice, as experimentation always shows the same results).
```
pragma solidity 0.8.13;
contract Test {
   bool isOpen;
   bool channelPreviouslyOpen;

   function boolTest() external view returns (uint) {
-       if (isOpen && !channelPreviouslyOpen) {
+       if (!(!isOpen || channelPreviouslyOpen)) {
           return 1;
-       } else if (!isOpen && channelPreviouslyOpen) {
+       } else if (!(isOpen || !channelPreviouslyOpen)) {
           return 2;
       }
   }
   function setBools(bool _isOpen, bool _channelPreviouslyOpen) external {
       isOpen = _isOpen;
       channelPreviouslyOpen= _channelPreviouslyOpen;
   }
}
```
effectively saving 12 gas.

### Affected Code

[ListOnZoraProposal.sol#L96](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L96)
[ListOnZoraProposal.sol#L98](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L98)
[ListOnZoraProposal.sol#L103](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L103)
[ListOnOpenseaProposal.sol#L204](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L204)
[ListOnOpenseaProposal.sol#L206](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L206)

Use `!(minDuration == 0 || !data.duration < minDuration)` instead of `(minDuration != 0 && data.duration < minDuration)`

[AuctionCrowdfund.sol#L203](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L203)

Use `!(lc == CrowdfundLifecycle.Active || lc == CrowdfundLifecycle.Expired)` instead of `(lc != CrowdfundLifecycle.Active && lc != CrowdfundLifecycle.Expired)`

[BuyCrowdfundBase.sol#L117](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L117)

Use `!(maximumPrice_ == 0 || !callValue > maximumPrice_)` instead of  `(maximumPrice_ != 0 && callValue > maximumPrice_)` 

[ListOnOpenseaProposal.sol#L140](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L140)

Use `!(isUnanimous || !LibProposal.isTokenIdPrecious(data.token,data.tokenId,params.preciousTokens,params.preciousTokenIds)` instead of `(!isUnanimous &&LibProposal.isTokenIdPrecious(data.token,data.tokenId,params.preciousTokens,params.preciousTokenIds)`

[PartyGovernance.sol#L230](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L230)

Use `!(snap.intrinsicVotingPower != 0 || snap.delegatedVotingPower != 0)` instead of `(snap.intrinsicVotingPower == 0 && snap.delegatedVotingPower == 0)`

[PartyGovernance.sol#L251](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L251)

Use `!(msg.sender == partyDao || isHost[msg.sender])` instead of `(msg.sender != partyDao && !isHost[msg.sender])`

[]()

All instances:
### PartyGovernance.sol
[PartyGovernance.sol#L577](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L577)
[PartyGovernance.sol#L600](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L600)
[PartyGovernance.sol#L624](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L624)
[PartyGovernance.sol#L674](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L674)
[PartyGovernance.sol#L856](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L856)
[PartyGovernance.sol#L934](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L934)
[LibProposal.sol#L33](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L33)