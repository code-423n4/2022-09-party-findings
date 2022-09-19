# QA


## ✅ L-1: `require()` should be used instead of `assert()`

### 📝 Description

Prior to solidity version 0.8.0, hitting an assert consumes the remainder of the transaction's available gas rather than returning it, as `require()`/`revert()` do. `assert()` should be avoided even past solidity version 0.8.0 as its [documentation](https://docs.soliditylang.org/en/v0.8.14/control-structures.html#panic-via-assert-and-error-via-require) states that The assert function creates an error of type Panic(uint256). ... Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix

### 💡 Recommendation

You should change from `assert()` to `require()`

### 🔍 Findings:

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L385` [assert(tokenType == TokenType.Erc20);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L385)

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L411` [assert(tokenType == TokenType.Erc20);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L411)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L504` [assert(tokenType == ITokenDistributor.TokenType.Erc20);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L504)

`party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L179` [assert(false); // Will not be reached.](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L179)

`party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L67` [assert(vault.balanceOf(address(this)) == supply);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L67)

`party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L221` [assert(step == ListOnOpenseaStep.ListedOnOpenSea);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L221)

`party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L302` [assert(SEAPORT.validate(orders));](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L302)

`party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L120` [assert(step == ZoraStep.ListedOnZora);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L120)

`party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L142` [assert(currentInProgressProposalId == 0);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L142)

`party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L30` [assert(false);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L30)

## ✅ N-1: Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant`

### 📝 Description

Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant`

### 💡 Recommendation

You should use immutable instead of constant

### 🔍 Findings:

`party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L80` [uint256 private constant \_STORAGE_SLOT = uint256(keccak256('ProposalExecutionEngine.Storage'));](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L80)


`party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L19` [uint256 private constant SHARED_STORAGE_SLOT = uint256(keccak256("ProposalStorage.SharedProposalStorage"));](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L19)

## ✅ N-2: Non-library/interface files should use fixed compiler versions, not floating ones

### 📝 Description

Non-library/interface files should use fixed compiler versions, not floating ones

### 💡 Recommendation

Delete the floating keyword `^`.

### 🔍 Findings:

`party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L2)

`party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L2)

`party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L2)

`party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L2)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L2)

`party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L2)

`party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#L2)

`party-contracts-c4/blob/main/contracts/distribution/ITokenDistributorParty.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributorParty.sol#L2)

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L2)

`party-contracts-c4/blob/main/contracts/gatekeepers/IGateKeeper.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/gatekeepers/IGateKeeper.sol#L2)

`party-contracts-c4/blob/main/contracts/globals/Globals.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L2)

`party-contracts-c4/blob/main/contracts/globals/IGlobals.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/IGlobals.sol#L2)

`party-contracts-c4/blob/main/contracts/party/IPartyFactory.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/IPartyFactory.sol#L2)

`party-contracts-c4/blob/main/contracts/party/Party.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L2)

`party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L2)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L2)

`party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L2)

`party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L2)

`party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L2)

`party-contracts-c4/blob/main/contracts/proposals/IProposalExecutionEngine.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/IProposalExecutionEngine.sol#L2)


`party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L2)

`party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L2)

`party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L2)

`party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L2)

`party-contracts-c4/blob/main/contracts/proposals/vendor/FractionalV1.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/FractionalV1.sol#L2)

`party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaConduitController.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaConduitController.sol#L2)

`party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaExchange.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaExchange.sol#L2)

`party-contracts-c4/blob/main/contracts/renderers/DummyERC721Renderer.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/DummyERC721Renderer.sol#L2)

`party-contracts-c4/blob/main/contracts/renderers/IERC721Renderer.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/IERC721Renderer.sol#L2)

`party-contracts-c4/blob/main/contracts/tokens/ERC1155Receiver.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC1155Receiver.sol#L2)

`party-contracts-c4/blob/main/contracts/tokens/ERC721Receiver.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC721Receiver.sol#L2)

`party-contracts-c4/blob/main/contracts/tokens/IERC1155.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC1155.sol#L2)

`party-contracts-c4/blob/main/contracts/tokens/IERC20.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC20.sol#L2)

`party-contracts-c4/blob/main/contracts/tokens/IERC721.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721.sol#L2)

`party-contracts-c4/blob/main/contracts/tokens/IERC721Receiver.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721Receiver.sol#L2)

`party-contracts-c4/blob/main/contracts/utils/EIP165.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/EIP165.sol#L2)

`party-contracts-c4/blob/main/contracts/utils/Implementation.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Implementation.sol#L2)

`party-contracts-c4/blob/main/contracts/utils/Proxy.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol#L2)

`party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L2` [pragma solidity ^0.8;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L2)

## ✅ N-3: Variable names that consist of all capital letters should be reserved for `constant/immutable` variables

### 📝 Description

Variable names that consist of all capital letters should be reserved for `constant/immutable` variables

### 💡 Recommendation

Variables that are not constant/immutable should be declared in lower case

### 🔍 Findings:

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L189` [uint256 constant private UINT40_HIGH_BIT = 1 << 39;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L189)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L190` [uint96 constant private VETO_VALUE = uint96(int96(-1));](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L190)

## ✅ N-4: Open Todos

### 📝 Description

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

### 💡 Recommendation

Delete TODO keyword

### 🔍 Findings:

`party-contracts-c4/blob/main/contracts/renderers/DummyERC721Renderer.sol#L8` [// TODO: make this human readable](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/DummyERC721Renderer.sol#L8)