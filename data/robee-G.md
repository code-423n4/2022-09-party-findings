


## Caching array length can save gas


Caching the array length is more gas efficient.
This is because access to a local variable in solidity is more efficient than query storage / calldata / memory.
We recommend to change from:    

    for (uint256 i=0; i<array.length; i++) { ... }

to: 

    uint len = array.length  
    for (uint256 i=0; i<len; i++) { ... }


### Code instances:

        ArbitraryCallsProposal.sol, calls, 61
        TokenDistributor.sol, infos, 239
        PartyHelpers.sol, members, 64
        TokenDistributor.sol, infos, 230
        PartyHelpers.sol, voters, 85
        Crowdfund.sol, preciousTokens, 300
        LibProposal.sol, preciousTokens, 32
        Crowdfund.sol, contributors, 180
        CollectionBuyCrowdfund.sol, hosts, 62
        LibProposal.sol, preciousTokens, 14



## Prefix increments are cheaper than postfix increments

Prefix increments are cheaper than postfix increments. 
Further more, using unchecked {++x} is even more gas efficient, and the gas saving accumulates every iteration and can make a real change
There is no risk of overflow caused by increamenting the iteration index in for loops (the `++i` in `for (uint256 i = 0; i < numIterations; ++i)`).
But increments perform overflow checks that are not necessary in this case.
These functions use not using prefix increments (`++x`) or not using the unchecked keyword: 

### Code instances:

        just change to unchecked: Crowdfund.sol, i, 300
        just change to unchecked: ArbitraryCallsProposal.sol, i, 61
        just change to unchecked: Crowdfund.sol, i, 242
        just change to unchecked: TokenDistributor.sol, i, 239
        change to prefix increment and unchecked: PartyHelpers.sol, i, 85
        just change to unchecked: TokenDistributor.sol, i, 230
        just change to unchecked: LibProposal.sol, i, 14
        change to prefix increment and unchecked: PartyHelpers.sol, i, 64
        change to prefix increment and unchecked: PartyHelpers.sol, i, 115
        change to prefix increment and unchecked: CollectionBuyCrowdfund.sol, i, 62
        just change to unchecked: Crowdfund.sol, i, 180
        just change to unchecked: LibProposal.sol, i, 32



## Unnecessary index init


In for loops you initialize the index to start from 0, but it already initialized to 0 in default and this assignment cost gas. 
It is more clear and gas efficient to declare without assigning 0 and will have the same meaning:

### Code instances:

        PartyHelpers.sol, 85
        TokenDistributor.sol, 239
        LibProposal.sol, 32
        ArbitraryCallsProposal.sol, 61
        LibProposal.sol, 14
        PartyHelpers.sol, 115
        TokenDistributor.sol, 230
        Crowdfund.sol, 300
        Crowdfund.sol, 242
        Crowdfund.sol, 180
        PartyHelpers.sol, 64



## Rearrange state variables

You can change the order of the storage variables to decrease memory uses.

### Code instances:

In TokenDistributor.sol,rearranging the storage fields can optimize to: 2 slots from: 3 slots.
The new order of types (you choose the actual variables):
        1. IGlobals
        2. address
        3. bool

In Crowdfund.sol,rearranging the storage fields can optimize to: 6 slots from: 7 slots.
The new order of types (you choose the actual variables):
        1. IGlobals
        2. Party
        3. IGateKeeper
        4. bytes32
        5. address
        6. uint96
        7. bytes12
        8. uint16
        9. bool




## Use bytes32 instead of string to save gas whenever possible


    Use bytes32 instead of string to save gas whenever possible.
    String is a dynamic data structure and therefore is more gas consuming then bytes32.

    
### Code instances:

        CrowdfundNFTRenderer.sol (L142), string memory json = Base64.encode(bytes( string( abi.encodePacked( '{"name":"', renderTokenName(tokenId), '", "description": "AuctionCrowdfund Crowdfund NFT", "image": "data:image/svg+xml;base64,', Base64.encode(bytes(output)), '"}' ) ) )); 
        CrowdfundNFTRenderer.sol (L160), string memory json = Base64.encode(bytes( string( abi.encodePacked( '{"name":"', renderNFTName(), '", "description":"', "AuctionCrowdfund Crowdfund NFTs represent your spot in a AuctionCrowdfund party.", '"}' ) ) )); 
        PartyGovernanceNFTRenderer.sol (L142), string memory json = Base64.encode(bytes( string( abi.encodePacked( '{"name":"', renderTokenName(tokenId), '", "description": "AuctionCrowdfund Governance NFT", "image": "data:image/svg+xml;base64,', Base64.encode(bytes(output)), '"}' ) ) )); 
        PartyGovernanceNFTRenderer.sol (L160), string memory json = Base64.encode(bytes( string( abi.encodePacked( '{"name":"', renderNFTName(), '", "description":"', "AuctionCrowdfund Governance NFTs give you voting power in a AuctionCrowdfund party.", '"}' ) ) )); 



## Use != 0 instead of > 0


Using != 0 is slightly cheaper than > 0. (see https://github.com/code-423n4/2021-12-maple-findings/issues/75 for similar issue)


### Code instance:

        TokenGateKeeper.sol, 37: change 'balance > 0' to 'balance != 0'



## Unnecessary cast


    
### Code instances:

        int192 LibSafeCast.sol.safeCastInt192ToUint96 - unnecessary casting int192(i192)
        uint40 ListOnZoraProposal.sol._createZoraAuction - unnecessary casting uint40(duration)



## Use unchecked to save gas for certain additive calculations that cannot overflow


You can use unchecked in the following calculations since there is no risk to overflow:

### Code instances:

        ListOnOpenseaProposal.sol (L#210) - uint256 expiry = block.timestamp + uint256(data.duration);
        AuctionCrowdfund.sol (L#118) - expiry = uint40(opts.duration + block.timestamp);
        ListOnZoraProposal.sol (L#153) - emit ZoraAuctionCreated( auctionId, token, tokenId, listPrice, uint40(duration), uint40(block.timestamp + timeout) );
        ListOnZoraProposal.sol (L#116) - auctionId: auctionId, minExpiry: (block.timestamp + data.timeout).safeCastUint256ToUint40()
        ListOnOpenseaProposal.sol (L#165) - auctionId: auctionId, minExpiry: (block.timestamp + zoraTimeout).safeCastUint256ToUint40()
        BuyCrowdfundBase.sol (L#73) - expiry = uint40(opts.duration + block.timestamp);



## Consider inline the following functions to save gas


    You can inline the following functions instead of writing a specific function to save gas.
    (see https://github.com/code-423n4/2021-11-nested-findings/issues/167 for a similar issue.)

    
### Code instances

        AuctionCrowdfund.sol, _getFinalPrice, { return lastBid; }
        BuyCrowdfundBase.sol, _getFinalPrice, { return settledPrice; }
        LibSafeCast.sol, safeCastUint96ToInt192, { return int192(uint192(v)); }
        PartyGovernanceNFTRenderer.sol, renderNFTName, { return string.concat(name, " Party"); }



## Inline one time use functions


The following functions are used exactly once. Therefore you can inline them and save gas and improve code clearness.
    

### Code instances:

        PartyGovernanceNFTRenderer.sol, renderNFTName
        CrowdfundNFTRenderer.sol, renderEthOwed
        ArbitraryCallsProposal.sol, _isCallAllowed
        CrowdfundNFTRenderer.sol, renderOwnerAddress
        ListOnZoraProposal.sol, _createZoraAuction
        BuyCrowdfundBase.sol, _initialize
        PartyGovernanceNFTRenderer.sol, renderVotingPowerAndDistributionShare
        CrowdfundNFTRenderer.sol, renderEthContributed
        CrowdfundNFT.sol, _doesTokenExistFor
        PartyGovernanceNFTRenderer.sol, renderDelegateAddress
        AuctionCrowdfund.sol, _getFinalPrice
        ListOnOpenseaProposal.sol, _cleanUpListing


## Unused state variables

Unused state variables are gas consuming at deployment (since they are located in storage) and are 
a bad code practice. Removing those variables will decrease deployment gas cost and improve code quality. 
This is a full list of all the unused storage variables we found in your code base. 

### Code instances:

        PartyGovernanceNFTRenderer.sol, feeRecipient
        LibGlobals.sol, GLOBAL_DAO_AUTHORITIES
        PartyGovernanceNFTRenderer.sol, _votingPowerSnapshotsByVoter
        PartyGovernanceNFTRenderer.sol, lastProposalId
        PartyGovernanceNFTRenderer.sol, isApprovedForAll
        PartyGovernanceNFTRenderer.sol, _balanceOf



## Unused declared local variables

Unused local variables are gas consuming, since the initial value assignment costs gas. And are 
a bad code practice. Removing those variables will decrease the gas cost and improve code quality. 
This is a full list of all the unused storage variables we found in your code base. 

### Code instances:

        ProposalStorage.sol, _getSharedProposalStorage, s
        ProposalExecutionEngine.sol, _getStorage, slot


