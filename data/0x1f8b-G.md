- [Gas](#gas)
    - [**1. Use require instead of assert**](#1-use-require-instead-of-assert)
    - [**2. Don't use the length of an array for loops condition**](#2-dont-use-the-length-of-an-array-for-loops-condition)
    - [**3. There's no need to set default values for variables**](#3-theres-no-need-to-set-default-values-for-variables)
        - [Total gas saved: **8 * 15 = 120**](#total-gas-saved-8--15--120)
    - [**4. Avoid compound assignment operator in state variables**](#4-avoid-compound-assignment-operator-in-state-variables)
        - [Total gas saved: **13 * 18 = 234**](#total-gas-saved-13--18--234)
    - [**5. Shift right instead of dividing by 2**](#5-shift-right-instead-of-dividing-by-2)
        - [Total gas saved: **172 * 1 = 172**](#total-gas-saved-172--1--172)
    - [**6. Remove empty blocks**](#6-remove-empty-blocks)
    - [**7. Use calldata instead of memory**](#7-use-calldata-instead-of-memory)
    - [**8. ++i costs less gas compared to i++ or i += 1**](#8-i-costs-less-gas-compared-to-i-or-i--1)
    - [**9. Use Custom Errors instead of Revert Strings to save Gas**](#9-use-custom-errors-instead-of-revert-strings-to-save-gas)
        - [Total gas saved: **9 * 28 = 252**](#total-gas-saved-9--28--252)
    - [**10. Avoid unused returns**](#10-avoid-unused-returns)
    - [**11. constants expressions are expressions, not constants**](#11-constants-expressions-are-expressions-not-constants)
    - [**12. Gas saving using immutable**](#12-gas-saving-using-immutable)
    - [**13. Use the unchecked keyword**](#13-use-the-unchecked-keyword)
    - [**14. Optimize CollectionBuyCrowdfund.onlyHost**](#14-optimize-collectionbuycrowdfundonlyhost)

# Gas

## **1. Use `require` instead of `assert`**

The `assert()` and `require()` functions are a part of the error handling aspect in Solidity. Solidity makes use of state-reverting error handling exceptions. This means all changes made to the contract on that call or any sub-calls are undone if an error is thrown. It also flags an error.

They are quite similar as both check for conditions and if they are not met, would throw an error.

The big difference between the two is that the `assert()` function when false, **uses up all the remaining gas and reverts all the changes made.**

Meanwhile, a `require()` function when false, also reverts back all the changes made to the contract but **does refund all the remaining gas fees we offered to pay**. This is the most common Solidity function used by developers for debugging and error handling.

**Affected source code:**

- [CrowdfundNFT.sol:166](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundNFT.sol#L166)
- [TokenDistributor.sol:385](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L385)
- [TokenDistributor.sol:411](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L411)
- [PartyGovernance.sol:504](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L504)
- [PartyGovernanceNFT.sol:179](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L179)
- [FractionalizeProposal.sol:67](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/FractionalizeProposal.sol#L67)
- [ListOnOpenseaProposal.sol:221](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L221)
- [ListOnOpenseaProposal.sol:302](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L302)
- [ListOnZoraProposal.sol:120](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L120)
- [ProposalExecutionEngine.sol:142](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L142)
- [ReadOnlyDelegateCall.sol:30](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L30)

## **2. Don't use the length of an array for loops condition**

It's cheaper to store the length of the array inside a local variable and iterate over it.

**Affected source code:**

- [CollectionBuyCrowdfund.sol:62](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62)
- [Crowdfund.sol:180](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L180)
- [Crowdfund.sol:300](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L300)
- [TokenDistributor.sol:230](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L230)
- [TokenDistributor.sol:239](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L239)
- [ArbitraryCallsProposal.sol:52](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52)
- [ArbitraryCallsProposal.sol:61](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61)
- [ArbitraryCallsProposal.sol:78](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L78)
- [LibProposal.sol:14](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14)
- [LibProposal.sol:32](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L32)
- [ListOnOpenseaProposal.sol:291](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291)
- [ERC1155.sol:80](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L80)
- [ERC1155.sol:118](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L118)

## **3. There's no need to set default values for variables**

If a variable is not set/initialized, the default value is assumed (0, `false`, 0x0 ... depending on the data type). You are simply wasting gas if you directly initialize it with its default value.

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.15;

contract TesterA {
function testInit() public view returns (uint) { uint a = 0; return a; }
}

contract TesterB {
function testNoInit() public view returns (uint) { uint a; return a; }
}
```

Gas saving executing: **8 per entry**

```
TesterA.testInit:   21392
TesterB.testNoInit: 21384   
```

**Affected source code:**

- [CollectionBuyCrowdfund.sol:62](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62)
- [Crowdfund.sol:180](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L180)
- [Crowdfund.sol:300](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L300)
- [TokenDistributor.sol:230](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L230)
- [TokenDistributor.sol:239](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L239)
- [ArbitraryCallsProposal.sol:52](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52)
- [ArbitraryCallsProposal.sol:61](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61)
- [ArbitraryCallsProposal.sol:78](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L78)
- [LibProposal.sol:14](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14)
- [LibProposal.sol:32](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L32)
- [ListOnOpenseaProposal.sol:291](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291)
- [ERC1155.sol:80](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L80)
- [ERC1155.sol:118](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L118)
- [PartyGovernance.sol:306](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306)
- [PartyGovernance.sol:432](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L432)

### Total gas saved: **8 * 15 = 120**

## **4. Avoid compound assignment operator in state variables**

Using compound assignment operators for state variables (like `State += X` or `State -= X` ...) it's more expensive than using operator assignment (like `State = State + X` or `State = State - X` ...).

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.15;

contract TesterA {
uint private _a;
function testShort() public {
_a += 1;
}
}

contract TesterB {
uint private _a;
function testLong() public {
_a = _a + 1;
}
}
```

Gas saving executing: **13 per entry**

```
TesterA.testShort: 43507
TesterB.testLong:  43494
```

**Affected source code:**

- [Crowdfund.sol:411](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L411)
- [TokenDistributor.sol:381](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L381)
- [ERC1155.sol:51](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L51)
- [ERC1155.sol:52](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L52)
- [ERC1155.sol:84](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L84)
- [ERC1155.sol:85](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L85)
- [ERC1155.sol:145](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L145)
- [ERC1155.sol:169](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L169)
- [ERC1155.sol:199](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L199)
- [ERC1155.sol:216](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L216)
- [ERC20.sol:76](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L76)
- [ERC20.sol:81](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L81)
- [ERC20.sol:98](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L98)
- [ERC20.sol:103](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L103)
- [ERC20.sol:183](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L183)
- [ERC20.sol:188](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L188)
- [ERC20.sol:195](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L195)
- [ERC20.sol:200](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L200)

### Total gas saved: **13 * 18 = 234**

## **5. Shift right instead of dividing by 2**

Shifting one to the right will calculate a division by two.

he `SHR` opcode only requires 3 gas, compared to the `DIV` opcode's consumption of 5. Additionally, shifting is used to get around Solidity's division operation's division-by-0 prohibition.

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.15;

contract TesterA {
function testDiv(uint a) public returns (uint) { return a / 2; }
}

contract TesterB {
function testShift(uint a) public returns (uint) { return a >> 1; }
}
```

Gas saving executing: **172 per entry**

```
TesterA.testDiv:    21965
TesterB.testShift:  21793   
```

**Affected source code:**

- [PartyGovernance.sol:434](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L434)

### Total gas saved: **172 * 1 = 172**

## **6. Remove empty blocks**

An empty block or an empty method generally indicates a lack of logic that it’s unnecessary and should be eliminated, call a method that literally does nothing only consumes gas unnecessarily, if it is a `virtual` method which is expects it to be filled by the class that implements it, it is better to use `abstract`  contracts with just the definition.

**Sample code:**

```javascript
pragma solidity >=0.4.0 <0.7.0;

abstract contract BaseContract {
    function totalSupply() public virtual returns (uint256);
}
```

**Reference:**
- https://docs.soliditylang.org/en/v0.6.0/contracts.html?highlight=abstract#abstract-contracts

**Affected source code:**

- [AuctionCrowdfund.sol:104](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L104)
- [AuctionCrowdfund.sol:144](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L144)
- [BuyCrowdfund.sol:62](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfund.sol#L62)
- [BuyCrowdfundBase.sol:67](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfundBase.sol#L67)
- [CollectionBuyCrowdfund.sol:77](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L77)
- [CrowdfundNFT.sol:49-52](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundNFT.sol#L49-L52)
- [CrowdfundNFT.sol:56-59](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundNFT.sol#L56-L59)
- [CrowdfundNFT.sol:63-66](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundNFT.sol#L63-L66)
- [CrowdfundNFT.sol:70-73](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundNFT.sol#L70-L73)
- [CrowdfundNFT.sol:77-80](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundNFT.sol#L77-L80)
- [Party.sol:28](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/Party.sol#L28)
- [Party.sol:47](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/Party.sol#L47)
- [ProposalExecutionEngine.sol:99-103](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L99-L103)

## **7. Use `calldata` instead of `memory`**

Some methods are declared as `external` but the arguments are defined as `memory` instead of as `calldata`.

By marking the function as `external` it is possible to use `calldata` in the arguments shown below and save significant gas.

**Recommended change:**

```diff
-   function initialize(AuctionCrowdfundOptions memory opts)
+   function initialize(AuctionCrowdfundOptions calldata opts)
        external
        payable
        onlyConstructor
    {
    ...
    }
```

**Affected source code:**

- [AuctionCrowdfund.sol:110-141](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L110-L141)
- [AuctionCrowdfund.sol:196-259](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L196-L259)
- [BuyCrowdfund.sol:68-88](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfund.sol#L68-L88)
- [CollectionBuyCrowdfund.sol:83-102](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L83-L102)
- [Party.sol:33-45](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/Party.sol#L33-L45)
- [ProposalExecutionEngine.sol:116-180](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L116-L180)

## **8. `++i` costs less gas compared to `i++` or `i += 1`**

`++i` costs less gas compared to `i++` or `i += 1` for unsigned integers, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.

`i++` increments `i` and returns the initial value of `i`. Which means:

```solidity
uint i = 1;
i++; // == 1 but i == 2
```

But `++i` returns the actual incremented value:

```solidity
uint i = 1;
++i; // == 2 and i == 2 too, so no need for a temporary variable
```

In the first case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`
I suggest using `++i` instead of `i++` to increment the value of an uint variable. Same thing for `--i` and `i--`

*Keep in mind that this change can only be made when we are not interested in the value returned by the operation, since the result is different, you only have to apply it when you only want to increase a counter.*

**Affected source code:**

- [CollectionBuyCrowdfund.sol:62](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62)
- [ERC20.sol:142](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L142)
- [ERC721.sol:98](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L98)
- [ERC721.sol:100](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L100)
- [ERC721.sol:163](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L163)
- [ERC721.sol:178](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L178)

## **9. Use Custom Errors instead of Revert Strings to save Gas**

Custom errors from Solidity `0.8.4` are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)

**Source Custom Errors in Solidity:**

Starting from Solidity `v0.8.4`, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.15;

contract TesterA {
function testRevert(bool path) public view {
 require(path, "test error");
}
}

contract TesterB {
error MyError(string msg);
function testError(bool path) public view {
 if(path) revert MyError("test error");
}
}
```

Gas saving executing: **9 per entry**

```
TesterA.testRevert: 21611
TesterB.testError:  21602     
```

**Affected source code:**

- [ProposalExecutionEngine.sol:246](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L246)
- [ProposalExecutionEngine.sol:247](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L247)
- [ReadOnlyDelegateCall.sol:20](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L20)
- [ERC1155.sol:49](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L49)
- [ERC1155.sol:56-62](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L56-L62)
- [ERC1155.sol:72](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L72)
- [ERC1155.sol:74](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L74)
- [ERC1155.sol:96-102](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L96-L102)
- [ERC1155.sol:111](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L111)
- [ERC1155.sol:149-155](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L149-L155)
- [ERC1155.sol:166](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L166)
- [ERC1155.sol:180-186](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L180-L186)
- [ERC1155.sol:196](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L196)
- [ERC20.sol:124](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L124)
- [ERC20.sol:153](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L153)
- [ERC721.sol:35](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L35)
- [ERC721.sol:39](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L39)
- [ERC721.sol:68](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L68)
- [ERC721.sol:86](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L86)
- [ERC721.sol:88](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L88)
- [ERC721.sol:90-93](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L90-L93)
- [ERC721.sol:117-122](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L117-L122)
- [ERC721.sol:133-138](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L133-L138)
- [ERC721.sol:157](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L157)
- [ERC721.sol:159](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L159)
- [ERC721.sol:174](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L174)
- [ERC721.sol:195-200](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L195-L200)
- [ERC721.sol:210-215](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L210-L215)

### Total gas saved: **9 * 28 = 252**

## **10. Avoid unused returns**

Having a method that always returns the same value is not correct in terms of consumption, if you want to modify a value, and the method will perform a `revert` in case of failure, it is not necessary to return a `true`, since it will never be `false`. It is less expensive not to return anything, and it also eliminates the need to double-check the returned value by the caller.

**Affected source code:**

- [ERC20.sol:67-73](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L67-L73)
- [ERC20.sol:75-87](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L75-L87)
- [ERC20.sol:89-109](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L89-L109)

## **11. `constants` expressions are expressions, not `constants`**

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.

If the variable was immutable instead: the calculation would only be done once at deploy time (in the constructor), and then the result would be saved and read directly at runtime rather than being recalculated.

**Reference:**

- https://github.com/ethereum/solidity/issues/9232

> Consequences: each usage of a "constant" costs ~100gas more on each access (it is still a little better than storing the result in storage, but not much..). since these are not real constants, they can't be referenced from a real constant environment (e.g. from assembly, or from another library).

**Affected source code:**

- [ProposalStorage.sol:19](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalStorage.sol#L19)
- [ProposalExecutionEngine.sol:80](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L80)

## **12. Gas saving using `immutable`**

It's possible to avoid storage access a save gas using `immutable` keyword for the following variables:

It's also better to remove the initial values, because they will be set during the constructor.

**Affected source code:**

- [ERC721.sol:20](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L20)
- [ERC721.sol:22](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC721.sol#L22)
- [ERC20.sol:20](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L20)
- [ERC20.sol:22](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC20.sol#L22)

## **13. Use the `unchecked` keyword**

When an underflow or overflow cannot occur, one might conserve gas by using the `unchecked` keyword to prevent unnecessary arithmetic underflow/overflow tests.

```diff
    function safeTransferFrom(
        address from,
        address to,
        uint256 id,
        uint256 amount,
        bytes calldata data
    ) public virtual {
        require(msg.sender == from || isApprovedForAll[from][msg.sender], "NOT_AUTHORIZED");

        balanceOf[from][id] -= amount;
-       balanceOf[to][id] += amount;
+       unchecked {
+           balanceOf[to][id] += amount;
+       }   

        emit TransferSingle(msg.sender, from, to, id, amount);

        require(
            to.code.length == 0
                ? to != address(0)
                : ERC1155TokenReceiverBase(to).onERC1155Received(msg.sender, from, id, amount, data) ==
                    ERC1155TokenReceiverBase.onERC1155Received.selector,
            "UNSAFE_RECIPIENT"
        );
    }
```

**Affected source code:**

- [ERC1155.sol:52](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L52)
- [ERC1155.sol:85](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/vendor/solmate/ERC1155.sol#L85)

## **14. Optimize `CollectionBuyCrowdfund.onlyHost`**

It's possible to remove the variable `isHost` like this:

```diff
    modifier onlyHost(address[] memory hosts) {
-       bool isHost;
        for (uint256 i; i < hosts.length; i++) {
            if (hosts[i] == msg.sender) {
-               isHost = true;
+               _;
+               return;
-               break;
            }
        }

+       revert OnlyPartyHostError();
-       if (!isHost) {
-           revert OnlyPartyHostError();
-       }
-       _;
    }
```

**Affected source code:**

- [CollectionBuyCrowdfund.sol:60-74](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L60-L74)
