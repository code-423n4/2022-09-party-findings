## DEFAULT VALUE INITIALIZATION

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