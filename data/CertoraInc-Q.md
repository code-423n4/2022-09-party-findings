# QA Report
* The receive function of the `AuctionCrowdfund` is empty, so ether can be transferred to the implementation and get stuck.
* Typo in [this line](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L258) of the `Crowdfund` contract
* Missing length check in the `Crowdfund._createParty()` function
