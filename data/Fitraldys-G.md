1. empty blocks should be remove or emit something

he code should be refactored such that they no longer exist, or the block should do something useful,
such as emitting an event or reverting. If the contract is meant to be extended,
the contract should be `abstract` and the function signatures be added without any default implementation.
If the block is an empty if-statement block to avoid doing subsequent checks in the else-if/else conditions,
the else-if/else conditions should be nested under the negation of the if-statement,
because they involve different classes of checks, which may lead to the introduction of errors when the code is
later modified (`if(x){}else if(y){...}else{...}` => `if(!x){if(y){...}else{...}}`)

POC :

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L47
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L144

2. cheaper foor loops

You can get cheaper for loops (at least 25 gas, however can be up to 80 gas under certain conditions), by rewriting:

```
for (uint256 i = 0; i < orders.length; /** NOTE: Removed i++ **/ ) {
        // Do the thing
        // Unchecked pre-increment is cheapest
        unchecked { ++i; }
}     
```

POC : 

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239

3. use `abi.encodepacked` for gas optimization

Changing the `abi.encode` function to `abi.encodePacked` can save gas since the `abi.encode` function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, `abi.encodePacked` is more gas-efficient.

POC :

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L115

4. Declaring `uint256 i`

Declaring `uint256 i = 0;` means doing an MSTORE of the value 0 Instead you could just declare `uint256 i` to declare the variable without assigning it’s default value, saving 3 gas per declaration

POC :

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239

