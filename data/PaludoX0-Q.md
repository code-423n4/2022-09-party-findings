https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L352
uint128 fee = supply * args.feeBps / 1e4, better to be written as uint128 fee = (supply * args.feeBps) / 1e4; in order to give priority to moltiplication and avoid loosing roundings

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L327
Since emergency functions can be disabled only, it would be better to disable for one or more party, not only for all. It could be that 
some parties are still immature and emergency functions are still needed.  

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L379
There's no check that _storedBalances[balanceId] > amount, If not function revert with underflow error but it would be better to set an error/event message
