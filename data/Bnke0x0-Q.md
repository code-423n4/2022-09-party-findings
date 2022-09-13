

### [L01] require() should be used instead of assert()


#### Findings:
```
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::166 => assert(false); // Will not be reached.
party-contracts-c4/contracts/distribution/TokenDistributor.sol::385 => assert(tokenType == TokenType.Erc20);
party-contracts-c4/contracts/distribution/TokenDistributor.sol::411 => assert(tokenType == TokenType.Erc20);
party-contracts-c4/contracts/party/PartyGovernance.sol::504 => assert(tokenType == ITokenDistributor.TokenType.Erc20);
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::179 => assert(false); // Will not be reached.
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol::67 => assert(vault.balanceOf(address(this)) == supply);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::221 => assert(step == ListOnOpenseaStep.ListedOnOpenSea);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::302 => assert(SEAPORT.validate(orders));
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::120 => assert(step == ZoraStep.ListedOnZora);
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::142 => assert(currentInProgressProposalId == 0);
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::30 => assert(false);
```





### [L02] Missing checks for `address(0x0)` when assigning values to `address` state variables


#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::159 => _bidStatus = AuctionCrowdfundStatus.Busy;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::161 => uint256 auctionId_ = auctionId;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::187 => _bidStatus = AuctionCrowdfundStatus.Active;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::208 => _bidStatus = AuctionCrowdfundStatus.Busy;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::236 => lastBid_ = totalContributions;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::241 => lastBid = lastBid_;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::258 => _bidStatus = AuctionCrowdfundStatus.Finalized;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::264 => AuctionCrowdfundStatus status = _bidStatus;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::104 => IPartyFactory partyFactory = _getPartyFactory();
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::122 => settledPrice_ = callValue == 0 ? totalContributions : callValue;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::119 => _GLOBALS = globals;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::138 => governanceOptsHash = _hashFixedGovernanceOpts(opts.governanceOpts);
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::26 => _GLOBALS = globals;
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::35 => _GLOBALS = globals;
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::43 => name = name_;
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::44 => symbol = symbol_;;
party-contracts-c4/contracts/globals/Globals.sol::24 => multiSig = multiSig_;
party-contracts-c4/contracts/party/PartyGovernance.sol::267 => _GLOBALS = globals;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::45 => _GLOBALS = globals;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::60 => name = name_;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::61 => symbol = symbol_;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::62 => mintAuthority = mintAuthority_;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::72 => _GLOBALS = globals;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::94 => _GLOBALS = globals;
```



### [L03] `initialize` functions can be front-run



#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::110 => function initialize(AuctionCrowdfundOptions memory opts)
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::68 => function initialize(BuyCrowdfundOptions memory opts)
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::70 => function _initialize(BuyCrowdfundBaseOptions memory opts)
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::83 => function initialize(CollectionBuyCrowdfundOptions memory opts)
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::124 => function _initialize(CrowdfundOptions memory opts)
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::39 => function _initialize(string memory name_, string memory symbol_)
party-contracts-c4/contracts/party/Party.sol::33 => function initialize(PartyInitData memory initData)
party-contracts-c4/contracts/party/PartyGovernance.sol::271 => function _initialize(
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::49 => function _initialize(
party-contracts-c4/contracts/proposals/IProposalExecutionEngine.sol::18 => function initialize(address oldImpl, bytes memory initData) external;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::99 => function initialize(address oldImpl, bytes calldata initializeData)
```




### [L04] Unused `receive()` function will lock Ether in contract

#### Impact
If the intention is for the Ether to be used, the function should call another function, otherwise it should revert
#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::144 => receive() external payable {}
party-contracts-c4/contracts/party/Party.sol::47 => receive() external payable {}
```






### [L05] approve should be replaced with safeApprove or safeIncreaseAllowance() / safeDecreaseAllowance()

#### Impact
approve is subject to a known front-running attack. Consider using safeApprove instead
#### Findings:
```
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol::53 => data.token.approve(address(VAULT_FACTORY), data.tokenId);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::256 => token.approve(conduit, tokenId);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::367 => token.approve(address(0), tokenId);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::143 => token.approve(address(ZORA), tokenId);
```



### [L06] `_safeMint()` should be used rather than `_mint()` wherever possible

#### Impact
_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both OpenZeppelin and solmate have versions of this function
#### Findings:
```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::439 => _mint(contributor);
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::132 => _mint(owner, tokenId);
```








### [L07] Unspecific Compiler Version Pragma

#### Impact
Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.
#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/ITokenDistributorParty.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/gatekeepers/IGateKeeper.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/globals/Globals.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/globals/IGlobals.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/market-wrapper/IMarketWrapper.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/IPartyFactory.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/Party.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyFactory.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyGovernance.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/IProposalExecutionEngine.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalStorage.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/FractionalV1.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/IOpenseaConduitController.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/IOpenseaExchange.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/EIP165.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/ERC1155Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/ERC721Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC1155.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC20.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC721.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC721Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/Implementation.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/Proxy.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol::2 => pragma solidity ^0.8;
```



### Non-Critical Issues



### [N01] `require()`/`revert()` statements should have descriptive reason strings


#### Findings:
```
party-contracts-c4/contracts/proposals/ProposalExecutionE29ngine.sol::246 => require(proposalType != ProposalType.Invalid);
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::247 => require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));
party-contracts-c4/contracts/utils/Proxy.sol::32 => revert(0x00, returndatasize())
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::20 => require(msg.sender == address(this));
```



### [N02] constants should be defined rather than using magic numbers


#### Findings:
```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::370 => votingPower = ((1e4 - splitBps_) * ethUsed) / 1e4;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::374 => votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up
party-contracts-c4/contracts/distribution/TokenDistributor.sol::261 => + (1e18 - 1)
party-contracts-c4/contracts/distribution/TokenDistributor.sol::263 => / 1e18
party-contracts-c4/contracts/distribution/TokenDistributor.sol::352 => uint128 fee = supply * args.feeBps / 1e4;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::398 => 0xffffffffffffffffffffffffffffff0000000000000000000000000000000000
party-contracts-c4/contracts/party/PartyGovernance.sol::1062 => uint256 acceptanceRatio = (totalVotes * 1e4) / totalVotingPower;
party-contracts-c4/contracts/party/PartyGovernance.sol::1078 => return uint256(voteCount) * 1e4
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::112 => return votingPowerByTokenId[tokenId] * 1e18 / _getTotalVotingPower();
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::164 => 0xffffffff00000000000000000000000000000000000000000000000000000000
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::62 => 0x474ba0184a7cd5de777156a56f3859150719340a6974b6ee50f05c58139f4dc2;
```



### [N03] Use a more recent version of solidity
Use a solidity version of at least 0.8.12 to get string.concat() to be used instead of abi.encodePacked(<str>,<str>)
#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/ITokenDistributorParty.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/gatekeepers/IGateKeeper.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/globals/Globals.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/globals/IGlobals.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/market-wrapper/IMarketWrapper.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/IPartyFactory.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/Party.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyFactory.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyGovernance.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/IProposalExecutionEngine.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalStorage.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/FractionalV1.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/IOpenseaConduitController.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/IOpenseaExchange.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/EIP165.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/ERC1155Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/ERC721Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC1155.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC20.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC721.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC721Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/Implementation.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/Proxy.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol::2 => pragma solidity ^0.8;
```

### [N04] Use a more recent version of solidity

#### Impact
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions
#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::18 => using LibSafeERC721 for IERC721;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::19 => using LibSafeCast for uint256;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::20 => using LibRawResult for bytes;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::16 => using LibSafeERC721 for IERC721;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::17 => using LibSafeCast for uint256;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::16 => using LibSafeERC721 for IERC721;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::17 => using LibSafeCast for uint256;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::18 => using LibRawResult for bytes;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::17 => using LibSafeERC721 for IERC721;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::18 => using LibSafeCast for uint256;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::18 => using LibRawResult for bytes;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::19 => using LibSafeCast for uint256;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::20 => using LibAddress for address payable;
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::14 => using LibRawResult for bytes;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::17 => using LibAddress for address payable;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::18 => using LibERC20Compat for IERC20;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::19 => using LibRawResult for bytes;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::20 => using LibSafeCast for uint256;
party-contracts-c4/contracts/party/PartyGovernance.sol::32 => using LibERC20Compat for IERC20;
party-contracts-c4/contracts/party/PartyGovernance.sol::33 => using LibRawResult for bytes;
party-contracts-c4/contracts/party/PartyGovernance.sol::34 => using LibSafeCast for uint256;
party-contracts-c4/contracts/party/PartyGovernance.sol::35 => using LibSafeCast for int192;
party-contracts-c4/contracts/party/PartyGovernance.sol::36 => using LibSafeCast for uint96;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::18 => using LibSafeCast for uint256;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::14 => using LibSafeERC721 for IERC721;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::18 => using LibSafeCast for uint256;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::18 => using LibRawResult for bytes;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::19 => using LibSafeERC721 for IERC721;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::20 => using LibSafeCast for uint256;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::27 => using LibRawResult for bytes;
party-contracts-c4/contracts/proposals/ProposalStorage.sol::12 => using LibRawResult for bytes;
party-contracts-c4/contracts/utils/Proxy.sol::9 => using LibRawResult for bytes;
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::15 => using LibRawResult for bytes;
```



### [N05] Event is missing `indexed` fields

#### Impact
Each event should use three indexed fields if there are three or more fields
#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::74 => event Won(uint256 bid, Party party);
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::52 => event Won(Party party, IERC721 token, uint256 tokenId, uint256 settledPrice);
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::82 => event Burned(address contributor, uint256 ethUsed, uint256 ethOwed, uint256 votingPower);
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::83 => event Contributed(address contributor, uint256 amount, address delegate, uint256 previousTotalContributions);
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::16 => event BuyCrowdfundCreated(BuyCrowdfund crowdfund, BuyCrowdfund.BuyCrowdfundOptions opts);
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::17 => event AuctionCrowdfundCreated(AuctionCrowdfund crowdfund, AuctionCrowdfund.AuctionCrowdfundOptions opts);
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::18 => event CollectionBuyCrowdfundCreated(CollectionBuyCrowdfund crowdfund, CollectionBuyCrowdfund.CollectionBuyCrowdfundOptions opts);
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::33 => event DistributionCreated(
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::37 => event DistributionFeeClaimed(
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::44 => event DistributionClaimedByPartyToken(
party-contracts-c4/contracts/party/IPartyFactory.sol::11 => event PartyCreated(Party party, address creator);
party-contracts-c4/contracts/party/PartyGovernance.sol::151 => event Proposed(
party-contracts-c4/contracts/party/PartyGovernance.sol::156 => event ProposalAccepted(
party-contracts-c4/contracts/party/PartyGovernance.sol::162 => event PartyInitialized(GovernanceOpts opts, IERC721[] preciousTokens, uint256[] preciousTokenIds);
party-contracts-c4/contracts/party/PartyGovernance.sol::164 => event ProposalVetoed(uint256 indexed proposalId, address host);
party-contracts-c4/contracts/party/PartyGovernance.sol::165 => event ProposalExecuted(uint256 indexed proposalId, address executor, bytes nextProgressData);
party-contracts-c4/contracts/party/PartyGovernance.sol::167 => event DistributionCreated(ITokenDistributor.TokenType tokenType, address token, uint256 tokenId);
party-contracts-c4/contracts/party/PartyGovernance.sol::168 => event VotingPowerDelegated(address indexed owner, address indexed delegate);
party-contracts-c4/contracts/party/PartyGovernance.sol::169 => event HostStatusTransferred(address oldHost, address newHost);
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::35 => event ArbitraryCallExecuted(uint256 proposalId, uint256 idx, uint256 count);
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol::22 => event FractionalV1VaultCreated(
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::63 => event OpenseaOrderListed(
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::71 => event OpenseaOrderSold(
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::77 => event OpenseaOrderExpired(
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::84 => event OrderValidated(
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::45 => event ZoraAuctionCreated(
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::53 => event ZoraAuctionExpired(uint256 auctionId, uint256 expiry);
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::66 => event ProposalEngineImplementationUpgraded(address oldImpl, address newImpl);
party-contracts-c4/contracts/tokens/IERC1155.sol::6 => event TransferSingle(
party-contracts-c4/contracts/tokens/IERC1155.sol::13 => event TransferBatch(
party-contracts-c4/contracts/tokens/IERC1155.sol::20 => event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
party-contracts-c4/contracts/tokens/IERC20.sol::6 => event Transfer(address indexed owner, address indexed to, uint256 amount);
party-contracts-c4/contracts/tokens/IERC20.sol::7 => event Approval(address indexed owner, address indexed spender, uint256 allowance);
party-contracts-c4/contracts/tokens/IERC721.sol::8 => event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
```



### [N06] Use of sensitive/NC-inclusive terms


#### Findings:
```
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::61 => bool isHost;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::28 => bool wasFeeClaimed;
party-contracts-c4/contracts/party/PartyGovernance.sol::108 => bool isDelegated;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::98 => bool isUnanimous,
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::144 => bool isUnanimous,
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol::15 => bool approved;
```






### [N07] `public` functions not called by the contract should be declared `external` instead


#### Findings:
```

party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::262 => function getCrowdfundLifecycle() public override view returns (CrowdfundLifecycle) {
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::150 => function getCrowdfundLifecycle() public override view returns (CrowdfundLifecycle) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::251 => function getCrowdfundLifecycle() public virtual view returns (CrowdfundLifecycle);
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::88 => function tokenURI(uint256) public override view returns (string memory) {
```



### [N08] Numeric values having to do with time should use time units for readability

#### Impact
There are units for seconds, minutes, hours, days, and weeks
#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::47 => // How long this crowdfund has to bid on the NFT, in seconds.
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::29 => // How long this crowdfund has to bid on the NFT, in seconds.
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::26 => // How long this crowdfund has to bid on the NFT, in seconds.
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::30 => // How long this crowdfund has to bid on the NFT, in seconds.
party-contracts-c4/contracts/party/PartyGovernance.sol::67 => // `cancelDelay` seconds passed since the first execute and was forcibly cancelled.
party-contracts-c4/contracts/party/PartyGovernance.sol::117 => // The minimum seconds this proposal can remain in the InProgress status
party-contracts-c4/contracts/party/PartyGovernance.sol::713 => /// @dev proposal.cancelDelay seconds must have passed since it was first
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::77 => // Calling this the second time will either cancel or finalize the auction.
```



### [N09] Constant redefined elsewhere

#### Impact
Consider defining in only one contract so that values cannot become out of sync when only one location is updated
#### Findings:
```

party-contracts-c4/contracts/party/PartyGovernance.sol::405 => bytes32 dataHash = keccak256(proposal.proposalData);
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::121 => bytes32 resultHash = keccak256(r);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::184 => bytes32 errHash = keccak256(errData);
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::150 => bytes32 progressDataHash = keccak256(params.progressData);
```



### [N10] NC-library/interface files should use fixed compiler versions, not floating ones


#### Findings:
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/ITokenDistributorParty.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/gatekeepers/IGateKeeper.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/globals/Globals.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/globals/IGlobals.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/market-wrapper/IMarketWrapper.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/IPartyFactory.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/Party.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyFactory.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyGovernance.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/IProposalExecutionEngine.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalStorage.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/FractionalV1.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/IOpenseaConduitController.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/IOpenseaExchange.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/EIP165.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/ERC1155Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/ERC721Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC1155.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC20.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC721.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC721Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/Implementation.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/Proxy.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol::2 => pragma solidity ^0.8;
```







### [N11] Interfaces should be moved to separate files

#### Impact
Most of the other interfaces in this project are in their own file in
 the interfaces directory. The interfaces below do not follow this 
pattern
#### Findings:
```

party-contracts-c4/contracts/distribution/ITokenDistributor.sol::9 => interface ITokenDistributor {
party-contracts-c4/contracts/distribution/ITokenDistributorParty.sol::5 => interface ITokenDistributorParty {
party-contracts-c4/contracts/gatekeepers/IGateKeeper.sol::5 => interface IGateKeeper {
party-contracts-c4/contracts/globals/IGlobals.sol::8 => interface IGlobals {
party-contracts-c4/contracts/market-wrapper/IMarketWrapper.sol::19 => interface IMarketWrapper {
party-contracts-c4/contracts/party/IPartyFactory.sol::10 => interface IPartyFactory {
party-contracts-c4/contracts/proposals/IProposalExecutionEngine.sol::7 => interface IProposalExecutionEngine {
party-contracts-c4/contracts/proposals/vendor/FractionalV1.sol::9 => interface IFractionalV1VaultFactory {
party-contracts-c4/contracts/proposals/vendor/FractionalV1.sol::28 => interface IFractionalV1Vault is IERC20 {
party-contracts-c4/contracts/proposals/vendor/IOpenseaConduitController.sol::4 => interface IOpenseaConduitController {
party-contracts-c4/contracts/proposals/vendor/IOpenseaExchange.sol::4 => interface IOpenseaExchange {
party-contracts-c4/contracts/tokens/IERC1155.sol::5 => interface IERC1155 {
party-contracts-c4/contracts/tokens/IERC20.sol::5 => interface IERC20 {
party-contracts-c4/contracts/tokens/IERC721.sol::5 => interface IERC721 {
party-contracts-c4/contracts/tokens/IERC721Receiver.sol::4 => interface IERC721Receiver {
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::6 => interface IReadOnlyDelegateCall {
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol::8 => interface IZoraAuctionHouse {
```





