# QA Report for Party DAO contest

## Overview
During the audit, 3 low and 3 non-critical issues were found.

№ | Title | Risk Rating  | Instance Count
--- | --- | --- | ---
L-1 | [Storage variables can be packed tightly](#l-1-storage-variables-can-be-packed-tightly) | Low | 6
L-2 | [Check zero denominator](#l-2-check-zero-denominator) | Low | 2
L-3 | [Large number of elements may cause out-of-gas error](#l-3-large-number-of-elements-may-cause-out-of-gas-error) | Low | 6
NC-1 | [Order of Functions](#nc-1-order-of-functions) | Non-Critical | 5
NC-2 | [Floating pragma](#nc-2-floating-pragma) | Non-Critical | 46
NC-3 | [Missing NatSpec](#nc-3-missing-natspec) | Non-Critical | 74

## Low Risk Findings (3)
### L-1. Storage variables can be packed tightly
##### Description
According to [docs](https://docs.soliditylang.org/en/v0.8.16/internals/layout_in_storage.html#layout-of-state-variables-in-storage), multiple, contiguous items that need less than 32 bytes are packed into a single storage slot if possible. It might be beneficial to use reduced-size types if you are dealing with storage values because the compiler will pack multiple elements into one storage slot, and thus, combine multiple reads or writes into a single operation. 

##### Instances 
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L27-L36
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L30-L39
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L31-L40
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L48-L56
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L27-L36
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L93-L104

##### Recommendation
Consider changing order of variables to, for example:
```
uint40 duration;
uint96 maximumPrice;
uint16 splitBps;
address payable splitRecipient;
```

#
### L-2. Check zero denominator
##### Description
If the input parameter ```totalVotingPower``` is equal to zero, this will cause the function call failure on division.

##### Instances
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1062
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1078-L1079

##### Recommendation
Add the check to prevent function call failure.

#
### L-3. Large number of elements may cause out-of-gas error
##### Description
[Loops](https://docs.soliditylang.org/en/develop/security-considerations.html#gas-limit-and-loops) that do not have a fixed number of iterations, for example, loops that depend on storage values, have to be used carefully: Due to the block gas limit, transactions can only consume a certain amount of gas. Either explicitly or just due to normal operation, the number of iterations in a loop can grow beyond the block gas limit, which can cause the complete contract to be stalled at a certain point. 

##### Instances
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L60-L67
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230-L231
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239-L240
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291-L298
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300-L301
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L306-L307

##### Recommendation
Restrict the maximum number of elements.

## Non-Critical Risk Findings (3)
### NC-1. Order of Functions
##### Description
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-functions), ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier.  
Functions should be grouped according to their visibility and ordered:
1) constructor
2) receive function (if exists)
3) fallback function (if exists)
4) external
5) public
6) internal
7) private

##### Instances
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

##### Recommendation
Reorder functions where possible.

#
### NC-2. Floating pragma
##### Description
Contracts should be deployed with the same compiler version. It helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

##### Instances
All contracts.

##### Recommendation
According to [SWC-103](https://swcregistry.io/docs/SWC-103), pragma version should be locked.

#
### NC-3. Missing NatSpec
##### Description
No NatSpec or any comments for 74 functions in 17 contracts.

##### Instances
- 4 functions in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol
- 14 functions in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol
- 1 function in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L107
- 4 functions in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol
- 1 function in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L173
- 1 function in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L283
- 4 functions in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol
- 4 functions in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol
- 1 function in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L166
- 3 functions in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol
- 5 functions in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol
- 10 functions in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol
- 2 functions in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaConduitController.sol
- 1 function in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721Receiver.sol
- 6 functions in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/FractionalV1.sol
- 6 functions in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC1155.sol
- 7 functions in https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaExchange.sol

##### Recommendation
Add NatSpec for all functions.