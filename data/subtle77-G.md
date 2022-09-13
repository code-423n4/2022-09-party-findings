[G-03] ++i costs less gas compared to i++ or i += 1 in for loops (~5 gas per iteration)

++i costs less gas compared to i++ or i += 1 for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.

i++ increments i and returns the initial value of i. Which means:

uint i = 1;  
i++; // == 1 but i == 2  

But ++i returns the actual incremented value:

uint i = 1;  
++i; // == 2 and i == 2 too, so no need for a temporary variable  

In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2

Instance include:
 File: CollectionBuyCrowdfund.sol line 62

for (uint256 i; i < hosts.length; i++) {
            if (hosts[i] == msg.sender) {
                isHost = true;
                break;
            }

