### No vote duration time limit

partygovernance.sol

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L75

there should be some standard put into place for how long the vote duration is set ofr to stop an unreasonable time being put into place, so that a proposal can not be left stale due to unreasonable time constraints


### Can pay funds but cannot withdraw

party.sol

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/Party.sol#L47

has a payable function but no withdraw or way to withdraw funds, consider adding such a function or fallback function incase the need ever arise so funds can never be locked into the contract
