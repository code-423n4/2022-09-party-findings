### 1. calldata instead of memory in external functions

It is reasonable to use `calldata` instead of `memory` for input arrays in `external` functions to reduce gas consumption and make the code more clear. As an example, `createParty` function from `PartyFactory` contract should be changed.

### 2. LibGlobals constants

It is reasonable to use in `LibGlobals` library an enum instead of 22 constants.

### 3. oldDelegate == newDelegate in _rebalanceDelegates

There is a `_rebalanceDelegates` function in `PartyGovernance`. For the case when `oldDelegate` parameter equals to `newDelegate` parameter it will be more clear to use special check and update, instead of doing more complicated interdependent logic.

### 4. MarketWrappers payable functions

MarketWrappers don't have payable functions but function bid sends something.