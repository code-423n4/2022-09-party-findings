# [G-01] Redundant zero initialization

Solidity does not recognize null as a value, so uint variables are initialized to zero. Setting a uint variable to zero is redundant and can waste gas.

Locations where this was found include
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L230
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L239
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L432
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L291
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L52
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L61
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L78
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L14
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L32
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L180
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L242
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L300
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L348

## Recommended Mitigation Steps

Remove the redundant zero initialization
`uint256 i;` instead of `uint256 i = 0;`

# [G-02] Use != 0 instead of > 0

Using `> 0` uses slightly more gas than using `!= 0`. Use `!= 0` when comparing uint variables to zero, which cannot hold values below zero

Locations where this was found include
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L144
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L471

## Recommended Mitigation Steps

Replace `> 0` with `!= 0` to save gas

# [G-03] Use prefix not postfix in loops

Using a prefix increment (++i) instead of a postfix increment (i++) saves gas for each loop cycle and so can have a big gas impact when the loop executes on a large number of elements.

Locations where this was found include
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62

## Recommended Mitigation Steps

Use prefix not postfix to increment in a loop

# [G-04] For loop incrementing can be unsafe

For loops that use i++ do not need to use safemath for this operation because the loop would run out of gas long before this point. Making this addition operation unsafe using unchecked saves gas.

Sample code to make the for loop increment unsafe
```
for (uint i = 0; i < length; i = unchecked_inc(i)) {
    // do something that doesn't change the value of i
}

function unchecked_inc(uint i) returns (uint) {
    unchecked {
        return i + 1;
    }
}
```

Idea borrowed from
https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked

Locations where this was found include
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L230
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L239
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L306
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L291
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L52
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L61
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L78
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L14
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L32
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L180
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L242
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L300
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L348
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62

## Recommended Mitigation Steps

Make the increment in for loops unsafe to save gas

# [G-05] Use iszero assembly for zero checks

Comparing a value to zero can be done using the `iszero` EVM opcode. This can save gas

Source from t11s https://twitter.com/transmissions11/status/1474465495243898885

Locations where this was found include
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/LibERC20Compat.sol#L18
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/LibERC20Compat.sol#L21
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L344
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L230
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L445
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L600
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L844
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1018
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1023
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L87
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L135
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L122
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L127
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L140
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L142
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L173
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L188
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L438
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L234
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L237
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L122
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L123

## Recommended Mitigation Steps

Use the assembly `iszero` evm opcode to compare values to zero

# [G-06] Save gas with unchecked

Use unchecked math when there is no overflow risk to save gas. Solidity 0.8.X adds Safemath by default, and there are cases where Safemath provides no benefit and uses extra gas.

Locations where this was found include
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L358
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L360

## Recommended Mitigation Steps

Add unchecked around math that can't overflow for gas savings. In Solidity before 0.8.0, use the normal math operators instead of safe math functions.

# [G-07] Add payable to functions that won't receive ETH

Identifying a function as payable saves gas. Functions that have a modifier like onlyPartyDao cannot be called by normal users and will not mistakenly receive ETH. These functions can be payable to save gas.

There are many functions that have access control modifiers in the contracts. Some examples are
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L451
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L458
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L483
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L526
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L800

## Recommended Mitigation Steps

Add payable to these functions for gas savings

# [G-08] Add payable to constructors that won't receive ETH

Identifying a constructor as payable saves gas. Constructors should only be called by the admin or deployer and should not mistakenly receive ETH. Constructors can be payable to save gas.

Locations where this was found include
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/Globals.sol#L23
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/Implementation.sol#L11
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/Implementation.sol#L22
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/Proxy.sol#L16
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L93
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernanceNFT.sol#L44
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyFactory.sol#L21
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L266
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/Party.sol#L28
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L70
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L108
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/FractionalizeProposal.sol#L34
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L83
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundNFT.sol#L34
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfund.sol#L62
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L118
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L104
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L77
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundFactory.sol#L25
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L67

## Recommended Mitigation Steps

Add payable to these functions for gas savings

# [G-09] Use internal function in place of modifier

An internal function can save gas vs. a modifier. A modifier inlines the code of the original function but an internal function does not.

Source https://blog.polymath.network/solidity-tips-and-tricks-to-save-gas-and-reduce-bytecode-size-c44580b218e6#dde7

Locations where this was found include
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/Globals.sol#L16
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/Implementation.sol#L14
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/Implementation.sol#L22
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L74
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L85
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernanceNFT.sol#L35
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L217
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L225
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L238
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L249
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L258
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundNFT.sol#L28
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L60

## Recommended Mitigation Steps

Use internal functions in place of modifiers to save gas.

# [G-10] Use uint not bool

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

Locations where this was found include
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L28
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L62
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L108
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L197
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L697
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/vendor/markets/IZoraAuctionHouse.sol#L15
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L61
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L144
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L98

## Recommended Mitigation Steps

Replace bool variables with uints

# [G-11] Use uint256 not smaller ints

From [the solidity docs](https://docs.soliditylang.org/en/v0.8.15/internals/layout_in_storage.html?highlight=elements%20that%20are%20smaller%20than%2032%20bytes#layout-of-state-variables-in-storage)
```
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
```

Locations where this was found include
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L27
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L30
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L36
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L60
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L62
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L64

## Recommended Mitigation Steps

Replace smaller int or uint variables with uint256 variables

# [G-12] Use newer solidity version

A solidity version before 0.8.X is used in this project. The latest release of solidity includes changes that can provide gas savings. The improvements include:

* [Low level inliner](https://blog.soliditylang.org/2021/03/02/saving-gas-with-simple-inliner/) from `0.8.2`, leads to cheaper runtime gas. Especially relevant when the contract has small functions. For example, OpenZeppelin libraries typically have a lot of small helper functions and if they are not inlined, they cost an additional 20 to 40 gas because of 2 extra `jump` instructions and additional stack operations needed for function calls.
* [Optimizer improvements in packed structs](https://blog.soliditylang.org/2021/03/23/solidity-0.8.3-release-announcement/#optimizer-improvements): Before `0.8.3`, storing packed structs, in some cases used an additional storage read operation. After [EIP-2929](https://eips.ethereum.org/EIPS/eip-2929), if the slot was already cold, this means unnecessary stack operations and extra deploy time costs. However, if the slot was already warm, this means additional cost of `100` gas alongside the same unnecessary stack operations and extra deploy time costs.
* [Custom errors](https://blog.soliditylang.org/2021/04/21/custom-errors) from `0.8.4`, leads to cheaper deploy time cost and run time cost. Note: the run time cost is only relevant when the revert condition is met. In short, replace revert strings by custom errors.
* Solidity version `0.8.13` can save more gas with [Yul IR pipeline](https://blog.soliditylang.org/2022/03/16/solidity-0.8.13-release-announcement/#yul-ir-pipeline-production-ready)

Source https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#upgrade-to-at-least-084

## Recommended Mitigation Steps

Use solidity release 0.8.13 with Yul IR pipeline and other improvements for gas savings

# [G-13] Use Solidity errors instead of require

Solidity errors introduced in version 0.8.4 can save gas on revert conditions
https://blog.soliditylang.org/2021/04/21/custom-errors/
https://twitter.com/PatrickAlphaC/status/1505197417884528640

Locations where this was found include
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/ReadOnlyDelegateCall.sol#L20
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L246
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L247

## Recommended Mitigation Steps

Replace require blocks with new solidity errors described in https://blog.soliditylang.org/2021/04/21/custom-errors/

# [G-14] Use simple comparison in if statement

The comparison operators >= and <= use more gas than >, <, or ==. Replacing the  >= and ≤ operators with a comparison operator that has an opcode in the EVM saves gas

The existing code is 
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L276-L280
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L159-L163
```
if (block.timestamp >= expiry) {
    // Expired. `finalize()` needs to be called.
    return CrowdfundLifecycle.Expired;
}
return CrowdfundLifecycle.Active;
```

A simple comparison can be used for gas savings by reversing the logic
```
if (block.timestamp < expiry) {
    return CrowdfundLifecycle.Active;
}
// Expired. `finalize()` needs to be called.
return CrowdfundLifecycle.Expired;
```

## Recommended Mitigation Steps

Replace the comparison operator and reverse the logic to save gas using the suggestions above

# [G-15] Bitshift for divide by 2

When multiply or dividing by a power of two, it is cheaper to bitshift than to use standard math operations.

There is a divide by 2 operation on these lines
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L434

## Recommended Mitigation Steps

Bitshift right by one bit instead of dividing by 2 to save gas

# [G-16] Non-public variables save gas

Many constant variables are public, but changing the visibility of these variables to private or internal can save gas.

Locations where this was found include
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/Implementation.sol#L9
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/Proxy.sol#L12
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L59
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyFactory.sol#L18
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L64
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L100
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L102
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/FractionalizeProposal.sol#L31

## Recommended Mitigation Steps

Declare some public variables as private or internal to save gas

# [G-17] Use calldata instead of memory for function arguments

Using calldata instead of memory for function arguments saves gas sometimes. This can happen when a function is called externally and the memory array values are kept in `calldata` and copied to `memory` during ABI decoding (using the opcode `calldataload` and `mstore`). If the array is used in a for loop, `arr[i]` accesses the value in memory using a `mload`. If calldata is used instead, then instead of going via memory, the value is directly read from `calldata` using `calldataload`. That is, there are no intermediate memory operations that carries this value.

Many cases of function arguments using memory instead of calldata can use this improvement to save gas
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L273
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L274
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L656
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L657
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L807
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L808
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1083
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1084
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1095
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1096
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1106
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1107

Source
https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#use-calldata-instead-of-memory-for-function-parameters

## Recommended Mitigation Steps

Change function arguments from memory to calldata

# [G-18] Write contracts in vyper

The contracts are all written entirely in solidity. Writing contracts with vyper instead of solidity can save gas.

Source https://twitter.com/eiber_david/status/1515737811881807876
doggo demonstrates https://twitter.com/fubuloubu/status/1528179581974417414?t=-hcq_26JFDaHdAQZ-wYxCA&s=19

## Recommended Mitigation Steps

Write some or all of the contracts in vyper to save gas