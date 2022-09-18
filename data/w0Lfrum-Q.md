# QA Report: Party DAO 

## 1. Conflicting Imports

### Lines of Code

IPartyFactory.sol file : 

[Lines 4-5](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/IPartyFactory.sol#L4-L5) 



Party.sol file : 

[Line 4](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/Party.sol#L4)

PartyGovernanceNFT.sol file : 

[Lines 5,7,8](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L4-L7)

PartyGovernance.sol file : 

[Line 21](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L21)

Crowdfund.sol file : 

[Line 11](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L11)


### Details


Conflicting file imports are present in the files IPartyFactory.sol, Party.sol, PartyGovernanceNFT.sol and PartyGovernance.sol; which cause declaration errors. 
The IGlobals.sol, IERC721.sol and the LibSafeCast.sol files are unnecessarily imported repeatedly in the above mentioned solidity files from the "party" folder.
There is also a conflicting import statement at line 21 of the PartyGovernance.sol file. The import of the IPartyFactory.sol in line 21 is not required. This is because the IPartyFactory.sol already indirectly imports the PartyGovernance.sol file (through the Party.sol -> PartyGovernanceNFT.sol -> PartyGovernance.sol). 
On removing the IPartyFactory.sol import in line 21 of the PartyGovernance.sol file, there would be an "Identifier not found" error caused in the Crowdfund.sol file. This may be solved by separately importing the IPartyFactory.sol file in the Crowdfund.sol file.

### Fixes

IPartyFactory.sol file : The file imports at line 4 and 5 may be removed.

Party.sol file : The file import at line 4 may be removed.

PartyGovernanceNFT.sol file : The file imports at line 5,7 and 8 may be removed.

PartyGovernance.sol file :  The file import at line 21 may be removed.

Crowdfund.sol file : A separate file import for the IPartyFactory.sol may be added (import "../party/IPartyFactory.sol";)

## 2. Unnecessary Imports 

### Lines of Code

CrowdFund.sol file : 

[Lines 5,6,7,9](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L5-L7)

AuctionCrowdfund.sol file : 

[Lines 4,5](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L4-L5)
[Lines 9,10](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L9-L10)

BuyCrowdfundBase.sol file :

[Lines 4-6](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfundBase.sol#L4-L6)
[Lines 8-10](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfundBase.sol#L8-L10)

BuyCrowdfund.sol file :

[Lines 4-9](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfund.sol#L4-L9)

CollectionBuyCrowdfund.sol file : 

[Lines 4-9](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L4-L9)

ArbitraryCallsProposal.sol file :
[Line 4](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L4)

ListOnZoraProposal.sol file :
[Line 6](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L6)

ListOnOpenseaProposal.sol file :
[Line 6](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L6)

FractionalizeProposal.sol file :
[Line 4](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/FractionalizeProposal.sol#L4)

ProposalExecutionEngine.sol file :
[Line 5,6,8](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L5-L8)
[Line 13](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L13)

TokenDistributor.sol file :
[Line 6](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L6)

### Details 

Redundant .sol file imports are present in the Crowdfund, AuctionCrowdfund, BuyCrowdfundBase, BuyCrowdfund, CollectionBuyCrowdfund files. These file imports are already imported from other imported files in the contract.

Lines 5,6,7,9 of Crowdfund.sol : imported from Party.sol and CrowdfundNFT.sol

Lines 4,5,9,10 of AuctionCrowdfund.sol : imported from Crowdfund.sol

Lines 4,5,6,8,9,10 of BuyCrowdfundBase.sol :  imported from Crowdfund.sol

Lines 4-9 of BuyCrowdfund.sol : imported from BuyCrowdfundBase.sol

Lines 4-9 of CollectionBuyCrowdfund.sol : imported from BuyCrowdfundBase.sol

Line 4 of ArbitraryCallsProposal.sol : imported from LibProposal.sol

Line 6 of ListOnZoraProposal.sol : imported from ZoraHelpers.sol

Line 6 of ListOnOpenseaProposal.sol : imported from ZoraHelpers.sol

Line 6 of FractionalizeProposal.sol : imported from IProposalExecutionEngine.sol

Lines 5,6,8,13 of ProposalExecutionEngine.sol : imported from ArbitraryCallsProposal, ListOnZoraProposal, ListOnOpenseaProposal, FractionalizeProposal

Line 6 of TokenDistributor.sol : imported from ITokenDistributor.sol

### Fix

The lines of code for these file imports may be removed.