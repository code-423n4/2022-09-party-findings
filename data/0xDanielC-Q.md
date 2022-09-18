# Unbounded loops may Prevent ERC721 Transfers
The unbounded array has no bounds on the amount of preciousTokens transferred to address(_party), therefore if the loop gets too large, the gas limit could be exceeded, preventing ERC721 transfers to the new _party. 

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol?#L300-L302

# Mitigation
Consider adding an upper bound to the number of assets transferred to the new party. 