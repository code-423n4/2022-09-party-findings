## [NAZ-G1] Use of `2**256 - 1 && type(uint256).max` When `2**255` Can Be Used
**Context**: [`ProposalExecutionEngine.sol#L161`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L161)

**Description**:
Infinity can also be represented via ``2**255`, it's hex representation is `0x8000000000000000000000000000000000000000000000000000000000000000` while `2**256 - 1` is `0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff`. Then main difference is and where the gas savings come from is, zeros are cheaper than non-zero values in hex representation.

**Recommendation**: 
Use `2**255` instead of `2**256 - 1` to save gas on deployment.


## [NAZ-G] Use `++index` instead of `index++` to increment a loop counter
**Context**: [`CollectionBuyCrowdfund.sol#L62`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62)

**Description**:
Due to reduced stack operations, using `++index` saves 5 gas per iteration.

**Recommendation**: 
Use `++index `to increment a loop counter.


## [NAZ-G2] The Increment In For Loop Post Condition Can Be Made Unchecked
**Context**: [`CollectionBuyCrowdfund.sol#L62`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62), [`ArbitraryCallsProposal.sol#L52`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52), [`ArbitraryCallsProposal.sol#L61`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61), [`ArbitraryCallsProposal.sol#L78`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78), [`TokenDistributor.sol#L230`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230), [`TokenDistributor.sol#L239`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239), [`Crowdfund.sol#L180`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180), [`Crowdfund.sol#L242`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L242), [`Crowdfund.sol#L300`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300), [`Crowdfund.sol#L348`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L348), [`PartyGovernance.sol#L306`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L306), [`ListOnOpenseaProposal.sol#L291`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291), [`LibProposal.sol#L14`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L14), [`LibProposal.sol#L32`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L32)

**Description**:
(This is only relevant if you are using the default solidity checked arithmetic). `i++` involves checked arithmetic, which is not required. This is because the value of `i` is always strictly less than length <= 2**256 - 1. Therefore, the theoretical maximum value of `i` to enter the for-loop body is `2**256 - 2`. This means that the `i++` in the for loop can never overflow. Regardless, the overflow checks are performed by the compiler.

Unfortunately, the Solidity optimizer is not smart enough to detect this and remove the checks. One can manually do this by:
```js
for (uint i = 0; i < length; ) {
    // do something that doesn't change the value of i
    unchecked {
        ++i;
    }
}
```
Note that it’s important that the call to `unchecked_inc` is inlined. This is only possible for solidity versions starting from `0.8.2`.

**Recommendation**: 
Consider doing the increment in the for loop post condition in an unchecked block.


## [NAZ-G3] Catching The Array Length Prior To Loop
**Context**: [`CollectionBuyCrowdfund.sol#L62`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62), [`ArbitraryCallsProposal.sol#L52`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52), [`ArbitraryCallsProposal.sol#L61`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61), [`ArbitraryCallsProposal.sol#L78`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78), [`TokenDistributor.sol#L230`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230), [`TokenDistributor.sol#L239`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239), [`Crowdfund.sol#L180`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180), [`Crowdfund.sol#L300`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300), [`PartyGovernance.sol#L306`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L306), [`ListOnOpenseaProposal.sol#L291`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291), [`LibProposal.sol#L14`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L14), [`LibProposal.sol#L32`](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L32)

**Description**:
One can save gas by caching the array length (in stack) and using that set variable in the loop. Replace state variable reads and writes within loops with local variable reads and writes. This is done by assigning state variable values to new local variables, reading and/or writing the local variables in a loop, then after the loop assigning any changed local variables to their equivalent state variables.

**Recommendation**: 
Simply do something like so before the for loop: ```uint length =  variable.length```. Then add ```length``` in place of ``` variable.length``` in the for loop. 


## [NAZ-G4] Setting The Constructor To Payable
**Context**: [`All Contracts`](https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts)

**Description**:
You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of `msg.value == 0` and saves 21 gas on deployment with no security risks.

**Recommendation**: 
Set the constructor to payable.


## [NAZ-G5] Use of Custom Errors Instead of String
**Context**: [`All Contracts`](https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts)

**Description**:
To save some gas the use of custom errors leads to cheaper deploy time cost and run time cost. The run time cost is only relevant when the revert condition is met.

**Recommendation**: 
Use Custom Errors instead of strings.


## [NAZ-G6] Function Ordering via Method ID
**Context**: [`All Contracts`](https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts)

**Description**:
Contracts most called functions could simply save gas by function ordering via Method ID. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because 22 gas are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. One could use [`This tool`](https://emn178.github.io/solidity-optimize-name/) to help find alternative function names with lower Method IDs while keeping the original name intact.

**Recommendation**: 
Find a lower method ID name for the most called functions for example ```mostCalled()``` vs. ```mostCalled_41q()``` is cheaper by 44 gas.


## [NAZ-G7] Upgrade To At Least 0.8.4
**Context**: [`All Contracts`](https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts)

**Description**:
Using newer compiler versions and the optimizer gives gas
optimizations and additional safety checks for free!

The advantages of versions =0.8.*= over =<0.8.0= are:

- Safemath by default from =0.8.0= (can be more gas efficient than /some/
  library based safemath).
- [`Low level inliner`](https://blog.soliditylang.org/2021/03/02/saving-gas-with-simple-inliner/) from =0.8.2=, leads to cheaper runtime gas.
  Especially relevant when the contract has small functions. For
  example, OpenZeppelin libraries typically have a lot of small
  helper functions and if they are not inlined, they cost an
  additional 20 to 40 gas because of 2 extra =jump= instructions and
  additional stack operations needed for function calls.
- [`Optimizer improvements in packed structs`](https://blog.soliditylang.org/2021/03/23/solidity-0.8.3-release-announcement/#optimizer-improvements): Before =0.8.3=, storing
  packed structs, in some cases used an additional storage read
  operation. After [`EIP-2929`](https://eips.ethereum.org/EIPS/eip-2929), if the slot was already cold, this
  means unnecessary stack operations and extra deploy time costs.
  However, if the slot was already warm, this means additional cost
  of =100= gas alongside the same unnecessary stack operations and
  extra deploy time costs.
- [`Custom errors`](https://blog.soliditylang.org/2021/04/21/custom-errors) from =0.8.4=, leads to cheaper deploy time cost and
  run time cost. Note: the run time cost is only relevant when the
  revert condition is met. In short, replace revert strings by
  custom errors.

**Recommendation**: 
Upgrade to at least 0.8.4 for the additional benefits.