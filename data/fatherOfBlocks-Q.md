**ReadOnlyDelegateCall.sol**
- L20 - The require should have a message to show when reversing this is important so that the user who receives the failed tx knows the reason why their tx could not be completed.


**ProposalStorage.sol**
- L6 - the IERC721 interface is imported but it is never used.

- L14/15 - A structure with a single element inside is created, this could be omitted and the IProposalExecutionEngine interface is simply called. The goal of structures is to connect multiple elements, in this case there is only one.


**BuyCrowdfund.sol**
- L7 - the LibRawResult interface is imported but is never used.


**Crowdfund.sol**
- L77 - An OnlyContributorAllowedError() error is created that is never used.

- L262/306 - There are two functions with the same name _createParty() but they have different inputs, one of them just converts base type inputs to arrays, this should make it have a different name.


**BuyCrowdfund.sol**
- L7 - the CollectionBuyCrowdfund interface is imported but it is never used.

**CrowdfundNFT.sol**
- L115/120/160/166 - The function _delegateToRenderer() in one of its last lines executes assert(false); therefore it would always revert, just like the other functions that use this function, this could be avoided directly by using the alwaysRevert() modifier.
 

**PartyGovernanceNFT**
- L88/94/101/173 - The function _delegateToRenderer() in one of its last lines executes assert(false); therefore it would always revert, just like the other functions that use this function, this could be avoided directly by using the alwaysRevert() modifier.


**ProposalExecutionEngine**
- L13 - the LibProposal library is imported but it is never used.

- L210-222 - A switch could be used instead of concatenating if/else.

- L246/247 - The requirements should have a defined message so that when they revert the message received by the user is consistent with the error that occurred.


**TokenDistributor**
- L44 - An OnlyPartyDaoAuthorityError error is created that is never used.

- L225/236 - When the for is iterated, the length of an array is used, but two arrays are used inside. This could bring errors since a length can be longer than another, generating exceptions without knowing a reason why, to improve the user experience, it should be previously validated and throw an exception with a correct message.


**PartyGoverance.sol**
- L173/175 - Errors are created: ProposalExistsError, ProposalHasNoVotesError that are never used.

- L795 - A low gas level transfer is performed that does not revert when it returns false. This should be a standard, although in this case it does not generate major inconveniences since it simply returns false, but there is no type of status modification within the contract.

- L1062/1079 - It should be validated that totalVotingPower is != 0 and return a corresponding message, otherwise it would revert showing an unreadable message.


**proposals/vendor/FractionalV1.sol**
- L9/28 - The contract is called FractionalV1 and inside it has interfaces, its name is somewhat confusing, since by convention the word I adenate is used, seeking to represent the word interface.
