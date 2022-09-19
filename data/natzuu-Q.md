## **L001 - Unsafe ERC20 Operation(s)**

### **Description**

ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard.

It is therefore recommended to always either use OpenZeppelin's `SafeERC20` library or at least to wrap each operation in a `require` statement.

To circumvent ERC20's `approve` functions race-condition vulnerability use OpenZeppelin's `SafeERC20` library's `safe{Increase|Decrease}Allowance` functions.

In case the vulnerability is of no danger for your implementation, provide enough documentation explaining the reasonings.

```solidity
preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L301](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L301)

```solidity
super.transferFrom(owner, to, tokenId);
```

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L143](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L143)

### **Background Information**

- [OpenZeppelin's IERC20 documentation](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/IERC20.sol#L43)