## USE CALLDATA INSTEAD OF MEMORY

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. `60 * <mem_array>.length`). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution.

When arguments are read-only on external functions, the data location should be `calldata`

58 Instances:
-AuctionCrowdfund.sol line 196
-BuyCrowdfund.sol line 102
-BuyCrowdfundBase.sol line 96
-CollectionBuyCrowdfund.sol line 118
-Crowdfund.sol lines 191,  264, 265, 266, 308, 383
-CrowdfundFactory.sol line 110
-TokenDistributor.sol lines 331, 390
-PartyFactory.sol lines 28, 29, 30
-PartyGovernance.sol lines 394, 526, 655, 656, 657, 806, 807, 808, 810, 811, 926, 927, 973, 1001, 1012, 1083, 1084, 1095, 1096, 1106, 1107, 1127
-ArbitraryCallsProposal.sol lines 38, 95, 96, 97, 143, 145, 146, 203, 221
-FractionalizeProposal.sol line 40
-LibProposal.sol lines 9, 25, 26
-ListOnOpenseaProposal.sol lines 124, 243, 244
-ListOnZoraProposal.sol line 79
-ProposalExecutionEngine.sol lines 205, 228, 251, 


## <ARRAY>.LENGTH SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP

Reading array length at each iteration of the loop consumes more gas than necessary.
In the best case scenario (length read on a memory variable), caching the array length in the stack saves around 3 gas per iteration. In the worst case scenario (external calls at each iteration), the amount of gas wasted can be massive.

Consider storing the array’s length in a variable before the for-loop, and use this new variable instead
12 Instances:
-CollectionBuyCrowfund.sol line 62
-Crowdfund.sol lines 180, 300
-TokenDistributor.sol lines 230, 239
-PartyGovernance.sol line 306
-ArbitraryCallsProposal.sol lines 52, 61, 78
-LibProposal.sol lines 14, 32
-ListOnOpenseaProposal.sol line 291

## USING STORAGE INSTEAD OF MEMORY FOR STRUCTS/ARRAYS SAVES GAS

When fetching data from a storage location, assigning the data to a `memory` variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new `memory` variable, they incur an additional `MLOAD` rather than a cheap stack read. Instead of declearing the variable with the `memory` keyword, declaring the variable with the `storage` keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a `memory` variable, is if the full struct/array is being returned by the function, is being passed to a function that requires `memory`, or if the array/struct is being read from another memory array/struct

9 Instances:
-Crowfund.sol 349, 424
-PartyGovernance.sol 569, 619, 670, 731, 980, 994, 1037

## <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES

2 Instances:
-Crowfund.sol line 441
-TokenDistributor.sol line 381

## `++I` INSTEAD OF `I++` (OR USE ASSEMBLY WHEN APPLICABLE)

Use `++i` instead of ` i++`. This is especially useful in for loops but this optimization can be used anywhere in your code. 

1 Instance:
-CollectionBuyCrowdfund.sol line 62 

## IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for (uint256 i = 0; i < numIterations; ++i) { should be replaced with for (uint256 i; i < numIterations; ++i) {

12 Instances:
-Crowdfund.sol lines 180, 242, 300, 348
-TokenDistributor.sol lines 230, 239
-PartyGovernance.sol line 432
-ArbitraryCallsProposal.sol lines 52,  61, 78
-LibProposal.sol lines 14, 32 


## `>=` COSTS LESS GAS THAN `>`

The compiler uses opcodes `GT` and `ISZERO` for solidity code that uses `>`, but only requires `LT` for `>=`, which saves 3 gas

6 Instances:
-Crowfund.sol lines 144, 471
-ProposalExecutionEngine.sol line 236
-ArbitraryCallsProposal.sol lines 208, 226
-LibSafeERC721.sol 25


## `++I/I++` SHOULD BE `UNCHECKED{++I}`/`UNCHECKED{I++}` WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop
```
for (uint256 i = 0; i < orders.length; /** NOTE: Removed i++ **/ ) {
           // Do the thing
           // Unchecked pre-increment is cheapest
           unchecked { ++i; }   
}  
```


14 Instances:
-CollectionBuyCrowdfund.sol line 62
-Crowdfund.sol lines 180, 242, 300, 348
-TokenDistributor.sol lines 230, 239
-PartyGovernance.sol line 306
-ArbitraryCallsProposal.sol lines 52, 61, 78
-LibProposal.sol 14, 32
-ListOnOpenseaProposal.sol 291


