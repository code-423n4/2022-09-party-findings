# PARTY-CONTRACTS-C4

## Low Risk and Non-Critical Issues

### Adding a `return` statement when the function defines a named return variable, is redundant

_There are **3** instances of this issue:_

```solidity
File: contracts/party/PartyGovernance.sol

848:  function _getVotingPowerSnapshotAt(address voter, uint40 timestamp, uint256 hintIndex)
864:              return snaps[hintIndex];

848:  function _getVotingPowerSnapshotAt(address voter, uint40 timestamp, uint256 hintIndex)
871:              return snaps[hintIndex];

848:  function _getVotingPowerSnapshotAt(address voter, uint40 timestamp, uint256 hintIndex)
876:      return snap;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

### Events not emmited on important state changes

Emmiting events is recommended each time when a state variable's value is being changed or just some critical event for the contract has occurred. It also helps off-chain monitoring of the contract's state.

_There are **12** instances of this issue:_

```solidity
File: contracts/crowdfund/AuctionCrowdfund.sol

110:  function initialize(AuctionCrowdfundOptions memory opts)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol

```solidity
File: contracts/crowdfund/BuyCrowdfund.sol

68:   function initialize(BuyCrowdfundOptions memory opts)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol

```solidity
File: contracts/crowdfund/BuyCrowdfundBase.sol

70:   function _initialize(BuyCrowdfundBaseOptions memory opts)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol

```solidity
File: contracts/crowdfund/CollectionBuyCrowdfund.sol

83:   function initialize(CollectionBuyCrowdfundOptions memory opts)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol

```solidity
File: contracts/crowdfund/Crowdfund.sol

124:  function _initialize(CrowdfundOptions memory opts)

262:  function _createParty(
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

```solidity
File: contracts/crowdfund/CrowdfundNFT.sol

39:   function _initialize(string memory name_, string memory symbol_)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol

```solidity
File: contracts/distribution/TokenDistributor.sol

327:  function disableEmergencyActions() onlyPartyDao external {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol

```solidity
File: contracts/globals/Globals.sol

27:   function transferMultiSig(address newMultiSig) external onlyMultisig {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol

```solidity
File: contracts/party/PartyGovernance.sol

800:  function disableEmergencyExecute() external onlyPartyDaoOrHost onlyDelegateCall {

1082: function _setPreciousList(
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

```solidity
File: contracts/party/PartyGovernanceNFT.sol

49:   function _initialize(
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol

### `public` functions not called by the contract should be declared `external` instead

_There are **6** instances of this issue:_

```solidity
File: contracts/crowdfund/CrowdfundFactory.sol

35:   function createBuyCrowdfund(

61:   function createAuctionCrowdfund(

87:   function createCollectionBuyCrowdfund(
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol

```solidity
File: contracts/party/PartyGovernanceNFT.sol

88:   function tokenURI(uint256) public override view returns (string memory) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol

```solidity
File: contracts/tokens/ERC721Receiver.sol

14:   function onERC721Received(address, address, uint256, bytes memory)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC721Receiver.sol

```solidity
File: contracts/utils/EIP165.sol

9:    function supportsInterface(bytes4 interfaceId)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/EIP165.sol

### Empty `receive()`/`fallback()` functions

_There are **4** instances of this issue:_

```solidity
File: contracts/crowdfund/AuctionCrowdfund.sol

144:  receive() external payable {}
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol

```solidity
File: contracts/party/Party.sol

47:   receive() external payable {}
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol

```solidity
File: contracts/party/PartyGovernance.sol

314:  fallback() external {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

```solidity
File: contracts/utils/Proxy.sol

25:   fallback() external payable {
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol

### Non-library/interface files should use fixed compiler versions, not floating ones

_There are **27** instances of this issue:_

```solidity
File: contracts/crowdfund/AuctionCrowdfund.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol

```solidity
File: contracts/crowdfund/BuyCrowdfund.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol

```solidity
File: contracts/crowdfund/BuyCrowdfundBase.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol

```solidity
File: contracts/crowdfund/CollectionBuyCrowdfund.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol

```solidity
File: contracts/crowdfund/Crowdfund.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

```solidity
File: contracts/crowdfund/CrowdfundFactory.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol

```solidity
File: contracts/crowdfund/CrowdfundNFT.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol

```solidity
File: contracts/distribution/TokenDistributor.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol

```solidity
File: contracts/globals/Globals.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol

```solidity
File: contracts/market-wrapper/IMarketWrapper.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/market-wrapper/IMarketWrapper.sol

```solidity
File: contracts/party/IPartyFactory.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/IPartyFactory.sol

```solidity
File: contracts/party/Party.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol

```solidity
File: contracts/party/PartyFactory.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol

```solidity
File: contracts/party/PartyGovernance.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

```solidity
File: contracts/party/PartyGovernanceNFT.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol

```solidity
File: contracts/proposals/ArbitraryCallsProposal.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol

```solidity
File: contracts/proposals/FractionalizeProposal.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol

```solidity
File: contracts/proposals/ListOnOpenseaProposal.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol

```solidity
File: contracts/proposals/ListOnZoraProposal.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol

```solidity
File: contracts/proposals/ProposalExecutionEngine.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol

```solidity
File: contracts/proposals/ProposalStorage.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol

```solidity
File: contracts/tokens/ERC1155Receiver.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC1155Receiver.sol

```solidity
File: contracts/tokens/ERC721Receiver.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC721Receiver.sol

```solidity
File: contracts/utils/EIP165.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/EIP165.sol

```solidity
File: contracts/utils/Implementation.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Implementation.sol

```solidity
File: contracts/utils/Proxy.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol

```solidity
File: contracts/utils/ReadOnlyDelegateCall.sol

2:    pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol

### Natspec is missing

_There are **18** instances of this issue:_

```solidity
File: contracts/globals/IGlobals.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/IGlobals.sol

```solidity
File: contracts/proposals/ArbitraryCallsProposal.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol

```solidity
File: contracts/proposals/LibProposal.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol

```solidity
File: contracts/proposals/ProposalStorage.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol

```solidity
File: contracts/proposals/vendor/IOpenseaConduitController.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaConduitController.sol

```solidity
File: contracts/proposals/vendor/IOpenseaExchange.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaExchange.sol

```solidity
File: contracts/tokens/IERC1155.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC1155.sol

```solidity
File: contracts/tokens/IERC20.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC20.sol

```solidity
File: contracts/tokens/IERC721.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721.sol

```solidity
File: contracts/tokens/IERC721Receiver.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721Receiver.sol

```solidity
File: contracts/utils/Implementation.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Implementation.sol

```solidity
File: contracts/utils/LibAddress.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibAddress.sol

```solidity
File: contracts/utils/LibERC20Compat.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibERC20Compat.sol

```solidity
File: contracts/utils/LibRawResult.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibRawResult.sol

```solidity
File: contracts/utils/LibSafeCast.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibSafeCast.sol

```solidity
File: contracts/utils/LibSafeERC721.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibSafeERC721.sol

```solidity
File: contracts/utils/ReadOnlyDelegateCall.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol

```solidity
File: contracts/vendor/markets/IZoraAuctionHouse.sol

      /// @audit
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/markets/IZoraAuctionHouse.sol

### Event is missing `indexed` fields

_There are **11** instances of this issue:_

```solidity
File: contracts/crowdfund/BuyCrowdfundBase.sol

event      Won(Party party, IERC721 token, uint256 tokenId, uint256 settledPrice)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol

```solidity
File: contracts/crowdfund/Crowdfund.sol

event      Burned(address contributor, uint256 ethUsed, uint256 ethOwed, uint256 votingPower)

event      Contributed(address contributor, uint256 amount, address delegate, uint256 previousTotalContributions)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

```solidity
File: contracts/party/PartyGovernance.sol

event      PartyInitialized(GovernanceOpts opts, IERC721[] preciousTokens, uint256[] preciousTokenIds)

event      ProposalExecuted(uint256 indexed proposalId, address executor, bytes nextProgressData)

event      DistributionCreated(ITokenDistributor.TokenType tokenType, address token, uint256 tokenId)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

```solidity
File: contracts/proposals/ArbitraryCallsProposal.sol

event      ArbitraryCallExecuted(uint256 proposalId, uint256 idx, uint256 count)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol

```solidity
File: contracts/tokens/IERC1155.sol

event      ApprovalForAll(address indexed owner, address indexed operator, bool approved)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC1155.sol

```solidity
File: contracts/tokens/IERC20.sol

event      Transfer(address indexed owner, address indexed to, uint256 amount)

event      Approval(address indexed owner, address indexed spender, uint256 allowance)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC20.sol

```solidity
File: contracts/tokens/IERC721.sol

event      ApprovalForAll(address indexed owner, address indexed operator, bool approved)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721.sol

### Not used import

_There are **7** instances of this issue:_

```solidity
File: contracts/crowdfund/BuyCrowdfund.sol

7:    import "../utils/LibRawResult.sol";
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol

```solidity
File: contracts/crowdfund/CollectionBuyCrowdfund.sol

7:    import "../utils/LibRawResult.sol";
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol

```solidity
File: contracts/party/PartyGovernance.sol

9:    import "../tokens/IERC1155.sol";

21:   import "./IPartyFactory.sol";
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

```solidity
File: contracts/party/PartyGovernanceNFT.sol

4:    import "../utils/ReadOnlyDelegateCall.sol";
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol

```solidity
File: contracts/proposals/ProposalExecutionEngine.sol

13:   import "./LibProposal.sol";
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol

```solidity
File: contracts/proposals/ProposalStorage.sol

6:    import "../tokens/IERC721.sol";
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol
