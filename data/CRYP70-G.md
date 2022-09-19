## `++i` Saves More Gas Than `i++`
`++i`  generally costs less gas than `i++` or `i = i + 1` (about 5 units per increment) because `i++` must increment a value and then "return" the old value which means the program may need to hold two numbers in memory.  When `++i` is used, it will only ever use one number in memory.

See the example below for an simplified illustration:

```
pragma solidity ^0.8.13;

contract MyFavouriteCounter {

uint public count;

function incrementPrefixCount() public returns (uint) {
    count = 1;
    return (++count); // returns 2
}

function incrementPostfixCount() public returns (uint) {
    count = 1;
    return (count++); // returns 1
}
}
```

I managed to identify this in the following locations:
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62


## Use `external` instead of `public` for functions only called outside the contract
I recommend changing the functions outlined below to be externally facing contracts as they are not used within the contract itself. This might help in saving gas as calling a `public` function costs `496` gas while an `external` function only uses `261` gas. The reason for this is that `public` functions need to write all of its arguments to memory so they may be called internally, which is actually an entirely different process than external calls. For external functions, the compiler does not allow internal calls so it allows arguments to be read from calldata, thus skipping an entire copy step.

Recommendation:
Simply changing the functions outlined from `public` facing to `external`

This was identified in the following locations:
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L39
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L65
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L91
