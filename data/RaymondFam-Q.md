## Typo Mistake
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L12

The comment should be rewritten as:

/// @notice Factory used to deploy new proxified `Party` instances. 

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L13

The comment should be rewritten as:

// Inherited by contracts to perform read-only delegate calls.

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L26

The comment should be rewritten as:

// A temporary state set by the contract during complex operations to

## Two-step Transfer of Ownership
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L27-L28

`transferMultiSig()` should be implemented giving ownership to another address in a pending state. And then, a `claimMultiSig()` should be introduced for the new owner(s) to claim ownership. This will ensure the new set of owners are going to be fully aware of their ownership in new duties. 

Where possible, include a zero address check for `transferMultiSig()` to avoid non-functional calls on `claimMultiSig()`, as well as emitting an event on the new ownership transferred. 

## Un-indexed Parameters in Events
Consider indexing parameters for events, serving as logs filter when looking for specifically wanted data. Up to three parameters in an event function can receive the attribute indexed which will cause the respective arguments to be treated as log topics instead of data. The following links are some of the instances entailed:

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/IPartyFactory.sol#L11
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L35

## Disable `transferFrom`
ERC721 has both `safeTransferFrom` and `transferFrom`, where `safeTransferFrom` throws an error if the receiving contract's `onERC721Received` method doesn't return a specific magic number. This will ensure a receiving contract is capable of receiving the token to prevent a permanent loss. Where possible, disable `transferFrom()` by rewriting it as follows to prevent calls emanating outside the UI: 

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L136-L144

```
    error VotesTransferDisabled();

    function transferFrom(address owner, address to, uint256 tokenId)
        public
        override
        onlyDelegateCall
    {
        revert VotesTransferDisabled();
    }
```
## Zero Address and Zero Value Checks

Zero address and zero value checks should be implemented when initializer is delegatecalled by `Proxy` constructor. This is because there is no setter functions for these options or initData parameters. In the event a mistake was committed, not only that all calls associated with it would be non-functional, the contract would also have to be redeployed. Here are some of the instances entailed:

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L64-L88
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L33-L45

## Use call instead of transferEth
`call`, which returns a boolean value indicating success or failure, in combination with re-entrancy guard is the recommended method to use after December 2019. And, guard against re-entrancy by making all state changes before calling other contracts using re-entrancy guard modifier. Here's one instance entailed:

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L487

## No `withdraw()` for Additional ETH Left in the Contract
The following line of code could receive ETH returned from an auction and also other sources including someone forces (suicides) ETH into the contract.  The inherited functions from `Crowdfund.sol` seem to only cater for address(this).balance at the initializer for the initial contributors, and refund `ethOwed` based on unused contributions. Consider implementing a `withdraw()` just in case there will be additional ETH stuck in the contract.

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L144
