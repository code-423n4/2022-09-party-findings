**Crowdfund.sol**
- L144/423/471 - You can save gas if instead of doing uint256() > 0 you do != 0.

- L180/242/300/348 - When we create a variable and we want it to have its base value, it is not necessary to set that value, since this would imply a double setting.

- L180/300 - When we are using a for and the length of the array is constantly consulted inside, it would be less expensive to create a variable in memory of the length of the array.

- L180/242/300/348/358 - When a uint256 value is impossible to overflow, such as in a for loop, it can become unchecked.


**BuyCrowdfund.sol**
- L62 - When we are using a for and the length of the array is constantly consulted inside, it would be less expensive to create a variable in memory of the length of the array.

- L62 - When a uint256 value is impossible to overflow, such as in a for loop, it can become unchecked.




**ArbitraryCallsProposal**
- L52/61/78 - When we create a variable and we want it to have its base value, it is not necessary to set that value, since this would imply a double setting.

- L52/61/78 - When we are using a for and the length of the array is constantly consulted inside, it would be less expensive to create a variable in memory of the length of the array.

- L52/61/78 - When a uint256 value is impossible to overflow, such as in a for loop, it can become unchecked.

- L137/139 - It is not necessary to create the hasPrecious variable, this generates an extra gas cost. Less gas could be wasted simply by returning the bool.




**TokenDistributor**
- L170/230 - When a uint256 value is impossible to overflow, such as in a for loop, it can become unchecked.

- L230 - When we create a variable and we want it to have its base value, it is not necessary to set that value, since this would imply a double setting.

- L230 - When we are using a for and the length of the array is constantly consulted inside, it would be less expensive to create a variable in memory of the length of the array.

- L381 - It is less expensive to make variable = variable - varA; which variable -= varA;


**PartyGoverance.sol**
- L306/432 - When we create a variable and we want it to have its base value, it is not necessary to set that value, since this would imply a double setting.

- L306 - When we are using a for and inside the length of the array is constantly consulted, it would be less expensive to create a variable in memory of the length of the array.

- L306 - When a uint256 value is impossible to overflow, such as in a for loop, it can become unchecked.

- L1062/1066 - It is not necessary to create a variable if it is only going to be used once, this saves gas expenses.


**LibProposal.sol**
- L14/32 - When we create a variable and we want it to have its base value, it is not necessary to set that value, since this would imply a double setting.

- L14/32 - When we are using a for and inside the length of the array is constantly consulted, it would be less expensive to create a variable in memory of the length of the array.

- L14/32 - When a uint256 value is impossible to overflow, such as in a for loop, it can become unchecked.


