# 1  USE SAFETRANSFER/SAFETRANSFERFROM CONSISTENTLY INSTEAD OF TRANSFER/TRANSFERFROM
It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract. 

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L143
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301
# 2 RETURN VALUES OF TRANSFER()/TRANSFERFROM() NOT CHECKED
Not all IERC20 implementations revert() when there’s a failure in transfer()/transferFrom(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L143
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301
# 3  USE OF BLOCK.TIMESTAMP
block.timestamp is being used several times
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L188 
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L118
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L684


Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

Block timestamps should not be used for entropy or generating random numbers—i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state.

Time-sensitive logic is sometimes required; e.g., for unlocking contracts (time-locking), completing an ICO after a few weeks, or enforcing expiry dates. It is sometimes recommended to use block.number and an average block time to estimate times; with a 10 second block time, 1 week equates to approximately, 60480 blocks. Thus, specifying a block number at which to change a contract state can be more secure, as miners are unable to easily manipulate the block number.

INTERNAL FUNCTIONS ONLY CALLED ONCE CAN BE INLINED TO SAVE GAS
Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

## Recommended Mitigation Steps

USE A MORE RECENT VERSION OF SOLIDITY and OpenZeppelin Library
current solidity version is 0.8.15 meanwhile project is using 0.8.6.
OpenZeppelin library currently 4.7.2 while the project is using 4.4.2.
It's best to use latest version because there are some features improvement and security patch.

Use block.number instead of  block.timestamp or now to reduce the risk of
MEV attacks

### here some reference :
https://www.bookstack.cn/read/ethereumbook-en/spilt.14.c2a6b48ca6e1e33c.md
https://ethereum.stackexchange.com/questions/108033/what-do-i-need-to-be-careful-about-when-using-block-timestamp