 # 1. require()/revert() statements should have descriptive reason strings
 ### Instances:
 // Links to github files
 [ProposalExecutionEngine.sol:L246](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L246)
[ProposalExecutionEngine.sol:L247](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L247)
[ReadOnlyDelegateCall.sol:L20](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L20)
 ### Actual Codes Used:
```
contracts/proposals/ProposalExecutionEngine.sol:246:        require(proposalType != ProposalType.Invalid);
contracts/proposals/ProposalExecutionEngine.sol:247:        require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));
contracts/utils/ReadOnlyDelegateCall.sol:20:        require(msg.sender == address(this));
```
----
# 2.`EVENT` IS MISSING INDEXED FIELDS
### Description
Each event should use three indexed fields if there are three or more fields
### Instances
//Links to Githubfile
[ListOnOpenseaProposal.sol:L86](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L86)
[PartyGovernance.sol:L165](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L165)
[IERC1155.sol:L20](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/tokens/IERC1155.sol#L20)
[IERC20.sol:L6](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/tokens/IERC20.sol#L6)
[IERC20.sol:L7](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/tokens/IERC20.sol#L7)
[IERC721.sol:L8](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/tokens/IERC721.sol#L8)
[ITokenDistributor.sol:L39](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/ITokenDistributor.sol#L39)
### Actual Codes Used:
```
contracts/proposals/ListOnOpenseaProposal.sol:86:        address indexed offerer,
contracts/party/PartyGovernance.sol:165:    event ProposalExecuted(uint256 indexed proposalId, address executor, bytes nextProgressData);
contracts/tokens/IERC1155.sol:20:    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
contracts/tokens/IERC20.sol:6:    event Transfer(address indexed owner, address indexed to, uint256 amount);
contracts/tokens/IERC20.sol:7:    event Approval(address indexed owner, address indexed spender, uint256 allowance);
contracts/tokens/IERC721.sol:8:    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
contracts/distribution/ITokenDistributor.sol:39:        address indexed feeRecipient,
```
----
# 3.Avoid using Floating Pragma:

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
 ### Instances:
 // Links to github files
[IZoraAuctionHouse.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/markets/IZoraAuctionHouse.sol#L2)
[IGateKeeper.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/gatekeepers/IGateKeeper.sol#L2)
[ArbitraryCallsProposal.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L2)
[ProposalExecutionEngine.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L2)
[FractionalV1.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/vendor/FractionalV1.sol#L2)
[IOpenseaConduitController.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/vendor/IOpenseaConduitController.sol#L2)
[IOpenseaExchange.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/vendor/IOpenseaExchange.sol#L2)
[ListOnZoraProposal.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L2)
[ProposalStorage.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalStorage.sol#L2)
[LibProposal.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L2)
[ListOnOpenseaProposal.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L2)
[IProposalExecutionEngine.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/IProposalExecutionEngine.sol#L2)
[FractionalizeProposal.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/FractionalizeProposal.sol#L2)
[PartyFactory.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyFactory.sol#L2)
[PartyGovernance.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L2)
[IPartyFactory.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/IPartyFactory.sol#L2)
[PartyGovernanceNFT.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L2)
[Party.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/Party.sol#L2)
[IGlobals.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/IGlobals.sol#L2)
[Globals.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L2)
[Crowdfund.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L2)
[BuyCrowdfund.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfund.sol#L2)
[CollectionBuyCrowdfund.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L2)
[CrowdfundFactory.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundFactory.sol#L2)
[AuctionCrowdfund.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L2)
[BuyCrowdfundBase.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfundBase.sol#L2)
[CrowdfundNFT.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundNFT.sol#L2)
[IERC721Receiver.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/tokens/IERC721Receiver.sol#L2)
[IERC1155.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/tokens/IERC1155.sol#L2)
[ERC1155Receiver.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/tokens/ERC1155Receiver.sol#L2)
[ERC721Receiver.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/tokens/ERC721Receiver.sol#L2)
[IERC20.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/tokens/IERC20.sol#L2)
[IERC721.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/tokens/IERC721.sol#L2)
[ReadOnlyDelegateCall.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L2)
[LibSafeERC721.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/LibSafeERC721.sol#L2)
[LibERC20Compat.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/LibERC20Compat.sol#L2)
[LibRawResult.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/LibRawResult.sol#L2)
[LibAddress.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/LibAddress.sol#L2)
[LibSafeCast.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/LibSafeCast.sol#L2)
[EIP165.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/EIP165.sol#L2)
[Implementation.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/Implementation.sol#L2)
[Proxy.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/Proxy.sol#L2)
[IMarketWrapper.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/market-wrapper/IMarketWrapper.sol#L2)
[ITokenDistributorParty.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/ITokenDistributorParty.sol#L2)
[TokenDistributor.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L2)
[ITokenDistributor.sol:L2](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/ITokenDistributor.sol#L2)
 ### Actual Codes Used:
```
contracts/vendor/markets/IZoraAuctionHouse.sol:2:pragma solidity ^0.8;
contracts/gatekeepers/IGateKeeper.sol:2:pragma solidity ^0.8;
contracts/proposals/ArbitraryCallsProposal.sol:2:pragma solidity ^0.8;
contracts/proposals/ProposalExecutionEngine.sol:2:pragma solidity ^0.8;
contracts/proposals/vendor/FractionalV1.sol:2:pragma solidity ^0.8;
contracts/proposals/vendor/IOpenseaConduitController.sol:2:pragma solidity ^0.8;
contracts/proposals/vendor/IOpenseaExchange.sol:2:pragma solidity ^0.8;
contracts/proposals/ListOnZoraProposal.sol:2:pragma solidity ^0.8;
contracts/proposals/ProposalStorage.sol:2:pragma solidity ^0.8;
contracts/proposals/LibProposal.sol:2:pragma solidity ^0.8;
contracts/proposals/ListOnOpenseaProposal.sol:2:pragma solidity ^0.8;
contracts/proposals/IProposalExecutionEngine.sol:2:pragma solidity ^0.8;
contracts/proposals/FractionalizeProposal.sol:2:pragma solidity ^0.8;
contracts/party/PartyFactory.sol:2:pragma solidity ^0.8;
contracts/party/PartyGovernance.sol:2:pragma solidity ^0.8;
contracts/party/IPartyFactory.sol:2:pragma solidity ^0.8;
contracts/party/PartyGovernanceNFT.sol:2:pragma solidity ^0.8;
contracts/party/Party.sol:2:pragma solidity ^0.8;
contracts/globals/IGlobals.sol:2:pragma solidity ^0.8;
contracts/globals/Globals.sol:2:pragma solidity ^0.8;
contracts/crowdfund/Crowdfund.sol:2:pragma solidity ^0.8;
contracts/crowdfund/BuyCrowdfund.sol:2:pragma solidity ^0.8;
contracts/crowdfund/CollectionBuyCrowdfund.sol:2:pragma solidity ^0.8;
contracts/crowdfund/CrowdfundFactory.sol:2:pragma solidity ^0.8;
contracts/crowdfund/AuctionCrowdfund.sol:2:pragma solidity ^0.8;
contracts/crowdfund/BuyCrowdfundBase.sol:2:pragma solidity ^0.8;
contracts/crowdfund/CrowdfundNFT.sol:2:pragma solidity ^0.8;
contracts/tokens/IERC721Receiver.sol:2:pragma solidity ^0.8;
contracts/tokens/IERC1155.sol:2:pragma solidity ^0.8;
contracts/tokens/ERC1155Receiver.sol:2:pragma solidity ^0.8;
contracts/tokens/ERC721Receiver.sol:2:pragma solidity ^0.8;
contracts/tokens/IERC20.sol:2:pragma solidity ^0.8;
contracts/tokens/IERC721.sol:2:pragma solidity ^0.8;
contracts/utils/ReadOnlyDelegateCall.sol:2:pragma solidity ^0.8;
contracts/utils/LibSafeERC721.sol:2:pragma solidity ^0.8;
contracts/utils/LibERC20Compat.sol:2:pragma solidity ^0.8;
contracts/utils/LibRawResult.sol:2:pragma solidity ^0.8;
contracts/utils/LibAddress.sol:2:pragma solidity ^0.8;
contracts/utils/LibSafeCast.sol:2:pragma solidity ^0.8;
contracts/utils/EIP165.sol:2:pragma solidity ^0.8;
contracts/utils/Implementation.sol:2:pragma solidity ^0.8;
contracts/utils/Proxy.sol:2:pragma solidity ^0.8;
contracts/market-wrapper/IMarketWrapper.sol:2:pragma solidity ^0.8;
contracts/distribution/ITokenDistributorParty.sol:2:pragma solidity ^0.8;
contracts/distribution/TokenDistributor.sol:2:pragma solidity ^0.8;
contracts/distribution/ITokenDistributor.sol:2:pragma solidity ^0.8;
```
Recommend using fixed solidity version
### References:

[https://code4rena.com/reports/2022-04-phuture#g-20-use-a-more-recent-version-of-solidity](https://code4rena.com/reports/2022-04-phuture#g-20-use-a-more-recent-version-of-solidity)

----
# 4. Use safeTransfer/safeTransferFrom consistently instead of transfer/transferFrom
### Description
It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.
 ### Instances:
 // Links to github files
[PartyGovernanceNFT.sol:L143](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L143)
[Crowdfund.sol:L301](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L301)
 ### Actual Codes Used:
```
contracts/party/PartyGovernanceNFT.sol:143:        super.transferFrom(owner, to, tokenId);
contracts/crowdfund/Crowdfund.sol:301:            preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
```
### Recommended Mitigation Steps
Consider using safeTransfer/safeTransferFrom or require() consistently.

----
# 5. _safeMint() should be used rather than _mint() wherever possible
### Description:
`_mint()` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`. Both open [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/Rari-Capital/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function so that NFTs aren’t lost if they’re minted to contracts that cannot transfer them back out.
 ### Instances:
 // Links to github files
[PartyGovernanceNFT.sol:L132](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L132)
[Crowdfund.sol:L439](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L439)
[CrowdfundNFT.sol:L141](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundNFT.sol#L141)
 ### Actual Codes Used:
```
contracts/party/PartyGovernanceNFT.sol:132:        _mint(owner, tokenId);
contracts/crowdfund/Crowdfund.sol:439:                _mint(contributor);
contracts/crowdfund/CrowdfundNFT.sol:141:    function _mint(address owner) internal returns (uint256 tokenId)
```
### Recommendations:
Use _safeMint() instead of _mint().

 ----
## 6.Multiple initialization due to initialize function not having initializer modifier.
### Description
The attacker can initialize the contract, take malicious actions, and allow it to be re-initialized by the project without any error being noticed.
 ### Instances:
 // Links to github files
[ProposalExecutionEngine.sol:L99](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L99)
[IProposalExecutionEngine.sol:L18](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/IProposalExecutionEngine.sol#L18)
[Party.sol:L33](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/Party.sol#L33)
[BuyCrowdfund.sol:L68](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfund.sol#L68)
[CollectionBuyCrowdfund.sol:L83](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L83)
[AuctionCrowdfund.sol:L110](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L110)
 ### Actual Codes Used:
```
contracts/proposals/ProposalExecutionEngine.sol:99:    function initialize(address oldImpl, bytes calldata initializeData)
contracts/proposals/IProposalExecutionEngine.sol:18:    function initialize(address oldImpl, bytes memory initData) external;
contracts/party/Party.sol:33:    function initialize(PartyInitData memory initData)
contracts/crowdfund/BuyCrowdfund.sol:68:    function initialize(BuyCrowdfundOptions memory opts)
contracts/crowdfund/CollectionBuyCrowdfund.sol:83:    function initialize(CollectionBuyCrowdfundOptions memory opts)
contracts/crowdfund/AuctionCrowdfund.sol:110:    function initialize(AuctionCrowdfundOptions memory opts)
```
----
## 7. Use of Block.timestamp
Impact - Non-Critical
### Description
Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.
 ### Instances:
 // Links to github files
[ListOnZoraProposal.sol:L117](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L117)
[ListOnZoraProposal.sol:L159](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L159)
[ListOnZoraProposal.sol:L188](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L188)
[ListOnOpenseaProposal.sol:L166](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L166)
[ListOnOpenseaProposal.sol:L210](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L210)
[ListOnOpenseaProposal.sol:L263](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L263)
[ListOnOpenseaProposal.sol:L364](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L364)
[PartyGovernance.sol:L539](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L539)
[PartyGovernance.sol:L605](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L605)
[PartyGovernance.sol:L681](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L681)
[PartyGovernance.sol:L684](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L684)
[PartyGovernance.sol:L687](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L687)
[PartyGovernance.sol:L695](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L695)
[PartyGovernance.sol:L753](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L753)
[PartyGovernance.sol:L755](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L755)
[PartyGovernance.sol:L762](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L762)
[PartyGovernance.sol:L903](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L903)
[PartyGovernance.sol:L940](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L940)
[PartyGovernance.sol:L963](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L963)
[PartyGovernance.sol:L1036](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L1036)
[AuctionCrowdfund.sol:L118](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L118)
[AuctionCrowdfund.sol:L276](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L276)
[BuyCrowdfundBase.sol:L73](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfundBase.sol#L73)
[BuyCrowdfundBase.sol:L159](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfundBase.sol#L159)
 ### Actual Codes Used:
```
contracts/proposals/ListOnZoraProposal.sol:117:                minExpiry: (block.timestamp + data.timeout).safeCastUint256ToUint40()
contracts/proposals/ListOnZoraProposal.sol:159:            uint40(block.timestamp + timeout)
contracts/proposals/ListOnZoraProposal.sol:188:                if (minExpiry > uint40(block.timestamp)) {
contracts/proposals/ListOnOpenseaProposal.sol:166:                        minExpiry: (block.timestamp + zoraTimeout).safeCastUint256ToUint40()
contracts/proposals/ListOnOpenseaProposal.sol:210:            uint256 expiry = block.timestamp + uint256(data.duration);
contracts/proposals/ListOnOpenseaProposal.sol:263:        orderParams.startTime = block.timestamp;
contracts/proposals/ListOnOpenseaProposal.sol:364:        } else if (expiry <= block.timestamp) {
contracts/party/PartyGovernance.sol:539:                proposedTime: uint40(block.timestamp),
contracts/party/PartyGovernance.sol:605:            info.values.passedTime = uint40(block.timestamp);
contracts/party/PartyGovernance.sol:681:            if (proposal.maxExecutableTime < block.timestamp) {
contracts/party/PartyGovernance.sol:684:                    uint40(block.timestamp)
contracts/party/PartyGovernance.sol:687:            proposalState.values.executedTime = uint40(block.timestamp);
contracts/party/PartyGovernance.sol:695:        proposalState.values.completedTime = uint40(block.timestamp);
contracts/party/PartyGovernance.sol:753:            if (block.timestamp < cancelTime) {
contracts/party/PartyGovernance.sol:755:                    uint40(block.timestamp),
contracts/party/PartyGovernance.sol:762:        proposalState.values.completedTime = uint40(block.timestamp | UINT40_HIGH_BIT);
contracts/party/PartyGovernance.sol:903:            timestamp: uint40(block.timestamp),
contracts/party/PartyGovernance.sol:940:                    timestamp: uint40(block.timestamp),
contracts/party/PartyGovernance.sol:963:                    timestamp: uint40(block.timestamp),
contracts/party/PartyGovernance.sol:1036:        uint40 t = uint40(block.timestamp);
contracts/crowdfund/AuctionCrowdfund.sol:118:        expiry = uint40(opts.duration + block.timestamp);
contracts/crowdfund/AuctionCrowdfund.sol:276:        if (block.timestamp >= expiry) {
contracts/crowdfund/BuyCrowdfundBase.sol:73:        expiry = uint40(opts.duration + block.timestamp);
contracts/crowdfund/BuyCrowdfundBase.sol:159:        if (block.timestamp >= expiry) {
```
### Recommended Mitigation Steps

Block timestamps should not be used for entropy or generating random numbers—i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state.

Time-sensitive logic is sometimes required; e.g., for unlocking contracts (time-locking), completing an ICO after a few weeks, or enforcing expiry dates. It is sometimes recommended to use block.number and an average block time to estimate times; with a 10 second block time, 1 week equates to approximately, 60480 blocks. Thus, specifying a block number at which to change a contract state can be more secure, as miners are unable to easily manipulate the block number.

----