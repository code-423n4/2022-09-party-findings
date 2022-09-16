## Typo Mistake
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L12

The comment should be rewritten as:

/// @notice Factory used to deploy new proxified `Party` instances. 

## Two-step Transfer of Ownership
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L27-L28

`transferMultiSig()` should be implemented giving ownership to another address in a pending state. And then, a `claimMultiSig()` should be introduced for the new owner(s) to claim ownership. This will ensure the new set of owners are going to be fully aware of their ownership in new duties. 

Where possible, include a zero address check for `transferMultiSig()` to avoid non-functional calls on `claimMultiSig()`, as well as emitting an event on the new ownership transferred. 