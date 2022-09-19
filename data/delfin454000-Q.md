### Typos
___

[BuyCrowdfundBase.sol: L31](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfundBase.sol#L31)
```solidity
        // An address that receieves an extra share of the final voting power
```
Change `receieves` to `receives`
___
[ListOnOpenseaProposal.sol: L303](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L303)
```solidity
        // Emit the the coordinated OS event so their backend can detect this order.
```
Remove repeated word `the`
___
The same typo (`recipeint`) occurs in both lines below:

[Crowdfund.sol: L46](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L46)

[PartyGovernance.sol: L86](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L86)

*Example ([Crowdfund.sol: L46](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L46)):*

```solidity
        // Fee recipeint for governance distributions.
```
Change `recipeint` to `recipient`
___
[Crowdfund.sol: L258](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L258)
```solidity
    // Deploys and initializes a a `Party` instance via the `PartyFactory`
```
Remove repeated word `a`
___
___

### Inconsistent wrap of long comments
Long comments are wrapped using inconsistent syntax in the contest. Some are wrapped conventionally, using a few additional spaces after the `///` or `//` in the second (and any subsequent lines), as shown below:

[PartyGovernance.sol: L713-714](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L713-L714)
```solidity
    /// @dev proposal.cancelDelay seconds must have passed since it was first
    ///       executed for this to be valid.
```
This flags for the reader that the comment is continuing. However, the majority of multiline comments in the contest are not indented in the second (and any subsequent wrapped lines). For example: 

[TokenDistributor.sol: L67-68](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L67-L68)
```solidity
    /// Allows one to simply transfer and call `createDistribution()` without
    /// fussing with allowances.
```
In addition, some comments wrap using extra-wide indentations, as shown below: 

[CrowdfundFactory.sol: L55-60](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundFactory.sol#L55-L60)
```solidity
    /// @notice Create a new crowdfund to bid on an auction for a specific NFT
    ///         (i.e. with a known token ID).
    /// @param opts Options used to initialize the crowdfund. These are fixed
    ///             and cannot be changed later.
    /// @param createGateCallData Encoded calldata used by `createGate()` to create
    ///                           the crowdfund if one is specified in `opts`.
```
Suggestion: For increased readability, wrap multiline comments using consistent syntax throughout `Party DAO`
___
___

### Named `return` variable not used
The existence of a named return variable that is not subsequently used (here, `newGateKeeperId`) is potentially perplexing
___
[CrowdfundFactory.sol: L107-122](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundFactory.sol#L107-L122)
```solidity
    function _prepareGate(
        IGateKeeper gateKeeper,
        bytes12 gateKeeperId,
        bytes memory createGateCallData
    )
        private
        returns (bytes12 newGateKeeperId)
    {
        if (
            address(gateKeeper) == address(0) ||
            gateKeeperId != bytes12(0)
        ) {
            // Using an existing gate on the gatekeeper
            // or not using a gate at all.
            return gateKeeperId;
        }
```
___
___

### Missing `require` revert strings
A `require` message should be included to enable users to understand reason for failure

Recommendation: Add a brief (<= 32 char) explanatory message to each of the three `requires` referenced below. Also, consider using custom error codes (which would be cheaper than revert strings).
___
[ReadOnlyDelegateCall.sol: L20](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L20)
```solidity
        require(msg.sender == address(this));
```
___
[ProposalExecutionEngine.sol: L246](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L246)
```solidity
        require(proposalType != ProposalType.Invalid);
```
___
[ProposalExecutionEngine.sol: L247](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L247)
```solidity
        require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));
```
___
___

### Use consistent initialization of counters in `for` loops 
One of the `for` loop counters is not initiated to zero, as shown below: 

[CollectionBuyCrowdfund.sol: L62](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62)
```solidity
        for (uint256 i; i < hosts.length; i++) {
```
However, the remaining 13 of the `for` loop counters are initialized to zero, as the example below shows:

[ArbitraryCallsProposal.sol: L52](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52)
```solidity
            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```
It is not necessary to initialize `for` loop counters to zero since this is their default value. For consistency, it makes sense to omit counter initializations in all of the `for` loops
___
___

