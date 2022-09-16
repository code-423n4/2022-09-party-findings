First Gas optimization - Using unchecked blocks can save gas.

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol
170: state.remainingMemberSupply = remainingMemberSupply - amountClaimed;
341: supply = (args.currentTokenBalance - _storedBalances[balanceId])
353: uint128 memberSupply = supply - fee;
381: _storedBalances[balanceId] -= amount;

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol
72: ethAvailable -= calls[i].value;

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol
118: expiry = uint40(opts.duration + block.timestamp);

Second Gas optimization - Multiple mapping of addres can be combined into a single mapping of an struct.

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol
207: mapping(address => bool) public isHost;
209: mapping(address => address) public delegationsByVoter;
215: mapping(address => VotingPowerSnapshot[]) private _votingPowerSnapshotsByVoter;

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol
112: mapping(address => address) public delegationsByContributor;
115: mapping (address => Contribution[]) private _contributionsByContributor;
