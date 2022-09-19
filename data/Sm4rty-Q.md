

## 1. Use safeTransfer/safeTransferFrom consistently instead of transfer/transferFrom

It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

### Instances:

```
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol:143:        super.transferFrom(owner, to, tokenId);
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:301:            preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
``` 
### Reference:

This [similar medium-severity finding](https://consensys.net/diligence/audits/2021/01/fei-protocol/#unchecked-return-value-for-iweth-transfer-call) from Consensys Diligence Audit of Fei Protocol.

### Recommended Mitigation Steps:

Consider using safeTransfer/safeTransferFrom or require() consistently.


-----

## 2. USE SAFEAPPROVE INSTEAD OF APPROVE

This is probably an oversight since SafeERC20 was imported and safeTransfer() was used for ERC20 token transfers. Nevertheless, note that approve() will fail for certain token implementations that do not return a boolean value (). Hence it is recommend to use safeApprove().

### Instances:

```
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol:53:        data.token.approve(address(VAULT_FACTORY), data.tokenId);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:256:        token.approve(conduit, tokenId);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:367:            token.approve(address(0), tokenId);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol:143:        token.approve(address(ZORA), tokenId);
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol:173:                if (selector == IERC721.approve.selector) {
``` 
### Recommended Mitigation Steps

Update to safeapprove in the function.


-----

## 3. _safeMint() should be used rather than _mint() wherever possible

`_mint()` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`. Both open [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/Rari-Capital/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function so that NFTs aren’t lost if they’re minted to contracts that cannot transfer them back out.

### Instances

```
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol:132:        _mint(owner, tokenId);
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:439:                _mint(contributor);
``` 
### Recommendations:

Use _safeMint() instead of _mint().


-----


## 4. Unused receive() function will lock Ether in contract

If the intention is for the Ether to be used, the function should call another function, otherwise, it should revert.

### Instances:

```
party-contracts-c4/contracts/party/Party.sol:47:    receive() external payable {}
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol:144:    receive() external payable {}
``` 
### References

[https://code4rena.com/reports/2022-05-sturdy#l-06-unused-receive-function-will-lock-ether-in-contract](https://code4rena.com/reports/2022-05-sturdy#l-06-unused-receive-function-will-lock-ether-in-contract)


-----

## 5. Avoid using Floating Pragma:

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

### Instances:

```
party-contracts-c4/contracts/globals/Globals.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/globals/IGlobals.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/Proxy.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibAddress.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibERC20Compat.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibRawResult.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/EIP165.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/Implementation.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibSafeERC721.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibSafeCast.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyGovernance.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyFactory.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/party/IPartyFactory.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/party/Party.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/market-wrapper/IMarketWrapper.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/gatekeepers/IGateKeeper.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/ITokenDistributorParty.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/ITokenDistributor.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/TokenDistributor.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/IProposalExecutionEngine.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/LibProposal.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalStorage.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/IOpenseaExchange.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/IOpenseaConduitController.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/FractionalV1.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC721Receiver.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/ERC721Receiver.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/ERC1155Receiver.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC20.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC721.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC1155.sol:2:pragma solidity ^0.8;
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol:2:pragma solidity ^0.8;
``` 
Recommend using fixed solidity version

### References:

[https://code4rena.com/reports/2022-04-phuture#g-20-use-a-more-recent-version-of-solidity](https://code4rena.com/reports/2022-04-phuture#g-20-use-a-more-recent-version-of-solidity)


-----



## 6. Use of Block.Timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

### Instances:
```
party-contracts-c4/contracts/party/PartyGovernance.sol:539:                proposedTime: uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol:605:            info.values.passedTime = uint40(block.timestamp);
party-contracts-c4/contracts/party/PartyGovernance.sol:681:            if (proposal.maxExecutableTime < block.timestamp) {
party-contracts-c4/contracts/party/PartyGovernance.sol:684:                    uint40(block.timestamp)
party-contracts-c4/contracts/party/PartyGovernance.sol:687:            proposalState.values.executedTime = uint40(block.timestamp);
party-contracts-c4/contracts/party/PartyGovernance.sol:695:        proposalState.values.completedTime = uint40(block.timestamp);
party-contracts-c4/contracts/party/PartyGovernance.sol:753:            if (block.timestamp < cancelTime) {
party-contracts-c4/contracts/party/PartyGovernance.sol:755:                    uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol:762:        proposalState.values.completedTime = uint40(block.timestamp | UINT40_HIGH_BIT);
party-contracts-c4/contracts/party/PartyGovernance.sol:903:            timestamp: uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol:940:                    timestamp: uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol:963:                    timestamp: uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol:1036:        uint40 t = uint40(block.timestamp);
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol:73:        expiry = uint40(opts.duration + block.timestamp);
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol:159:        if (block.timestamp >= expiry) {
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol:118:        expiry = uint40(opts.duration + block.timestamp);
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol:276:        if (block.timestamp >= expiry) {
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:166:                        minExpiry: (block.timestamp + zoraTimeout).safeCastUint256ToUint40()
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:210:            uint256 expiry = block.timestamp + uint256(data.duration);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:263:        orderParams.startTime = block.timestamp;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:364:        } else if (expiry <= block.timestamp) {
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol:117:                minExpiry: (block.timestamp + data.timeout).safeCastUint256ToUint40()
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol:159:            uint40(block.timestamp + timeout)
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol:188:                if (minExpiry > uint40(block.timestamp)) {
``` 
### References:

https://github.com/code-423n4/2022-04-dualityfocus-findings/issues/33


-----