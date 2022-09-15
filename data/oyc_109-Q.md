## [L-01] Unspecific Compiler Version Pragma

Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

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
party-contracts-c4/contracts/proposals/LibProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalStorage.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/FractionalV1.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/IOpenseaConduitController.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/vendor/IOpenseaExchange.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/renderers/DummyERC721Renderer.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/ERC1155Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/ERC721Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC1155.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC20.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC721.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/tokens/IERC721Receiver.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/EIP165.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/Implementation.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibAddress.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibERC20Compat.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibRawResult.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibSafeCast.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/LibSafeERC721.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/Proxy.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::2 => pragma solidity ^0.8;
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol::2 => pragma solidity ^0.8;
```

## [L-02] Use of Block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::118 => expiry = uint40(opts.duration + block.timestamp);
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::276 => if (block.timestamp >= expiry) {
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::73 => expiry = uint40(opts.duration + block.timestamp);
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::159 => if (block.timestamp >= expiry) {
party-contracts-c4/contracts/party/PartyGovernance.sol::539 => proposedTime: uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol::605 => info.values.passedTime = uint40(block.timestamp);
party-contracts-c4/contracts/party/PartyGovernance.sol::681 => if (proposal.maxExecutableTime < block.timestamp) {
party-contracts-c4/contracts/party/PartyGovernance.sol::684 => uint40(block.timestamp)
party-contracts-c4/contracts/party/PartyGovernance.sol::687 => proposalState.values.executedTime = uint40(block.timestamp);
party-contracts-c4/contracts/party/PartyGovernance.sol::695 => proposalState.values.completedTime = uint40(block.timestamp);
party-contracts-c4/contracts/party/PartyGovernance.sol::753 => if (block.timestamp < cancelTime) {
party-contracts-c4/contracts/party/PartyGovernance.sol::755 => uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol::762 => proposalState.values.completedTime = uint40(block.timestamp | UINT40_HIGH_BIT);
party-contracts-c4/contracts/party/PartyGovernance.sol::903 => timestamp: uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol::940 => timestamp: uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol::963 => timestamp: uint40(block.timestamp),
party-contracts-c4/contracts/party/PartyGovernance.sol::1036 => uint40 t = uint40(block.timestamp);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::166 => minExpiry: (block.timestamp + zoraTimeout).safeCastUint256ToUint40()
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::210 => uint256 expiry = block.timestamp + uint256(data.duration);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::263 => orderParams.startTime = block.timestamp;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::364 => } else if (expiry <= block.timestamp) {
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::117 => minExpiry: (block.timestamp + data.timeout).safeCastUint256ToUint40()
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::159 => uint40(block.timestamp + timeout)
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::188 => if (minExpiry > uint40(block.timestamp)) {
```

## [L-03] Unused receive()/fallback() function

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert

```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::144 => receive() external payable {}
party-contracts-c4/contracts/party/Party.sol::47 => receive() external payable {}
```

## [L-04] Open TODOs

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

```
party-contracts-c4/contracts/renderers/DummyERC721Renderer.sol::8 => // TODO: make this human readable
```

## [L-05] require() should be used instead of assert()

require() should be used for checking error conditions on inputs and return values while assert() should be used for invariant checking

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

## [L-06] approve should be replaced with safeApprove or safeIncreaseAllowance() / safeDecreaseAllowance()

approve is subject to a known front-running attack. Consider using safeApprove() or safeIncreaseAllowance() or safeDecreaseAllowance() instead

```
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol::53 => data.token.approve(address(VAULT_FACTORY), data.tokenId);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::256 => token.approve(conduit, tokenId);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::367 => token.approve(address(0), tokenId);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::143 => token.approve(address(ZORA), tokenId);
```

## [L-07] Unsafe use of transfer()/transferFrom() with IERC20

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead

```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::301 => preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::143 => super.transferFrom(owner, to, tokenId);
```

## [L-08] Events not emitted for important state changes

When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

```
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::77 => function setApprovalForAll(address, bool)
party-contracts-c4/contracts/globals/Globals.sol::67 => function setAddress(uint256 key, address value) external onlyMultisig {
party-contracts-c4/contracts/globals/Globals.sol::79 => function setIncludesAddress(uint256 key, address value, bool isIncluded) external onlyMultisig {
party-contracts-c4/contracts/globals/IGlobals.sol::19 => function setAddress(uint256 key, address value) external;
party-contracts-c4/contracts/globals/IGlobals.sol::22 => function setIncludesAddress(uint256 key, address value, bool isIncluded) external;
party-contracts-c4/contracts/tokens/IERC1155.sol::22 => function setApprovalForAll(address operator, bool approved) external;
party-contracts-c4/contracts/tokens/IERC721.sol::14 => function setApprovalForAll(address operator, bool isApproved) external;
```

## [L-09] _safeMint() should be used rather than _mint() wherever possible

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead

```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::439 => _mint(contributor);
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::141 => function _mint(address owner) internal returns (uint256 tokenId)
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::132 => _mint(owner, tokenId);
```

## [N-1] require()/revert() statements should have descriptive reason strings

Descriptive reason strings should be used so that users can troubleshot any reverted calls

```
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::246 => require(proposalType != ProposalType.Invalid);
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::247 => require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::20 => require(msg.sender == address(this));
```

## [N-2] Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::73 => event Bid(uint256 bidAmount);
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::74 => event Won(uint256 bid, Party party);
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::75 => event Lost();
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::52 => event Won(Party party, IERC721 token, uint256 tokenId, uint256 settledPrice);
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::82 => event Burned(address contributor, uint256 ethUsed, uint256 ethOwed, uint256 votingPower);
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::83 => event Contributed(address contributor, uint256 amount, address delegate, uint256 previousTotalContributions);
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::16 => event BuyCrowdfundCreated(BuyCrowdfund crowdfund, BuyCrowdfund.BuyCrowdfundOptions opts);
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::17 => event AuctionCrowdfundCreated(AuctionCrowdfund crowdfund, AuctionCrowdfund.AuctionCrowdfundOptions opts);
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::18 => event CollectionBuyCrowdfundCreated(CollectionBuyCrowdfund crowdfund, CollectionBuyCrowdfund.CollectionBuyCrowdfundOptions opts);
party-contracts-c4/contracts/party/IPartyFactory.sol::11 => event PartyCreated(Party party, address creator);
party-contracts-c4/contracts/party/PartyGovernance.sol::162 => event PartyInitialized(GovernanceOpts opts, IERC721[] preciousTokens, uint256[] preciousTokenIds);
party-contracts-c4/contracts/party/PartyGovernance.sol::167 => event DistributionCreated(ITokenDistributor.TokenType tokenType, address token, uint256 tokenId);
party-contracts-c4/contracts/party/PartyGovernance.sol::169 => event HostStatusTransferred(address oldHost, address newHost);
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::35 => event ArbitraryCallExecuted(uint256 proposalId, uint256 idx, uint256 count);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::53 => event ZoraAuctionExpired(uint256 auctionId, uint256 expiry);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::54 => event ZoraAuctionSold(uint256 auctionId);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::55 => event ZoraAuctionFailed(uint256 auctionId);
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::66 => event ProposalEngineImplementationUpgraded(address oldImpl, address newImpl);
```

## [N-3] Missing NatSpec

Code should include NatSpec

```
party-contracts-c4/contracts/globals/IGlobals.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/proposals/LibProposal.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/proposals/ProposalStorage.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/proposals/vendor/IOpenseaConduitController.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/proposals/vendor/IOpenseaExchange.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/renderers/DummyERC721Renderer.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/tokens/IERC1155.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/tokens/IERC20.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/tokens/IERC721.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/tokens/IERC721Receiver.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/utils/Implementation.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/utils/LibAddress.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/utils/LibERC20Compat.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/utils/LibRawResult.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/utils/LibSafeCast.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/utils/LibSafeERC721.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::1 => // SPDX-License-Identifier: Beta Software
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol::1 => // SPDX-License-Identifier: Beta Software
```

## [N-4] Public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents' functions and change the visibility from external to public.

```
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::88 => function tokenURI(uint256) public override view returns (string memory) {
```

## [N-5] Adding a return statement when the function defines a named return variable, is redundant

It is not necessary to have both a named return and a return statement.

```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::287 => returns (uint256 price)
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::113 => returns (bytes12 newGateKeeperId)
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::133 => function balanceOf(address owner) external view returns (uint256 numTokens) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::406 => returns (bytes32 balanceId)
party-contracts-c4/contracts/party/PartyGovernance.sol::350 => returns (uint96 votingPower)
party-contracts-c4/contracts/party/PartyGovernance.sol::364 => returns (uint96 votingPower)
party-contracts-c4/contracts/party/PartyGovernance.sol::425 => returns (uint256 index)
party-contracts-c4/contracts/party/PartyGovernance.sol::565 => returns (uint256 totalVotes)
party-contracts-c4/contracts/party/PartyGovernance.sol::814 => returns (bool completed)
party-contracts-c4/contracts/party/PartyGovernance.sol::1015 => returns (ProposalStatus status)
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::70 => returns (address owner)
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::104 => returns (address, uint256)
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::150 => returns (bool isAllowed)
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::172 => returns (bool sold)
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::110 => returns (uint256 id)
party-contracts-c4/contracts/proposals/ProposalStorage.sol::24 => returns (IProposalExecutionEngine impl)
```

## [N-6] Missing event for critical parameter change

Emitting events after sensitive changes take place, to facilitate tracking and notify off-chain clients following changes to the contract.

```
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::77 => function setApprovalForAll(address, bool)
party-contracts-c4/contracts/globals/Globals.sol::67 => function setAddress(uint256 key, address value) external onlyMultisig {
party-contracts-c4/contracts/globals/Globals.sol::79 => function setIncludesAddress(uint256 key, address value, bool isIncluded) external onlyMultisig {
party-contracts-c4/contracts/globals/IGlobals.sol::19 => function setAddress(uint256 key, address value) external;
party-contracts-c4/contracts/globals/IGlobals.sol::22 => function setIncludesAddress(uint256 key, address value, bool isIncluded) external;
party-contracts-c4/contracts/tokens/IERC1155.sol::22 => function setApprovalForAll(address operator, bool approved) external;
party-contracts-c4/contracts/tokens/IERC721.sol::14 => function setApprovalForAll(address operator, bool isApproved) external;
```

## [N-7] Expressions for constant values such as a call to keccak256(), should use immutable rather than constant

instances:

```
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::80 => uint256 private constant _STORAGE_SLOT = uint256(keccak256('ProposalExecutionEngine.Storage'));
party-contracts-c4/contracts/proposals/ProposalStorage.sol::19 => uint256 private constant SHARED_STORAGE_SLOT = uint256(keccak256("ProposalStorage.SharedProposalStorage"));
```
