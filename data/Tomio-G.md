1.
Title: Consider remove empty block

impact:
The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting.

Proof of Concept:
[Party.sol#L47](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L47)
[AuctionCrowdfund.sol#L144](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L144)
________________________________________________________________________

2.
Title: Expression for `constant` values such as a call to `keccak256()`, should use `immutable` rather than `constant`

Proof of Concept:
[ProposalStorage.sol#L19](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L19)

Recommended Mitigation Steps:
Change from `constant` to `immutable`
reference: [here](https://github.com/ethereum/solidity/issues/9232)
________________________________________________________________________

3.
Title: Comparison operators

Proof of Concept:
[ArbitraryCallsProposal.sol#L156](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L156)
[Crowdfund.sol#L423](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L423)

Recommended Mitigation Steps:
Change to:
```
	if (call.data.length > 3) {
```
Replace `<=` with `<`, and `>=` with `>` for gas optimization
________________________________________________________________________

4.
Title: abi.encode() is less efficient than abi.encodePacked()

Proof of Concept:
[ListOnZoraProposal.sol#L115](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L115)
________________________________________________________________________

5.
Title: `>=` is cheaper than `>`

Impact:
Strict inequalities (`>`) are more expensive than non-strict ones (`>=`). This is due to some supplementary checks (ISZERO, 3 gas)

Proof of Concept:
[TokenDistributor.sol#L167](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L167)

Recommended Mitigation Steps:
Consider using `>=` instead of `>` to avoid some opcodes
________________________________________________________________________

6.
Title: Using unchecked can save gas

Proof of Concept:
[TokenDistributor.sol#L167-L170](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L167-L170)

Recommended Mitigation Steps:
using `unchecked` can save gas (because `amountClaimed > remainingMemberSupply`)

________________________________________________________________________

7.
Title: Caching `length` for loop can save gas

Proof of Concept:
[TokenDistributor.sol#L239](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239)
[TokenDistributor.sol#L230](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230)
[ListOnOpenseaProposal.sol#L291](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291)
[crowdfund/Crowdfund.sol#L180](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180)

Recommended Mitigation Steps:
Change to:

``` 
	uint256 Length = infos.length;
	for (uint256 i = 0; i < Length; ++i) {
```
________________________________________________________________________

8.
Title: Using unchecked and prefix increment is more effective for gas saving:

Proof of Concept:
[TokenDistributor.sol#L239](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239)
[TokenDistributor.sol#L230](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230)
[ListOnOpenseaProposal.sol#L291](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291)
[crowdfund/Crowdfund.sol#L180](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180)

Recommended Mitigation Steps:
Change to:

```
	for (uint256 i = 0; i < infos.length;) {
	// ...
	unchecked { ++i; }
	}
```
________________________________________________________________________

9.
Title: Default value initialization

Impact:
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

Proof of Concept:
[TokenDistributor.sol#L239](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239)
[TokenDistributor.sol#L230](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230)
[ListOnOpenseaProposal.sol#L291](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291)
[crowdfund/Crowdfund.sol#L180](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180)
[PartyGovernance.sol#L432](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L432)

Recommended Mitigation Steps:
Remove explicit initialization for default values.
________________________________________________________________________

10.
Title: `calldata` instead of `memory` for RO function parameters

Impact:
If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory.

Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

Proof of Concept:
[BuyCrowdfundBase.sol#L70](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L70)

Recommended Mitigation Steps:
Replace `memory` with `calldata`
________________________________________________________________________

11.
Title: function _getFinalContribution(): L#358 should be unchecked due to L#350

Proof of Concept:
[Crowdfund.sol#L358](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L358)

Recommended Mitigation Steps:
Use `unchecked`
________________________________________________________________________

12.
Title: Gas optimization for increment or decrement

Proof of Concept:
Title: Cheaper to use `++` instead `+ 1`

Proof of Concept:
[PartyGovernance.sol#L440](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L440)

Recommended Mitigation Steps:
instead of using `+1` replace it with `++`

```
	low = ++mid;
```
________________________________________________________________________

13.
Title: Gas optimization to dividing by 2

Proof of Concept:
[PartyGovernance.sol#L434](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L434)

Recommended Mitigation Steps:
Replace `/ 2` with `>> 1`

Reference: [here](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md#g008---use-shift-rightleft-instead-of-divisionmultiplication-if-possible)
________________________________________________________________________

14.
Title: using delete statement can save gas

Proof of Concept:
[PartyGovernance.sol#L708](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L708)

Recommended Mitigation Steps:
Change to:

```
      delete proposalState.values.completedTime;
```
________________________________________________________________________

15.
Title: Gas savings for using solidity 0.8.10

Proof of Concept:
All contract in scope

Recommended Mitigation Steps:
Consider to upgrade pragma to at least 0.8.10.

Solidity 0.8.10 has a useful change which reduced gas costs of external calls
Reference: [here](https://blog.soliditylang.org/2021/11/09/solidity-0.8.10-release-announcement/)
______________________________________________________________________