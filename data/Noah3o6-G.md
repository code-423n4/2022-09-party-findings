-> X = X + Y IS CHEAPER THAN X += Y (same for X = X - Y IS CHEAPER THAN X -= Y)

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#:~:text=ethContributed%20%2B%3D%20contributions%5Bi%5D.amount%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=ethUsed%20%2B%3D%20c.amount%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=ethUsed%20%2B%3D%20partialEthUsed%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=votingPower%20%2B%3D%20(splitBps_%20*%20totalEthUsed%20%2B%20(1e4%20%2D%201))%20/%201e4%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=totalContributions%20%2B%3D%20amount%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=lastContribution.amount%20%2B%3D%20amount%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#:~:text=_storedBalances%5BbalanceId%5D%20%2D%3D%20amount%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#:~:text=values.votes%20%2B%3D%20votingPower%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#:~:text=%3D%20amounts%5Bi%5D%3B-,balanceOf%5Bfrom%5D%5Bid%5D%20%2D%3D%20amount%3B,-balanceOf%5Bto%5D%5Bid
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#:~:text=id%5D%20%2D%3D%20amount%3B-,balanceOf%5Bto%5D%5Bid%5D%20%2B%3D%20amount%3B,-//%20An%20array%20can%27t
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#:~:text=internal%20virtual%20%7B-,balanceOf%5Bto%5D%5Bid%5D%20%2B%3D%20amount%3B,-emit%20TransferSingle(
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#:~:text=balanceOf%5Bto%5D%5Bids%5Bi%5D%5D%20%2B%3D%20amounts%5Bi%5D%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#:~:text=balanceOf%5Bfrom%5D%5Bids%5Bi%5D%5D%20%2D%3D%20amounts%5Bi%5D%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#:~:text=internal%20virtual%20%7B-,balanceOf%5Bfrom%5D%5Bid%5D%20%2D%3D%20amount%3B,-emit%20TransferSingle(
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC20.sol#:~:text=totalSupply%20%2B%3D%20amount%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC20.sol#:~:text=internal%20virtual%20%7B-,balanceOf%5Bfrom%5D%20%2D%3D%20amount%3B,-//%20Cannot%20underflow%20because
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC20.sol#:~:text=totalSupply%20%2D%3D%20amount%3B

-> ++i costs less gas compared to i++ or i += 1 (Also --i costs less gas compared to i-- or i -= 1)

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#:~:text=hosts.length%3B-,i%2B%2B),-%7B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/PartyHelpers.sol#:~:text=members.length%3B-,i%2B%2B),-%7B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/PartyHelpers.sol#:~:text=voters.length%3B-,i%2B%2B),-%7B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/PartyHelpers.sol#:~:text=i%20%3C%20count%3B-,i%2B%2B),-%7B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC721.sol#:~:text=unchecked%20%7B-,_balanceOf%5Bto%5D%2B%2B%3B,-%7D
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC721.sol#:~:text=_balanceOf%5Bowner%5D%2D%2D%3B

-> REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE() STRINGS TO SAVE GAS

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/market-wrapper/KoansMarketWrapper.sol#:~:text=%22KoansMarketWrapper%3A%3AgetMinimumBid%3A%20Auction%20not%20active%22
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/market-wrapper/KoansMarketWrapper.sol#:~:text=%22KoansMarketWrapper%3A%3AgetCurrentHighestBidder%3A%20Auction%20not%20active%22
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/market-wrapper/NounsMarketWrapper.sol#:~:text=%22NounsMarketWrapper%3A%3AgetCurrentHighestBidder%3A%20Auction%20not%20active%22
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC20.sol#:~:text=%22Permit(address%20owner%2Caddress%20spender%2Cuint256%20value%2Cuint256%20nonce%2Cuint256%20deadline)%22


-> USAGE OF UINTS/INTS SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#:~:text=//%20receives.-,uint16,-splitBps%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#:~:text=contract%20to%20use.-,bytes12,-gateKeeperId%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#:~:text=//%20receives.-,uint16,-splitBps%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#:~:text=contract%20to%20use.-,bytes12,-gateKeeperId%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#:~:text=where%2010%2C000%20%3D%3D%20100%25.-,uint16,-passThresholdBps%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#:~:text=for%20governance%20distributions.-,uint16,-feeBps%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#:~:text=where%2010%2C000%20%3D%20100%25.-,uint16,-public%20splitBps%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=returns%20(-,bytes16,-h)
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#:~:text=address%20payable%20feeRecipient%3B-,uint16,-feeBps%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#:~:text=of%20the%20%60DistributionInfo%60.-,bytes15,-distributionHash15%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/market-wrapper/KoansMarketWrapper.sol#:~:text=the%20increment%20buffer-,uint8,-minBidIncrementPercentage%20%3D%20market
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/market-wrapper/ZoraMarketWrapper.sol#:~:text=internal%20immutable%20market%3B-,uint8,-internal%20immutable%20minBidIncrementPercentage

->IT COSTS MORE GAS TO INITIALIZE NON-CONSTANT/NON-IMMUTABLE VARIABLES TO ZERO THAN TO LET THE DEFAULT OF ZERO BE APPLIED

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#:~:text=infos.length)%3B-,for%20(uint256%20i%20%3D%200%3B,-i%20%3C%20infos
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#:~:text=%7B-,for%20(uint256%20i%20%3D%200%3B,-i%20%3C%20infos
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#:~:text=.value%3B-,for%20(uint256%20i%20%3D%200%3B,-i%20%3C%20calls


->USING BOOLS FOR STORAGE INCURS OVERHEAD

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#:~:text=mapping(uint256%20%3D%3E%20bool)%20hasPartyTokenClaimed%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#:~:text=mapping(uint256%20%3D%3E%20bool)%20hasPartyTokenClaimed%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#:~:text=mapping(uint256%20%3D%3E%20bool)
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#:~:text=mapping(bytes32%20%3D%3E%20bool)
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#:~:text=mapping(address%20%3D%3E%20bool)
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/PartyGovernanceNFTRenderer.sol#:~:text=uint256%20lastProposalId%3B-,mapping(address%20%3D%3E%20bool),-isHost%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/PartyGovernanceNFTRenderer.sol#:~:text=(address%20%3D%3E-,mapping(address%20%3D%3E%20bool),-)%20isApprovedForAll%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#:~:text=mapping(address%20%3D%3E%20bool)
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC721.sol#:~:text=mapping(address%20%3D%3E%20bool)
