First QA issue - require statements are missing strings

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol
246: require(proposalType != ProposalType.Invalid);
247: require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol
20: require(msg.sender == address(this));