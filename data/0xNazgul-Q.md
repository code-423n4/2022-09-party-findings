## [NAZ-L1] Lack of Event Emission For Critical Functions
**Severity**: Low
**Context**: [`Globals.sol#L27`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L27), [`Globals.sol#L59`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L59), [`Globals.sol#L63`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L63), [`Globals.sol#L67`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L67), [`Globals.sol#L71`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L71), [`Globals.sol#L75`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L75), [`Globals.sol#L79`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L79), [`PartyGovernanceNFT.sol#L169`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L169)

**Description**:
Several functions update critical parameters that are missing event emission. These should be performed to ensure tracking of changes of such critical parameters.

**Recommendation**:
Consider adding events to functions that change critical parameters.


## [NAZ-L2] Unsafe Type Casting
**Severity** Low
**Context**: [`Globals.sol#L44`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L44)

**Description**:
Downcasting from uint256/int256 in Solidity does not revert on overflow. This can easily result in undesired exploitation or bugs.

**Recommendation**:
Consider using SafeCast library from [OpenZeppelin](https://docs.openzeppelin.com/contracts/4.x/api/utils#SafeCast).


## [NAZ-L3] Missing Time locks
**Severity**: Low 
**Context**: [`Globals.sol#L27`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L27), [`PartyGovernanceNFT.sol#L169`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L169)

**Description**:
When critical parameters of systems need to be changed, it is required to broadcast the change via event emission and recommended to enforce the changes after a time-delay. This is to allow system users to be aware of such critical changes and give them an opportunity to exit or adjust their engagement with the system accordingly. None of the onlyOwner functions that change critical protocol addresses/parameters have a timelock for a time-delayed change to alert: (1) users and give them a chance to engage/exit protocol if they are not agreeable to the changes (2) team in case of compromised owner(s) and give them a chance to perform incident response.

**Recommendation**:
Users may be surprised when critical parameters are changed. Furthermore, it can erode users' trust since they can’t be sure the protocol rules won’t be changed later on. Compromised owner keys may be used to change protocol addresses/parameters to benefit attackers. Without a time-delay, authorized owners have no time for any planned incident response.


## [NAZ-L4] Missing Equivalence Checks in Setters
**Severity**: Low
**Context**: [`Globals.sol#L27`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L27), [`Globals.sol#L59`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L59), [`Globals.sol#L63`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L63), [`Globals.sol#L71`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L71), [`Globals.sol#L75`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L75), [`Globals.sol#L79`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L79)

**Description**:
Setter functions are missing checks to validate if the new value being set is the same as the current value already set in the contract. Such checks will showcase mismatches between on-chain and off-chain states.

**Recommendation**:
This may hinder detecting discrepancies between on-chain and off-chain states leading to flawed assumptions of on-chain state and protocol behavior.


## [NAZ-L5] Lack of Two-Step Process for Critical Operations
**Severity** Low
**Context**: [`Globals.sol#L27`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L27), [`PartyGovernanceNFT.sol#L169`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L169)

**Description**:
This function transfers the ownership of the contract in a single step. There is no way to reverse a one-step transfer of ownership to an address without an owner. This would not be the case if ownership were transferred through a two-step process in which an owner proposed a transfer and the prospective recipient accepted it.

**Recommendation**:
Consider using a two-step process for ownership transfers. 


## [NAZ-L6] Missing Zero-address Validation
**Severity**: Low
**Context**: [`Proxy.sol#L17`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol#L17), [`Proxy.sol#L26`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol#L26), [`PartyFactory.sol#L22`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L22), [`FractionalizeProposal.sol#L35`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L35), [`CrowdfundFactory.sol#L25`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L25), [`CrowdfundNFT.sol#L34`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L34), [`PartyGovernanceNFT.sol#L44`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L44), [`PartyGovernanceNFT.sol#L62`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L62),[`ListOnZoraProposal.sol#L70`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L70), [`BuyCrowdfundBase.sol#L78`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L78), [`BuyCrowdfundBase.sol#L80-L82`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L80-L82), [`BuyCrowdfund.sol#L87`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L87), [`CollectionBuyCrowdfund.sol#L101`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L101), [`ProposalExecutionEngine.sol#L83`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L83), [`TokenDistributor.sol#L93`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L93), [`Globals.sol#L23`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L23), [`Globals.sol#L27`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L27), [`Globals.sol#L67`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L67), [`Globals.sol#L79`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L79) 

**Description**:
Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

**Recommendation**:
Consider adding explicit zero-address validation on input parameters of address type.


## [NAZ-L7] Value Range Validity
**Severity** Low
**Context**: [`BuyCrowdfundBase.sol#L73-L74`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L73-L74), [`BuyCrowdfundBase.sol#L79`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L79)

**Description**:
These functions doesn't have any checks to ensure that the variables being set is within some kind of value range.

**Recommendation**:
Each variable input parameter updated should have it's own value range checks to ensure their validity.


## [NAZ-L8] Assert Violation That Always Incorrect Control Flow Implementation
**Severity**: Low
**Context**: [`ReadOnlyDelegateCall.sol#L30`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L30), [`FractionalizeProposal.sol#L67`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L67), [`CrowdfundNFT.sol#L166`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L166), [`PartyGovernanceNFT.sol#L179`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L179), [`ListOnZoraProposal.sol#L120`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L120), [`ProposalExecutionEngine.sol#L142`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L142), [`TokenDistributor.sol#L385`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L385), [`TokenDistributor.sol#L411`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L411), [`ListOnOpenseaProposal.sol#L221`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L221), [`ListOnOpenseaProposal.sol#L302`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L302), [`PartyGovernance.sol#L504`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L504)

**Description**:
The Solidity assert() function is meant to assert invariants. Properly functioning code should never reach a failing assert statement. A reachable assertion can mean one of two things:

1). A bug exists in the contract that allows it to enter an invalid state;
2). The assert statement is used incorrectly, e.g. to validate inputs.

**Recommendation**:
Consider whether the condition checked in the assert() is actually an invariant. If not, replace the assert() statement with a require() statement. If the exception is indeed caused by unexpected behaviour of the code, fix the underlying bug(s) that allow the assertion to be violated.


## [NAZ-L9] `receive()` Function Should Emit An Event
**Severity**: Low
**Context**: [`Party.sol#L47`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L47), [`AuctionCrowdfund.sol#L144`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L144)

**Description**:
Consider emitting an event inside this function with `msg.sender` and `msg.value` as the parameters. This would make it easier to track incoming ether transfers.

**Recommendation**:
Consider adding events to the `receive()` functions. 


## [NAZ-L10] Missing Events In Initialize Functions
**Severity**: Low
**Context**: [`BuyCrowdfundBase.sol#L70`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L70), [`CrowdfundNFT.sol#L39`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L39), [`AuctionCrowdfund.sol#L110`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L110), [`ProposalExecutionEngine.sol#L99`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L99), [`ProposalStorage.sol#L33`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L33), [`BuyCrowdfund.sol#L68`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L68), [`CollectionBuyCrowdfund.sol#L83`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L83)

**Description**:
None of the initialize functions emit emit init-specific events. They all however have the initializer modifier (from Initializable) so that they can be called only once. Off-chain monitoring of calls to these critical functions is not possible.

**Recommendation**:
It is recommended to emit events in your initialization functions.


## [NAZ-N1] Function States Named Return But Doesn't Use It
**Severity** Informational
**Context**: [`ProposalStorage.sol#L24`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L24), [`AuctionCrowdfund.sol#L287`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L287), [`ProposalExecutionEngine.sol#L110`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L110), [`PartyGovernance.sol#L385`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L385)

**Description**:
The linked function(s) have a named return that is not used. Using both named returns and a return statement isn't necessary. 

**Recommendation**:
Removing one of those can improve code clarity.


## [NAZ-N2] Array length mismatch
**Severity**: Informational
**Context**: [`PartyFactory.sol#L29-L30`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L29-L30), [`TokenDistributor.sol#L225`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L225), [`TokenDistributor.sol#L236`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L236)

**Description**:
These fail to perform input validation on arrays to verify the lengths match. A mismatch could lead to an exception or undefined behavior.

**Recommendation**:
Perform input validation on the arrays to verify that the lengths match.


## [NAZ-N3] Visibility Should Be Before Modifiers
**Severity**: Informational
**Context**: [`TokenDistributor.sol#L307`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L307), [`TokenDistributor.sol#L321`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L321), [`TokenDistributor.sol#L327`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L327)

**Description**:
Visibility keyword should be before the list of modifiers as per best practice.

**Recommendation**:
Consider moving the visibility keyword before the list of modifiers.


## [NAZ-N4] Line Length
**Severity**: Informational
**Context**: [`CrowdfundFactory.sol#L18`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L18), [`ProposalExecutionEngine.sol#L73`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L73), [`Crowdfund.sol#L78`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L78)

**Description**:
Max line length must be no more than 120 but many lines are extended past this length.

**Recommendation**:
Consider cutting down the line length below 120.


## [NAZ-N5] Code Contains Empty Blocks
**Severity**: Informational
**Context**: [`Party.sol#L28`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L28), [`Party.sol#L47`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L47), [`BuyCrowdfund.sol#L62`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L62), [`CollectionBuyCrowdfund.sol#L77`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L77), [`CrowdfundNFT.sol#L52`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L52), [`CrowdfundNFT.sol#L59`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L59), [`CrowdfundNFT.sol#L66`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L66), [`CrowdfundNFT.sol#L73`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L73), [`CrowdfundNFT.sol#L80`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L80), [`AuctionCrowdfund.sol#L104`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L104), [`AuctionCrowdfund.sol#L144`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L144), [`BuyCrowdfundBase.sol#L67`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L67)

**Description**:
It's best practice that when there is an empty block, to add a comment in the block explaining why it's empty.

**Recommendation**:
Consider adding `/* Comment on why */` to the empty blocks.


## [NAZ-N6] Function && Variable Naming Convention
**Severity** Informational
**Context**: [`Proxy.sol#L12`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol#L12), [`PartyFactory.sol#L18`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L18), [`ProposalStorage.sol#L18-L19`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L18-L19), [`FractionalizeProposal.sol#L31`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L31), [`CrowdfundFactory.sol#L22`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L22), [`CrowdfundNFT.sol#L17`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L17), [`PartyGovernanceNFT.sol#L24`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L24), [`ListOnZoraProposal.sol#L58`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L58), [`ListOnZoraProposal.sol#L61`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L61), [`ListOnZoraProposal.sol#L64`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L64), [`ListOnZoraProposal.sol#L67`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L67), [`ProposalExecutionEngine.sol#L77`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L77), [`TokenDistributor.sol#L55`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L55), [`TokenDistributor.sol#L59`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L59), [`Implementation.sol#L9`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Implementation.sol#L9), [`ListOnOpenseaProposal.sol#L100`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L100), [`ListOnOpenseaProposal.sol#L102`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L102), [`ListOnOpenseaProposal.sol#L105`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L105), [`Crowdfund.sol#L87`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L87), [`PartyGovernance.sol#L189-L190`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L189-L190), [`PartyGovernance.sol#L194`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L194), [`LibProposal.sol#L7`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L7)

**Description**:
The linked variables do not conform to the standard naming convention of Solidity whereby functions and variable names(local and state) utilize the `mixedCase` format unless variables are declared as `constant` in which case they utilize the `UPPER_CASE_WITH_UNDERSCORES` format. Private variables and functions should lead with an `_underscore`.

**Recommendation**:
Consider naming conventions utilized by the linked statements are adjusted to reflect the correct type of declaration according to the [Solidity style guide](https://docs.soliditylang.org/en/latest/style-guide.html). 


## [NAZ-N7] Code Structure Deviates From Best-Practice
**Severity**: Informational
**Context**: [`ProposalStorage.sol#L29`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L29), [`FractionalizeProposal.sol#L31`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L31), [`CrowdfundFactory.sol#L22`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L22), [`AuctionCrowdfund.sol#L144`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L144)

**Description**:
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier.  Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Functions should then further be ordered with view functions coming after the non-view labeled ones.

**Recommendation**:
Consider adopting recommended best-practice for code structure and layout.


## [NAZ-N8] Unindexed Event Parameters
**Severity** Informational
**Context**: [`CrowdfundFactory.sol#L16-L18`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L16-L18), [`ListOnZoraProposal.sol#L53`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L53), [`ArbitraryCallsProposal.sol#L35`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L35), [`BuyCrowdfundBase.sol#L52`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L52), [`ListOnOpenseaProposal.sol#L63-L82`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L63-L82), [`Crowdfund.sol#L82-L83`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L82-L83), [`PartyGovernance.sol#L151-L162`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L151-L162)

**Description**:
Parameters of certain events are expected to be indexed so that they’re included in the block’s bloom filter for faster access. Failure to do so might confuse off-chain tooling looking for such indexed events.

**Recommendation**:
Consider adding the indexed keyword to event parameters that should include it.


## [NAZ-N9] Unclear Revert Messages
**Severity** Informational
**Context**: [`ReadOnlyDelegateCall.sol#L20`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L20), [`ProposalExecutionEngine.sol#L246-L247`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L246-L247)

**Description**:
Some revert messages are unclear which can lead to confusion. Unclear revert messages may cause misunderstandings on reverted transactions.

**Recommendation**: 
Consider making revert messages more clear.


## [NAZ-N10] Spelling Errors
**Severity**: Informational
**Context**: [`BuyCrowdfundBase.sol#L31 (receieves => receives)`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L31), [`Crowdfund.sol#L46 (recipeint => recipient)`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L46)

**Description**:
Spelling errors in comments can cause confusion to both users and developers.

**Recommendation**:
Consider checking all misspellings to ensure they are corrected.


## [NAZ-N11] Missing or Incomplete NatSpec
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts)

**Description**:
Some functions are missing @notice/@dev NatSpec comments for the function, @param for all/some of their parameters and @return for return values. Given that NatSpec is an important part of code documentation, this affects code comprehension, auditability and usability.

**Recommendation**:
Consider adding in full NatSpec comments for all functions to have complete code documentation for future use.


## [NAZ-N12] Floating Pragma
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts)

**Description**:
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

**Recommendation**: 
Consider locking the pragma version.


## [NAZ-N13] Older Version Pragma
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts)

**Description**:
Using very old versions of Solidity prevents benefits of bug fixes and newer security checks. Using the latest versions might make contracts susceptible to undiscovered compiler bugs. 

**Recommendation**:
Consider using the most recent version.