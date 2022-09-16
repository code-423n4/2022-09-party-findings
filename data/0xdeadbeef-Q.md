## In the file AuctionCrowdfund.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 59: initialContributor
	line 62: initialDelegate
	line 178: market
	line 218: market
```
## In the file BuyCrowdfund.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 42: initialContributor
	line 45: initialDelegate
```
## In the file BuyCrowdfundBase.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 39: initialContributor
	line 42: initialDelegate
	line 105: partyFactory
```
## In the file CollectionBuyCrowdfund.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 43: initialContributor
	line 46: initialDelegate
```
## In the file Crowdfund.sol: 
Line 439	                _mint(contributor): 
_safemint() should be used instead of _mint() function whereever possible

Line 480	            party_.mint: 
_safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 56: initialContributor
	line 57: initialDelegate
	line 229: contributor
	line 339: contributor
	line 379: contributor
```

## In the file CrowdfundNFT.sol: 
Line 141	    function _mint(address owner) internal returns (uint256 tokenId: 
 _safemint() should be used instead of _mint() function whereever possible


## In the file TokenDistributor.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 37: token
	line 76: partyDao
	line 130: token
	line 144: ownerOfPartyToken
	line 315: token
	line 315: token
	line 403: token
```
the assert() function when false, uses up all the remaining gas and reverts all the changes made.
Line 385:	            assert(tokenType == TokenType.Erc20);

the assert() function when false, uses up all the remaining gas and reverts all the changes made.
Line 411:	        assert(tokenType == TokenType.Erc20);

## In the file AllowListGateKeeper.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 15: participant
```

## In the file TokenGateKeeper.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 10: owner
	line 32: participant
```

## In the file Globals.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 23: multiSig_
	line 27: newMultiSig
	line 40: uint160
	line 44: uint160
	line 55: value
	line 67: value
	line 79: value
```

## In the file FoundationMarketWrapper.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 24: _foundationMarket
	line 49: nftContract
	line 90: market
```

## In the file KoansMarketWrapper.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 24: _koansAuctionHouse
	line 106: market
```

## In the file NounsMarketWrapper.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 23: _nounsAuctionHouse
	line 104: market
```

## In the file ZoraMarketWrapper.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 27: _zoraAuctionHouse
	line 53: nftContract
	line 104: market
```

## In the file Party.sol: 
Line 24	        address mintAuthority: 
: _safemint() should be used instead of _mint() function whereever possible

Line 43	            initData.mintAuthorit: 
: _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 24: mintAuthority
```

## In the file PartyFactory.sol: 
Line 44	            mintAuthority: authorit: 
: _safemint() should be used instead of _mint() function whereever possible

## In the file PartyGovernance.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 153: proposer
	line 240: partyDao
	line 250: partyDao
	line 316: _getProposalExecutionEngine
	line 361: voter
	line 485: token
	line 506: distributor
	line 766: _getProposalExecutionEngine
	line 784: targetAddress
	line 832: _getProposalExecutionEngine
	line 890: voter
	line 158: voter
	line 989: voter
```
the assert() function when false, uses up all the remaining gas and reverts all the changes made.
Line 504:	        assert(tokenType == ITokenDistributor.TokenType.Erc20);

## In the file PartyGovernanceNFT.sol: 
Line 28	    address public mintAuthority: 
: _safemint() should be used instead of _mint() function whereever possible

Line 36	        if (msg.sender != mintAuthority) : 
: _safemint() should be used instead of _mint() function whereever possible

Line 37	            revert OnlyMintAuthorityError(msg.sender, mintAuthority): 
: _safemint() should be used instead of _mint() function whereever possible

Line 55	        address mintAuthority: 
: _safemint() should be used instead of _mint() function whereever possible

Line 62	        mintAuthority = mintAuthority_: 
: _safemint() should be used instead of _mint() function whereever possible

Line 120	    function mint: 
: _safemint() should be used instead of _mint() function whereever possible

Line 132	        _mint(owner, tokenId): 
 _safemint() should be used instead of _mint() function whereever possible

Line 170	        delete mintAuthority: 
: _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 55: mintAuthority_
	line 70: owner
	line 121: owner
	line 123: delegate
	line 136: owner
	line 147: owner
	line 158: owner
```

the assert() function when false, uses up all the remaining gas and reverts all the changes made.
Line 179:	        assert(false); // Will not be reached.
## In the file ArbitraryCallsProposal.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 206: operator
	line 224: operator
```
## In the file ListOnOpenseaProposal.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 255: conduit
```

the assert() function when false, uses up all the remaining gas and reverts all the changes made.
Line 221:	        assert(step == ListOnOpenseaStep.ListedOnOpenSea);

the assert() function when false, uses up all the remaining gas and reverts all the changes made.
Line 302:	        assert(SEAPORT.validate(orders));

## In the file ProposalExecutionEngine.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 99: oldImpl
	line 254: expectedImpl
	line 260: newImpl
	line 267: IMPL
```

the assert() function when false, uses up all the remaining gas and reverts all the changes made.
Line 142:	                assert(currentInProgressProposalId == 0);

## In the file FractionalizeProposal.sol: 
Line 54	        uint256 vaultId = VAULT_FACTORY.mint: 
: _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 53: VAULT_FACTORY
```

the assert() function when false, uses up all the remaining gas and reverts all the changes made.
Line 67:	        assert(vault.balanceOf(address(this)) == supply);

## In the file ListOnZoraProposal.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 143: ZORA
```

the assert() function when false, uses up all the remaining gas and reverts all the changes made.
Line 120:	        assert(step == ZoraStep.ListedOnZora);

## In the file ProposalStorage.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 39: impl
	line 42: oldImpl
```

## In the file FractionalV1.sol: 
Line 13	    function mint: 
: _safemint() should be used instead of _mint() function whereever possible

## In the file CrowdfundNFTRenderer.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 89: contributor
	line 95: contributor
	line 101: contributor
```
## In the file DummyERC721Renderer.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
```
## In the file IERC721Renderer.sol: 
## In the file PartyGovernanceNFTRenderer.sol: 
Line 37	    address mintAuthority: 
: _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 37: mintAuthority
	line 101: owner
	line 108: delegatedAddress
```
## In the file ERC1155.sol: 
Line 139	    function _mint: 
 _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 36: operator
	line 43: from
```
