## 1. Use a more recent version of solidity


- Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath  
- Use a solidity version of at least 0.8.2 to get compiler automatic inlining  
- Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads  
- Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()/require()` strings  
- Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value
```
All contracts
```
## 2. `++i` costs less gas compared to `i++` or `i += 1`, same for `--i/i--`. Especially in for loops


`++i` costs less gas compared to `i++` or `i += 1` for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.

`i++` increments i and returns the initial value of `i`.

```
uint i = 1;  
i++; // == 1 but i == 2
```
But ++i returns the actual incremented value:
```
uint i = 1;  
++i; // == 2 and i == 2 too, so no need for a temporary variable  
```
In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2

I suggest using ++i instead of i++ to increment the value of an uint variable.

If done inside for loop, saves 6 gas per loop.

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol
```
62		for (uint256 i; i < hosts.length; i++) {
```

## 3. No need to explicitly initialize variables with default values


If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: `for (uint256 i = 0; i < hadPreciouses.length; ++i) {` should be replaced with for `(uint256 i; i < hadPreciouses.length; ++i) {`

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol
```
52		for (uint256 i =  0; i < hadPreciouses.length; ++i) {
61		for (uint256 i =  0; i < calls.length; ++i) {
78		for (uint256 i =  0; i < hadPreciouses.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol
```
230		for (uint256 i =  0; i < infos.length; ++i) {
239		for (uint256 i =  0; i < infos.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol
```
291		for (uint256 i =  0; i < fees.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol
```
180		for (uint256 i =  0; i < contributors.length; ++i) {
242		for (uint256 i =  0; i < numContributions; ++i) {
300		for (uint256 i =  0; i < preciousTokens.length; ++i) {
348		for (uint256 i =  0; i < numContributions; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol
```
14		for (uint256 i =  0; i < preciousTokens.length; ++i) {
32		for (uint256 i =  0; i < preciousTokens.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol
```
306		for (uint256 i=0; i < opts.hosts.length; ++i) {
432		uint256 low =  0;
```

## 4. Increments can be unchecked


In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline. This saves 30-40 gas PER LOOP.

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol
```
52		for (uint256 i =  0; i < hadPreciouses.length; ++i) {
61		for (uint256 i =  0; i < calls.length; ++i) {
78		for (uint256 i =  0; i < hadPreciouses.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol
```
230		for (uint256 i =  0; i < infos.length; ++i) {
239		for (uint256 i =  0; i < infos.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol
```
306		for (uint256 i=0; i < opts.hosts.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol
```
291		for (uint256 i =  0; i < fees.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol
```
180		for (uint256 i =  0; i < contributors.length; ++i) {
242		for (uint256 i =  0; i < numContributions; ++i) {
300		for (uint256 i =  0; i < preciousTokens.length; ++i) {
348		for (uint256 i =  0; i < numContributions; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol
```
14		for (uint256 i =  0; i < preciousTokens.length; ++i) {
32		for (uint256 i =  0; i < preciousTokens.length; ++i) {
```

## 5. \<array>.length should not be looked up in every loop of a for-loop


The overheads outlined below are PER LOOP, excluding the first loop

- storage arrays incur a Gwarmaccess (100 gas)
- memory arrays use MLOAD (3 gas)
- calldata arrays use CALLDATALOAD (3 gas)

Caching the length changes each of these to a DUP\<N> (3 gas), and gets rid of the extra DUP\<N> needed to store the stack offset

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol
```
52		for (uint256 i =  0; i < hadPreciouses.length; ++i) {
61		for (uint256 i =  0; i < calls.length; ++i) {
78		for (uint256 i =  0; i < hadPreciouses.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol
```
230		for (uint256 i =  0; i < infos.length; ++i) {
239		for (uint256 i =  0; i < infos.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol
```
306		for (uint256 i=0; i < opts.hosts.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol
```
291		for (uint256 i =  0; i < fees.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol
```
180		for (uint256 i =  0; i < contributors.length; ++i) {
300		for (uint256 i =  0; i < preciousTokens.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol
```
14		for (uint256 i =  0; i < preciousTokens.length; ++i) {
32		for (uint256 i =  0; i < preciousTokens.length; ++i) {
```

## 6. Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead


When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

These can be found in most contracts that have variable assignments. Whenever possible consider using uint256 instead of smaller uint types.

Here is just a couple of examples:
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L30
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L39
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L51
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L94

## 7. `abi.encode()` is less efficient than `abi.encodepacked()`


Whenever possible consider using `abi.encodepacked()` to save gas.

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol
```
115		return  abi.encode(ZoraStep.ListedOnZora, ZoraProgressData({
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol
```
164		return  abi.encode(ListOnOpenseaStep.ListedOnZora, ZoraProgressData({
219		return  abi.encode(ListOnOpenseaStep.ListedOnOpenSea, orderHash, expiry);
```

## 8.  Using bools for storage incurs overhead


// Booleans are more expensive than uint256 or any type that takes up a full
// word because each write operation emits an extra SLOAD to first read the
// slot's contents, replace the bits taken up by the boolean, and then write
// back. This is the compiler's defense against contract upgrades and 
// pointer aliasing, and it cannot be disabled.

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27

Use uint256(1) and uint256(2) for true/false

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol
```
12		mapping(uint256  =>  mapping(bytes32  =>  bool)) private _includedWordValues;
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol
```
30		mapping(uint256  =>  bool) hasPartyTokenClaimed;
62		bool  public emergencyActionsDisabled;
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol
```
106		bool  private _splitRecipientHasBurned;
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol
```
148		mapping (address  =>  bool) hasVoted;
197		bool  public emergencyExecuteDisabled;
207		mapping(address  =>  bool) public isHost;
```

## 9. Not using the named return variables when a function returns, wastes deployment gas

This can be found in most contracts that have return functions.

Some examples:
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L32
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L36
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L40
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L99
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L72
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L181
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L193