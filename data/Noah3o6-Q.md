-> EVENT IS MISSING INDEXED FIELDS
Each event should use three indexed fields if there are three or more fields.

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#:~:text=event%20Won(Party%20party%2C%20IERC721%20token%2C%20uint256%20tokenId%2C%20uint256%20settledPrice)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#:~:text=event%20Burned(address%20contributor%2C%20uint256%20ethUsed%2C%20uint256%20ethOwed%2C%20uint256%20votingPower)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#:~:text=event%20Contributed(address%20contributor%2C%20uint256%20amount%2C%20address%20delegate%2C%20uint256%20previousTotalContributions)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#:~:text=event%20DistributionFeeClaimed(,uint256%20amount
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#:~:text=event%20DistributionClaimedByPartyToken(,uint256%20amountClaimed

-> _SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE

_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=%3D%3D%200)%20%7B-,_mint(contributor)%3B,-%7D
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=party_.-,mint,-(
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#:~:text=function-,_mint,-(address%20owner

->USE A MORE RECENT VERSION OF SOLIDITY

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#:~:text=pragma%20solidity%20%5E0.8%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#:~:text=pragma%20solidity%20%5E0.8%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#:~:text=//%20receives.-,uint16,-splitBps%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#:~:text=if%20non%2Dnull).-,bytes12,-gateKeeperId%3B
