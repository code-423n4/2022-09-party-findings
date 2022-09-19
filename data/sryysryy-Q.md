### Unspecific Compiler Version Pragma
In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
*All contract inscope (non library / interface) are affected*
```solidity
contracts/crowdfund/AuctionCrowdfund.sol::2 => pragma solidity ^0.8;
contracts/crowdfund/BuyCrowdfund.sol::2 => pragma solidity ^0.8;
contracts/crowdfund/BuyCrowdfundBase.sol::2 => pragma solidity ^0.8;
contracts/crowdfund/CollectionBuyCrowdfund.sol::2 => pragma solidity ^0.8;
contracts/crowdfund/Crowdfund.sol::2 => pragma solidity ^0.8;
contracts/crowdfund/CrowdfundFactory.sol::2 => pragma solidity ^0.8;
contracts/crowdfund/CrowdfundNFT.sol::2 => pragma solidity ^0.8;
contracts/distribution/ITokenDistributor.sol::2 => pragma solidity ^0.8;
contracts/distribution/ITokenDistributorParty.sol::2 => pragma solidity ^0.8;
contracts/distribution/TokenDistributor.sol::2 => pragma solidity ^0.8
contracts/globals/Globals.sol::2 => pragma solidity ^0.8;
contracts/globals/IGlobals.sol::2 => pragma solidity ^0.8;
contracts/party/IPartyFactory.sol::2 => pragma solidity ^0.8;
contracts/party/Party.sol::2 => pragma solidity ^0.8;
contracts/party/PartyFactory.sol::2 => pragma solidity ^0.8;
contracts/party/PartyGovernance.sol::2 => pragma solidity ^0.8;
contracts/party/PartyGovernanceNFT.sol::2 => pragma solidity ^0.8;
contracts/proposals/ArbitraryCallsProposal.sol::2 => pragma solidity ^0.8;
contracts/proposals/FractionalizeProposal.sol::2 => pragma solidity ^0.8;
contracts/proposals/IProposalExecutionEngine.sol::2 => pragma solidity ^0.8;
contracts/proposals/LibProposal.sol::2 => pragma solidity ^0.8;
contracts/proposals/ListOnOpenseaProposal.sol::2 => pragma solidity ^0.8;
contracts/proposals/ListOnZoraProposal.sol::2 => pragma solidity ^0.8;
contracts/proposals/ProposalExecutionEngine.sol::2 => pragma solidity ^0.8;
contracts/proposals/ProposalStorage.sol::2 => pragma solidity ^0.8;
contracts/proposals/vendor/FractionalV1.sol::2 => pragma solidity ^0.8;
contracts/proposals/vendor/IOpenseaConduitController.sol::2 => pragma solidity ^0.8;
contracts/proposals/vendor/IOpenseaExchange.sol::2 => pragma solidity ^0.8;
```

### EXPRESSIONS FOR CONSTANT VALUES SUCH AS A CALL TO `KECCAK256()`, SHOULD USE `IMMUTABLE` RATHER THAN `CONSTANT`
*There are 2 instances of this issue*
```solidity
contracts/proposals/ProposalExecutionEngine.sol
80:    uint256 private constant _STORAGE_SLOT = uint256(keccak256('ProposalExecutionEngine.Storage'));

contracts/proposals/ProposalStorage.sol
19:    uint256 private constant SHARED_STORAGE_SLOT = uint256(keccak256("ProposalStorage.SharedProposalStorage"));
```


### RETURN VALUES OF `APPROVE()` NOT CHECKED
*There are 4 instances of this issue*
```solidity
contracts/proposals/FractionalizeProposal.sol::53 => data.token.approve(address(VAULT_FACTORY), data.tokenId);
contracts/proposals/ListOnOpenseaProposal.sol::256 => token.approve(conduit, tokenId);
contracts/proposals/ListOnOpenseaProposal.sol::367 => token.approve(address(0), tokenId);
contracts/proposals/ListOnZoraProposal.sol::143 => token.approve(address(ZORA), tokenId);
```


### UNUSED/EMPTY `RECEIVE()`/`FALLBACK()` FUNCTION
*There are 2 instances of this issue*
```solidity
contracts/crowdfund/AuctionCrowdfund.sol
144:    receive() external payable {}

contracts/party/Party.sol
47:    receive() external payable {}
```

### USE SAFETRANSFER/SAFETRANSFERFROM CONSISTENTLY INSTEAD OF TRANSFER/TRANSFERFROM
It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.
*There are 2 instances of this issue*
```solidity
contracts/crowdfund/Crowdfund.sol::301 => preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
contracts/party/PartyGovernanceNFT.sol::143 => super.transferFrom(owner, to, tokenId);
```