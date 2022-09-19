https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L28
Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents replace the bits taken up by the boolean, and then write back.
The values being non-zero value makes deployment a bit more expensive, but in exchange the refund on every call will be lower in amount.
Use following variables and constant to WRITE /READ
    variable uint256 feeClaimed;
    uint256 private constant _WAS_FEE_NOT_CLAIMED = 1;
    uint256 private constant _WAS_FEE_CLAIMED = 2;


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62
In order to save gas hosts.length to be saved in a temporary variable

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L180
In order to save gas contributors.length to be saved in a temporary variable

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L300
In order to save gas preciousToken.length to be saved in a temporary variable