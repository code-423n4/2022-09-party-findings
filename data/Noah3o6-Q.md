-> EVENT IS MISSING INDEXED FIELDS
Each event should use three indexed fields if there are three or more fields.

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#:~:text=event%20Won(Party%20party%2C%20IERC721%20token%2C%20uint256%20tokenId%2C%20uint256%20settledPrice)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#:~:text=event%20Burned(address%20contributor%2C%20uint256%20ethUsed%2C%20uint256%20ethOwed%2C%20uint256%20votingPower)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#:~:text=event%20Contributed(address%20contributor%2C%20uint256%20amount%2C%20address%20delegate%2C%20uint256%20previousTotalContributions)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#:~:text=event%20DistributionFeeClaimed(,uint256%20amount
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#:~:text=event%20DistributionClaimedByPartyToken(,uint256%20amountClaimed
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#:~:text=event%20Proposed(,Proposal%20proposal
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#:~:text=event%20ProposalAccepted(,uint256%20weight
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#:~:text=event%20PartyInitialized(GovernanceOpts%20opts%2C%20IERC721%5B%5D%20preciousTokens%2C%20uint256%5B%5D%20preciousTokenIds)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#:~:text=event%20ProposalExecuted(uint256%20indexed%20proposalId%2C%20address%20executor%2C%20bytes%20nextProgressData)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#:~:text=event%20DistributionCreated(ITokenDistributor.TokenType%20tokenType%2C%20address%20token%2C%20uint256%20tokenId)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#:~:text=event%20ArbitraryCallExecuted(uint256%20proposalId%2C%20uint256%20idx%2C%20uint256%20count)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#:~:text=event%20FractionalV1VaultCreated(,uint256%20listPrice
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#:~:text=event%20OpenseaOrderListed(,)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#:~:text=event%20OpenseaOrderSold(,uint256%20listPrice
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#:~:text=event%20OpenseaOrderExpired(,uint256%20expiry
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#:~:text=event%20OrderValidated(,uint256%20counter
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#:~:text=event%20ZoraAuctionCreated(,uint40%20timeoutTime
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC1155.sol#:~:text=event%20TransferSingle(,uint256%20amount
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC1155.sol#:~:text=event%20TransferBatch(,uint256%5B%5D%20amounts
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC1155.sol#:~:text=event%20ApprovalForAll(address%20indexed%20owner%2C%20address%20indexed%20operator%2C%20bool%20approved)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC20.sol#:~:text=event%20Transfer(address%20indexed%20owner%2C%20address%20indexed%20to%2C%20uint256%20amount)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC20.sol#:~:text=event%20Approval(address%20indexed%20owner%2C%20address%20indexed%20spender%2C%20uint256%20allowance)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721.sol#:~:text=event%20ApprovalForAll(address%20indexed%20owner%2C%20address%20indexed%20operator%2C%20bool%20approved)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/markets/IKoansAuctionHouse.sol#:~:text=event%20AuctionCreated(uint256%20indexed%20koanId%2C%20uint256%20startTime%2C%20uint256%20endTime)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/markets/IKoansAuctionHouse.sol#:~:text=event%20AuctionBid(uint256%20indexed%20koanId%2C%20address%20sender%2C%20uint256%20value%2C%20bool%20extended)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/markets/IKoansAuctionHouse.sol#:~:text=event%20AuctionSettled(uint256%20indexed%20koanId%2C%20address%20winner%2C%20uint256%20amount)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/markets/INounsAuctionHouse.sol#:~:text=event%20AuctionCreated(uint256%20indexed%20nounId%2C%20uint256%20startTime%2C%20uint256%20endTime)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/markets/INounsAuctionHouse.sol#:~:text=event%20AuctionBid(uint256%20indexed%20nounId%2C%20address%20sender%2C%20uint256%20value%2C%20bool%20extended)%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/markets/INounsAuctionHouse.sol#:~:text=event%20AuctionSettled(uint256%20indexed%20nounId%2C%20address%20winner%2C%20uint256%20amount)%3B


-> _SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE

_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=%3D%3D%200)%20%7B-,_mint(contributor)%3B,-%7D
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=party_.-,mint,-(
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#:~:text=function-,_mint,-(address%20owner
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#:~:text=safeCastUint256ToInt192()%2C%20delegate)%3B-,_mint,-(owner%2C%20tokenId)%3B

->USE A MORE RECENT VERSION OF SOLIDITY

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#:~:text=pragma%20solidity%20%5E0.8%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#:~:text=pragma%20solidity%20%5E0.8%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#:~:text=//%20receives.-,uint16,-splitBps%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#:~:text=if%20non%2Dnull).-,bytes12,-gateKeeperId%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#:~:text=pragma%20solidity%20%5E0.8%3B
