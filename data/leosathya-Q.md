### [L-01] FLOATING PRAGMA USED, PRAGMA SHOULD LOCKED

Contracts should be deployed using the same compiler version/flags with which they have been tested. Locking the pragma (for e.g. by not using ^ in pragma solidity 0.8) ensures that contracts do not accidentally get deployed using any other compiler version with unfixed bugs.

There are 24 instance of this issue:
>**File : crowdfund/AuctionCrowdfund.sol** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L02
>
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/CrowdfundNFTRenderer.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/PartyGovernanceNFTRenderer.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/CrowdfundNFTRenderer.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L02
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L02

##### Mitigation
Should lock Pragma Version



### [L-02] ABSENCE OF ZERO ADDRESS CHECK

There are 22 instance of this issue:

>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L26
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L119
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L35
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/CrowdfundNFTRenderer.sol#L22
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/PartyGovernanceNFTRenderer.sol#L44
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/CrowdfundNFTRenderer.sol#L22
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L94
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L100
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L119
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L121
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L35
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L94
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L71
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L72
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L114-L116
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L267
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L24
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L28

##### Mitigation

Should make zero address check before assigning parameter to state variable



### [L-03] ADD CONSTRUCTOR INITIALIZERS

As per OpenZeppelin’s (OZ) recommendation, “The guidelines are now to make it impossible for anyone to run initialize on an implementation contract, by adding an empty constructor with the initializer modifier. So the implementation contract gets initialized automatically upon deployment.”

**Or**
Should add Initializer modifier to _initialize() function

There are 4 instance of this issue:
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L124
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L41
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L99
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L271


### [L-04] _SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE

>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L480



###[N-01] SHOULD RETURN A NAMED RETURN VALUE

>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/PartyGovernanceNFTRenderer.sol#L90
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/PartyGovernanceNFTRenderer.sol#L110
>**File : ** => https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/PartyGovernanceNFTRenderer.sol#L175

