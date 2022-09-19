Findings: ++I COSTS LESS GAS COMPARED TO I++ OR I += 1 (SAME FOR --I VS I-- OR I -= 1)

Code: contracts/CollectionBuyCrowfund.sol - line 632
Contracts/Crowdfund.sol - line 180

Explanation:

Pre-increments and pre-decrements are cheaper.

For a uint256 i variable, the following is true with the Optimizer enabled at 10k:

Increment:

i += 1 is the most expensive form
i++ costs 6 gas less than i += 1
++i costs 5 gas less than i++ (11 gas less than i += 1)


-------------------

Findings:  AN ARRAY’S LENGTH SHOULD BE CACHED TO SAVE GAS IN FOR-LOOPS

Code: contracts/CollectionBuyCrowfund.sol - line 62
Contracts/Crowdfund.sol - line 180, line 300
Contracts/TokenDistributor.sol - 230
Contracts/PartyGovernance.sol - 306

Explanation:

Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.
Caching the array length in the stack saves around 3 gas per iteration.

-------------------

Findings:  It costs more gas to initialize non-constant/non-immutable variables to zero than to let the default of zero be applied

Code: Contracts/Crowdfund.sol - line 247, line 307, line 356
Contracts/TokenDistributor.sol - line 230

Explanation:

Not overwriting the default for stack variables saves 8 gas. Storage and memory variables have larger savings

-------------------


Findings:  X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES (or is not a state variable)

Code: contracts/Crowdfund.sol - line 427

Explanation:

-------------------

Findings: MULTIPLICATION/DIVISION BY TWO SHOULD USE BIT SHIFTING

Code: contracts/PartyGovernance.sol - line 434

Explanation: <x> * 2 is equivalent to <x> << 1 and <x> / 2 is the same as <x> >> 1. The MUL and DIV opcodes cost 5 gas, whereas SHL and SHR only cost 3 gas