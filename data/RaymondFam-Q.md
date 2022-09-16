## Typo Mistake
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L12

The comment should be rewritten as:

/// @notice Factory used to deploy new proxified `Party` instances. 

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

