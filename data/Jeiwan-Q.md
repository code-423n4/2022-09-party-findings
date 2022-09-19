# [L-01] Single-step multisig address transfer can break management of `Globals`
## Targets
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L27
## Description
In the case when multisig address is accidentally set to a wrong one, the management of `Globals` won't be possible. To
mitigate the risk of transferring multisig to an invalid address, consider implementing a two-step transition. For
example, [here's how OpenZeppelin implements 2-step ownership transferring](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/7a14f6c5953a1f2228280e6eb1dfee8e5c28d79a/contracts/access/Ownable2Step.sol#L35-L56).

# [L-02] False positive when minting CF token
## Targets
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L147
## Description
The `_mint` function doesn't fail if a token wasn't minted (when it already has an owner). While this situation is not
possible in the current implementation, returning successfully when a token wasn't minted can cause bugs in new versions of the contract.

# [N-01] Immutable variable re-definition
## Targets
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L87
    Duplicates `_GLOBAL` from `CrowdfundNFT`:
    https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L17
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L24
    Duplicates `_GLOBAL` from `PartyGovernance`:
    https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L194
## Description
The `_GLOBAL` immutable variable is defined in both parent and children contracts, which increases the cost of deployment and introduces confusion to the code.