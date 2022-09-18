# Low effect on readability

## [G-01] Optimisation fo for loop
The easiest thing to do is to optimize every `for` loop, the objective is to replace those like the following example:

    for  (uint256 i =  0; i < array.length; i++)  {
    	//do something
    }

to

    uint256 len =  array.length;
    for  (uint256 i; i < len;)  {
    	//do something
    	unchecked{ ++i; }
    }

By doing so, the `length` is cached which is cheaper than looking at it every loop, `i = 0` is not initialized since `uint` have already a default value of `0` and the increment is transformed to a cheaper form since it can't overflow. (This is cheaper because without `unchecked` there is a check for overflow at each calculation and with the post-increment, the EVM need to store the value with and without increment, but the pre-increment only store the value with increment.)

3 instances: 

 - https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62-L67
 - https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230-L232
 - https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239-L241

Consider optimizing `for` loop.

With those change, these evolutions in gas average report can be observed:

    CollectionBuyCrowdfund: Deployment: 2582486 -> 2581279 (-1207)
    CollectionBuyCrowdfund: buy: 42786 -> 42771 (-15)
    TokenDistributor: Deployment: 1492622 -> 1490022 (-2600)


## [G-02] Using `calldata` instead of `memory` for read only argument in external function
If a function parameter is read only, it is cheaper in gas to use `calldata` instead of `memory`.

7 instance:

 - https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L110
 - https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L196
 - https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L118
 - https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L33
 - https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L28-L30

 
Consider changing `memory` to `calldata` in these lines

With those change, these evolutions in gas average report can be observed:

    AuctionCrowdfund: Deployment:  2790146 ->  2817977 (+27831)
    AuctionCrowdfund: initialize: 209495 -> 207640 (-1855)
    AuctionCrowdfund: finalize: 30202 -> 29426 (-776)
    CollectionBuyCrowdfund: Deployment: 2582486 -> 2613717 (+31231)
    CollectionBuyCrowdfund: buy: 42786 -> 42736 (-50)
    Party: Deployment: 4528700 -> 4570961 (+42261)
    Party: initialize: 182337 -> 177648 (-4689)
    PartyFactory: Deployment: 655926 -> 637307 (-18619)
    PartyFactory: createParty: 297056 -> 291766 (-5290)