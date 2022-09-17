## The receive function is not implemented

```
contracts/crowdfund/AuctionCrowdfund.sol:
  143      /// @notice Accept naked ETH, e.g., if an auction needs to return ETH to us.
  144:     receive() external payable {}
  145  

contracts/party/Party.sol:
  46  
  47:     receive() external payable {}
  48  }

```



## open todo
```
contracts/renderers/CrowdfundNFTRenderer.sol:
  33              '" class="',
  34:             // TODO: Parameterize
  35              baseStyle,

contracts/renderers/DummyERC721Renderer.sol:
  7      function tokenURI(uint256 tokenId) external view returns (string memory) {
  8:         // TODO: make this human readable
  9          return string(abi.encode(address(this), tokenId));

contracts/renderers/PartyGovernanceNFTRenderer.sol:
  55              '" class="',
  56:             // TODO: Parameterize
  57              baseStyle,

  86      function renderVotingPowerAndDistributionShare(uint256 tokenId) internal view returns (string memory) {
  87:         // TODO: Write decimal string library
  88          uint256 votingPower = votingPowerByTokenId[tokenId] * 1e2 / _governanceValues.totalVotingPower;
```