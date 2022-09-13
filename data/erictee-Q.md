### [L-01] ```approve()``` and ```safeApprove()``` should be replaced with ```safeIncreaseAllowance()``` / ```safeDecreaseAllowance()```


#### Impact
```approve()``` & ```safeApprove()``` are deprecated and subject to a known front-running attack. Consider using  ```safeIncreaseAllowance()``` & ```safeDecreaseAllowance()``` instead.


#### Findings:
```
contracts/proposals/FractionalizeProposal.sol:L53                 data.token.approve(address(VAULT_FACTORY), data.tokenId);

contracts/proposals/ListOnZoraProposal.sol:L143                 token.approve(address(ZORA), tokenId);

contracts/proposals/ListOnOpenseaProposal.sol:L256                 token.approve(conduit, tokenId);

contracts/proposals/ListOnOpenseaProposal.sol:L367                     token.approve(address(0), tokenId);

```
### [L-02] ```require()``` should be used instead of ```assert()```


#### Impact
Prior to solidity version 0.8.0, hitting an assert consumes the remainder of the transaction’s available gas rather than returning it, as ```require()```/```revert()``` do. ```assert()``` should be avoided even past solidity version 0.8.0 as its [documentation](https://docs.soliditylang.org/en/v0.8.14/control-structures.html#panic-via-assert-and-error-via-require) states that “The assert function creates an error of type Panic(uint256). … Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix”.


#### Findings:
```
contracts/distribution/TokenDistributor.sol:L385            assert(tokenType == TokenType.Erc20);

contracts/distribution/TokenDistributor.sol:L411        assert(tokenType == TokenType.Erc20);

contracts/utils/ReadOnlyDelegateCall.sol:L30            assert(false);

contracts/party/PartyGovernance.sol:L504        assert(tokenType == ITokenDistributor.TokenType.Erc20);

contracts/party/PartyGovernanceNFT.sol:L179        assert(false); // Will not be reached.

contracts/proposals/FractionalizeProposal.sol:L67        assert(vault.balanceOf(address(this)) == supply);

contracts/proposals/ListOnZoraProposal.sol:L120        assert(step == ZoraStep.ListedOnZora);

contracts/proposals/ListOnOpenseaProposal.sol:L221        assert(step == ListOnOpenseaStep.ListedOnOpenSea);

contracts/proposals/ListOnOpenseaProposal.sol:L302        assert(SEAPORT.validate(orders));

contracts/proposals/ProposalExecutionEngine.sol:L142                assert(currentInProgressProposalId == 0);

contracts/crowdfund/CrowdfundNFT.sol:L166        assert(false); // Will not be reached.

```

### [L-03] Avoid floating pragmas for non-library contracts.


#### Impact
While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

#### Findings:
```
contracts/distribution/ITokenDistributorParty.sol:L2     pragma solidity ^0.8;

contracts/distribution/ITokenDistributor.sol:L2     pragma solidity ^0.8;

contracts/distribution/TokenDistributor.sol:L2     pragma solidity ^0.8;

contracts/utils/Proxy.sol:L2     pragma solidity ^0.8;

contracts/utils/LibSafeCast.sol:L2     pragma solidity ^0.8;

contracts/utils/ReadOnlyDelegateCall.sol:L2     pragma solidity ^0.8;

contracts/utils/LibAddress.sol:L2     pragma solidity ^0.8;

contracts/utils/LibERC20Compat.sol:L2     pragma solidity ^0.8;

contracts/utils/Implementation.sol:L2     pragma solidity ^0.8;

contracts/utils/LibRawResult.sol:L2     pragma solidity ^0.8;

contracts/utils/LibSafeERC721.sol:L2     pragma solidity ^0.8;

contracts/utils/EIP165.sol:L2     pragma solidity ^0.8;

contracts/globals/IGlobals.sol:L2     pragma solidity ^0.8;

contracts/globals/Globals.sol:L2     pragma solidity ^0.8;

contracts/party/Party.sol:L2     pragma solidity ^0.8;

contracts/party/PartyGovernance.sol:L2     pragma solidity ^0.8;

contracts/party/PartyFactory.sol:L2     pragma solidity ^0.8;

contracts/party/IPartyFactory.sol:L2     pragma solidity ^0.8;

contracts/party/PartyGovernanceNFT.sol:L2     pragma solidity ^0.8;

contracts/renderers/DummyERC721Renderer.sol:L2     pragma solidity ^0.8;

contracts/gatekeepers/IGateKeeper.sol:L2     pragma solidity ^0.8;

contracts/proposals/FractionalizeProposal.sol:L2     pragma solidity ^0.8;

contracts/proposals/ProposalStorage.sol:L2     pragma solidity ^0.8;

contracts/proposals/ListOnZoraProposal.sol:L2     pragma solidity ^0.8;

contracts/proposals/ArbitraryCallsProposal.sol:L2     pragma solidity ^0.8;

contracts/proposals/LibProposal.sol:L2     pragma solidity ^0.8;

contracts/proposals/IProposalExecutionEngine.sol:L2     pragma solidity ^0.8;

contracts/proposals/ListOnOpenseaProposal.sol:L2     pragma solidity ^0.8;

contracts/proposals/ProposalExecutionEngine.sol:L2     pragma solidity ^0.8;

contracts/proposals/vendor/FractionalV1.sol:L2     pragma solidity ^0.8;

contracts/proposals/vendor/IOpenseaExchange.sol:L2     pragma solidity ^0.8;

contracts/proposals/vendor/IOpenseaConduitController.sol:L2     pragma solidity ^0.8;

contracts/market-wrapper/IMarketWrapper.sol:L2     pragma solidity ^0.8;

contracts/vendor/markets/IZoraAuctionHouse.sol:L2     pragma solidity ^0.8;

contracts/tokens/ERC721Receiver.sol:L2     pragma solidity ^0.8;

contracts/tokens/ERC1155Receiver.sol:L2     pragma solidity ^0.8;

contracts/tokens/IERC721.sol:L2     pragma solidity ^0.8;

contracts/tokens/IERC1155.sol:L2     pragma solidity ^0.8;

contracts/tokens/IERC20.sol:L2     pragma solidity ^0.8;

contracts/tokens/IERC721Receiver.sol:L2     pragma solidity ^0.8;

contracts/crowdfund/CrowdfundNFT.sol:L2     pragma solidity ^0.8;

contracts/crowdfund/CollectionBuyCrowdfund.sol:L2     pragma solidity ^0.8;

contracts/crowdfund/CrowdfundFactory.sol:L2     pragma solidity ^0.8;

contracts/crowdfund/BuyCrowdfund.sol:L2     pragma solidity ^0.8;

contracts/crowdfund/Crowdfund.sol:L2     pragma solidity ^0.8;

contracts/crowdfund/BuyCrowdfundBase.sol:L2     pragma solidity ^0.8;

contracts/crowdfund/AuctionCrowdfund.sol:L2     pragma solidity ^0.8;

```
### [L-04] ```require()```/```revert()``` statements should have descriptive strings.


#### Impact
Consider adding descriptive strings in ```require()```/```revert()```.


#### Findings:
```
contracts/utils/ReadOnlyDelegateCall.sol:L20        require(msg.sender == address(this));

contracts/proposals/ProposalExecutionEngine.sol:L246        require(proposalType != ProposalType.Invalid);

contracts/proposals/ProposalExecutionEngine.sol:L247        require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));

```
### [L-05] ```_safemint()``` should be used rather than ```_mint()``` wherever possible


#### Impact
```_mint()``` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of ```_safeMint()``` which ensures that the recipient is either an EOA or implements ```IERC721Receiver```. Both [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/transmissions11/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function


#### Findings:
```
contracts/party/PartyGovernanceNFT.sol:L132        _mint(owner, tokenId);

contracts/crowdfund/CrowdfundNFT.sol:L141    function _mint(address owner) internal returns (uint256 tokenId)

contracts/crowdfund/Crowdfund.sol:L439                _mint(contributor);

```
### [L-06] Events not emitted for important state changes.


#### Impact
When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.


#### Findings:
```
contracts/globals/Globals.sol:L59             function setBytes32(uint256 key, bytes32 value) external onlyMultisig {

contracts/globals/Globals.sol:L63             function setUint256(uint256 key, uint256 value) external onlyMultisig {

contracts/globals/Globals.sol:L67             function setAddress(uint256 key, address value) external onlyMultisig {

contracts/globals/Globals.sol:L71             function setIncludesBytes32(uint256 key, bytes32 value, bool isIncluded) external onlyMultisig {

contracts/globals/Globals.sol:L75             function setIncludesUint256(uint256 key, uint256 value, bool isIncluded) external onlyMultisig {

contracts/globals/Globals.sol:L79             function setIncludesAddress(uint256 key, address value, bool isIncluded) external onlyMultisig {

```
### [L-07] Unsafe ERC20 Operation(s)


#### Impact
ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard.

It is therefore recommended to always either use OpenZeppelin's SafeERC20 library or at least to wrap each operation in a require statement.


#### Findings:
```

contracts/party/PartyGovernanceNFT.sol:L143        super.transferFrom(owner, to, tokenId);

contracts/crowdfund/Crowdfund.sol:L301            preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);

```

### [N-01] Open TODOs


#### Impact
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment.


#### Findings:
```
contracts/renderers/DummyERC721Renderer.sol:L8        // TODO: make this human readable

```

