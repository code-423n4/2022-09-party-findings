## Loop on unbounded arrays can lead to DOS attacks when the size gets too big
In loop https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230 and https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239, `infos` array is unbounded, so when the size of the array is too large, it may use up all gas and lead to DOS

## Missing emits for events
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L327 : `disableEmergencyActions` is a critical function and should emit

## Use of block attributes like block.timestamp are risky, as they can be manipulated by miners
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L118
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L276
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L73
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L159
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L539
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L605
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L681

## Address should be 0-checked. Missing 0-address check, can lead to unintended issues, which may cause re-deployment of the contract
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L23
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L141
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L150

## Avoid floating pragma. The version should be locked to a particular compiler version
All of the in-scope contracts uses `^0.8` , so it should be locked to a particular version

## Update to the latest compiler version
Update to compiler version 0.8.15 in all in-scope contracts

## Add extra storage gap on base contracts for future development purpose
Base contract:  https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol 
Base contract: https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

