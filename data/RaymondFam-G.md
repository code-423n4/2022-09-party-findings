## calldata and memory
When running a function we could pass the function parameters as calldata or memory for variables such as strings, structs, arrays etc. If we are not modifying the passed parameter we should pass it as calldata because calldata is more gas efficient than memory. Here are some of the instances entailed:

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L50-L54
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L33

## No Need to Initialize Variables with Default Values
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). If you explicitly initialize it with its default value, you will be incurring more gas. Hence, in the for loop of the following instances, refrain from doing i = 0. Here are some of the instances entailed:

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78

## Non-strict inequalities are cheaper than strict ones
In the EVM, there is no opcode for non-strict inequalities (>=, <=) and two operations are performed (> + =.) Consider replacing >= with the strict counterpart >. For instance, the following line of code could be rewritten as:

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L156

```
if (call.data.length > 3)
```
## Function Order Affects Gas Consumption
The order of function will also have an impact on gas consumption. Because in smart contracts, there is a difference in the order of the functions. Each position will have an extra 22 gas. The order is dependent on method ID. So, if you rename the frequently accessed function to more early method ID, you can save gas cost. Please visit the following site for further information:

https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

## Activate the Optimizer
Before deploying your contract, activate the optimizer when compiling using “solc --optimize --bin sourceFile.sol”. By default, the optimizer will optimize the contract assuming it is called 200 times across its lifetime. If you want the initial contract deployment to be cheaper and the later function executions to be more expensive, set it to “ --optimize-runs=1”. Conversely, if you expect many transactions and do not care for higher deployment cost and output size, set “--optimize-runs” to a high number. Please visit the following site for further information:

https://docs.soliditylang.org/en/v0.5.4/using-the-compiler.html#using-the-commandline-compiler

## Uncheck SafeMath
"Checked" math, which is default in 0.8.0 is not free. The compiler will add some overflow checks, somehow similar to those implemented by `SafeMath`. While it is reasonable to expect these checks to be less expensive than the current `SafeMath`, one should keep in mind that these checks will increase the cost of "basic math operation" that were not previously covered. This particularly concerns variable increments in for loops. Considering no arithmetic overflow/underflow is going to happen in the for loop instance below, `unchecked { ++i ;}` to use the previous wrapping behavior further saves gas:

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L306-L308

```
        for (uint256 i=0; i < opts.hosts.length;) {
            isHost[opts.hosts[i]] = true;

            unchecked {
                ++i;
            }
        }
```
## += Costs More Gas
`+=` generally costs more gas than writing out the assigned equation explicitly. As an example, the following line of code could be rewritten as: 

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L595

```
        values.votes = values.votes + votingPower;
```

## Assert Costs More Gas Than Require
The `assert()` function when false, uses up all the remaining gas and reverts all the changes made. On the other hand, a `require()` function when false, also reverts back all the changes made to the contract but does refund all the remaining gas fees we offered to pay.

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L221

On a side note, the assert function should only be used to examine invariants and test for internal problems. When used correctly, it can assess your contract and discover the conditions and function calls that will result in a failed assert. A properly running program should never reach a failing assert statement; if this occurs, there is a flaw in your contract that has to be addressed.

## Private Function Embedded Modifier to Reduce Contract Size
Consider having the logic of a modifier embedded through an internal or private function to reduce contract size if need be. For instance, the following instance of modifier may be rewritten as follows:

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L249-L255

```
    function _onlyPartyDaoOrHost() private view {
        address partyDao = _GLOBALS.getAddress(LibGlobals.GLOBAL_DAO_WALLET);
        if (msg.sender != partyDao && !isHost[msg.sender]) {
            revert OnlyPartyDaoOrHostError(msg.sender, partyDao);
        }
    }

    modifier onlyPartyDaoOrHost() {
        _onlyPartyDaoOrHost();
        _;
    }
```
## ++i costs less gas compared to i++
++i costs less gas compared to i++ or i += 1 for unsigned integers considering the pre-increment operation is cheaper (about 5 GAS per iteration).

i++ increments i and makes the compiler create a temporary variable for returning the initial value of i. In contrast, ++i returns the actual incremented value without making the compiler do extra job.

Here's one instance entailed:

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62