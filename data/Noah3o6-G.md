-> X = X + Y IS CHEAPER THAN X += Y (same for X = X - Y IS CHEAPER THAN X -= Y)

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#:~:text=ethContributed%20%2B%3D%20contributions%5Bi%5D.amount%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=ethUsed%20%2B%3D%20c.amount%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=ethUsed%20%2B%3D%20partialEthUsed%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=votingPower%20%2B%3D%20(splitBps_%20*%20totalEthUsed%20%2B%20(1e4%20%2D%201))%20/%201e4%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=totalContributions%20%2B%3D%20amount%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301:~:text=lastContribution.amount%20%2B%3D%20amount%3B

-> ++i costs less gas compared to i++ or i += 1 (Also --i costs less gas compared to i-- or i -= 1)

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#:~:text=hosts.length%3B-,i%2B%2B),-%7B

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

->USING BOOLS FOR STORAGE INCURS OVERHEAD

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#:~:text=mapping(uint256%20%3D%3E%20bool)%20hasPartyTokenClaimed%3B
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#:~:text=mapping(uint256%20%3D%3E%20bool)%20hasPartyTokenClaimed%3B
