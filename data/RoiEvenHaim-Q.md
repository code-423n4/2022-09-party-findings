# QA


### Unsafe ERC20 Operation(s)

### Examples:

```
party-contracts-c4\contracts\crowdfund\Crowdfund.sol::301 => preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
party-contracts-c4\contracts\party\PartyGovernanceNFT.sol::143 => super.transferFrom(owner, to, tokenId);
party-contracts-c4\contracts\proposals\FractionalizeProposal.sol::53 => data.token.approve(address(VAULT_FACTORY), data.tokenId);
party-contracts-c4\contracts\proposals\ListOnOpenseaProposal.sol::256 => token.approve(conduit, tokenId);
party-contracts-c4\contracts\proposals\ListOnOpenseaProposal.sol::367 => token.approve(address(0), tokenId);
party-contracts-c4\contracts\proposals\ListOnZoraProposal.sol::143 => token.approve(address(ZORA), tokenId);
party-contracts-c4\contracts\utils\LibERC20Compat.sol::11 => // Perform an `IERC20.transfer()` handling non-compliant implementations.
```
