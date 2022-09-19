## Low

## [L-1] Use safeTransfer/safeTransferFrom consistently instead of transfer/transferFrom
It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

Reference: This [similar medium-severity finding](https://consensys.net/diligence/audits/2021/01/fei-protocol/#unchecked-return-value-for-iweth-transfer-call) from Consensys Diligence Audit of Fei Protocol.
```solidity
crowdfund/CrowdfundNFT.sol:49:    function transferFrom(address, address, uint256)
crowdfund/Crowdfund.sol:301:            preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
distribution/TokenDistributor.sol:371:    function _transfer(
party/PartyGovernanceNFT.sol:136:    function transferFrom(address owner, address to, uint256 tokenId)
party/PartyGovernanceNFT.sol:143:        super.transferFrom(owner, to, tokenId);
tokens/IERC721.sol:10:    function transferFrom(address from, address to, uint256 tokenId) external;
tokens/IERC20.sol:9:    function transfer(address to, uint256 amount) external returns (bool);
tokens/IERC20.sol:10:    function transferFrom(address from, address to, uint256 amount) external returns (bool);
```

## [L-2] USE SAFEERC20.SAFEAPPROVE INSTEAD OF APPROVE
This is probably an oversight since SafeERC20 was imported and safeTransfer() was used for ERC20 token transfers. Nevertheless, note that approve() will fail for certain token implementations that do not return a boolean value (). Hence it is recommend to use safeApprove().
```solidity
crowdfund/CrowdfundNFT.sol:70:    function approve(address, uint256)
proposals/ListOnZoraProposal.sol:143:        token.approve(address(ZORA), tokenId);
proposals/ListOnOpenseaProposal.sol:256:        token.approve(conduit, tokenId);
proposals/ListOnOpenseaProposal.sol:367:            token.approve(address(0), tokenId);
proposals/FractionalizeProposal.sol:53:        data.token.approve(address(VAULT_FACTORY), data.tokenId);
proposals/ArbitraryCallsProposal.sol:173:                if (selector == IERC721.approve.selector) {
tokens/IERC721.sol:13:    function approve(address operator, uint256 tokenId) external;
tokens/IERC20.sol:11:    function approve(address spender, uint256 allowance) external returns (bool);
```
## Non critical 
## [N-1] `require()`/`revert()` statements should have descriptive reason strings
```solidity
utils/ReadOnlyDelegateCall.sol:20:        require(msg.sender == address(this));
proposals/ProposalExecutionEngine.sol:246:        require(proposalType != ProposalType.Invalid);
proposals/ProposalExecutionEngine.sol:247:        require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));
```


