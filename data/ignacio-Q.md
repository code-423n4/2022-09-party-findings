# 1  USE SAFETRANSFER/SAFETRANSFERFROM CONSISTENTLY INSTEAD OF TRANSFER/TRANSFERFROM
It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract. 

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L143
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301
# 2 RETURN VALUES OF TRANSFER()/TRANSFERFROM() NOT CHECKED
Not all IERC20 implementations revert() when there’s a failure in transfer()/transferFrom(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L143
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301
# 3 Use of Block Timestamp ,A miner can manipulate the block timestamp which can be used to their advantage to attack a smart contract via Block Timestamp Manipulation

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L188 
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L118
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L684
Blocks have a timestamp field in the block header which is set by the miner and can be changed to whatever they want (with some restriction). In order for a miner to set a block timestamp they need to win the next block and abide by the following time constrains:

The next timestamp is after the last block timestamp
The timestamp can not be too far into the future
If the miner wins a block they can slightly change the block timestamp to their advantage.

## Impact
Dishonest  Miners can influence the value of block.timestamp to perform Maximal Extractable Value (MEV) attacks.
The use of now creates a risk that time manipulation can be performed to manipulate price oracles. Miners can modify the timestamp by up to 900 seconds , Usually to an extent of few seconds on Ethereum, or generally few percent of the block time on any EVM-compatible PoW network.

## Recommended Mitigation Steps
Use block.number instead of  block.timestamp or now to reduce the risk of
MEV attacks

### here some reference :
https://www.bookstack.cn/read/ethereumbook-en/spilt.14.c2a6b48ca6e1e33c.md
https://ethereum.stackexchange.com/questions/108033/what-do-i-need-to-be-careful-about-when-using-block-timestamp