### 1. calldata instead of memory in external functions

It is reasonable to use `calldata` instead of `memory` for input arrays in `external` functions to reduce gas consumption and make the code more clear. As an example, `createParty` function from `PartyFactory` contract should be changed.