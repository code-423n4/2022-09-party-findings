# Low Issues

|No.|Issue|Instances|
|:-|:-|:-:|
|1|Use of `assert()` statements|11|
|2|Unused/empty `receive()`/`fallback()` function|2|

Total: **13** instances over **2** issues

## `require()` should be used instead of `assert()` statements
Prior to Solidity v0.8.0, hitting an `assert()` statement **consumes all the remaining of the transaction's available gas** rather than returning it, as `require()`/`revert()` do.

Even past Solidity v0.8.0, `assert()` statements should be avoided, as seen in Solidity's [documentation](https://docs.soliditylang.org/en/v0.8.0/control-structures.html#panic-via-assert-and-error-via-require):
> Assert should only be used to test for internal errors, and to check invariants. Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix. Language analysis tools can evaluate your contract to identify the conditions and function calls which will cause a Panic.

Thus, consider using `require()` statements instead of `assert()`, or if-statements with `revert()`.

_There are **11** instances of this issue:_  
```solidity
contracts/proposals/FractionalizeProposal.sol:
  67:        assert(vault.balanceOf(address(this)) == supply);

contracts/proposals/ListOnZoraProposal.sol:
 120:        assert(step == ZoraStep.ListedOnZora);

contracts/proposals/ProposalExecutionEngine.sol:
 142:        assert(currentInProgressProposalId == 0);

contracts/proposals/ListOnOpenseaProposal.sol:
 221:        assert(step == ListOnOpenseaStep.ListedOnOpenSea);
 302:        assert(SEAPORT.validate(orders));

contracts/utils/ReadOnlyDelegateCall.sol:
  30:        assert(false);

contracts/party/PartyGovernance.sol:
 504:        assert(tokenType == ITokenDistributor.TokenType.Erc20);

contracts/party/PartyGovernanceNFT.sol:
 179:        assert(false); // Will not be reached.

contracts/distribution/TokenDistributor.sol:
 385:        assert(tokenType == TokenType.Erc20);
 411:        assert(tokenType == TokenType.Erc20);

contracts/crowdfund/CrowdfundNFT.sol:
 166:        assert(false); // Will not be reached.
```

## Unused/empty `receive()`/`fallback()` function
If the intention is for the Ether to be used, the function should call another function, otherwise it should revert.

_There are **2** instances of this issue:_  
```solidity
contracts/party/Party.sol:
  47:        receive() external payable {}

contracts/crowdfund/AuctionCrowdfund.sol:
 144:        receive() external payable {}
```

# Non-Critical Issues

|No.|Issue|Instances|
|:-|:-|:-:|
|1|Missing error message in `require` statements|3|
|2|Floating compiler versions|19|
|3|Unused `override` function arguments|7|
|4|`constants` should be defined rather than using magic numbers|22|
|5|`event` is missing `indexed` fields|20|

Total: **71** instances over **5** issues

## Missing error message in `require` statements
It is recommended to have descriptive error messages for all `require` statements to provide users with information should a transaction fail.

_There are **3** instances of this issue:_  
```solidity
contracts/proposals/ProposalExecutionEngine.sol:
 246:        require(proposalType != ProposalType.Invalid);
 247:        require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));

contracts/utils/ReadOnlyDelegateCall.sol:
  20:        require(msg.sender == address(this));
```

## Floating compiler versions
Non-library/interface files should use fixed compiler versions, not floating ones.

_There are **19** instances of this issue:_  
```solidity
contracts/proposals/FractionalizeProposal.sol:
   2:        pragma solidity ^0.8;

contracts/proposals/ListOnZoraProposal.sol:
   2:        pragma solidity ^0.8;

contracts/proposals/ArbitraryCallsProposal.sol:
   2:        pragma solidity ^0.8;

contracts/proposals/ProposalExecutionEngine.sol:
   2:        pragma solidity ^0.8;

contracts/proposals/ProposalStorage.sol:
   2:        pragma solidity ^0.8;

contracts/globals/Globals.sol:
   2:        pragma solidity ^0.8;

contracts/tokens/ERC1155Receiver.sol:
   2:        pragma solidity ^0.8;

contracts/tokens/ERC721Receiver.sol:
   2:        pragma solidity ^0.8;

contracts/utils/Proxy.sol:
   2:        pragma solidity ^0.8;

contracts/utils/EIP165.sol:
   2:        pragma solidity ^0.8;

contracts/party/PartyFactory.sol:
   2:        pragma solidity ^0.8;

contracts/party/Party.sol:
   2:        pragma solidity ^0.8;

contracts/party/PartyGovernanceNFT.sol:
   2:        pragma solidity ^0.8;

contracts/distribution/TokenDistributor.sol:
   2:        pragma solidity ^0.8;

contracts/crowdfund/CrowdfundNFT.sol:
   2:        pragma solidity ^0.8;

contracts/crowdfund/CrowdfundFactory.sol:
   2:        pragma solidity ^0.8;

contracts/crowdfund/BuyCrowdfund.sol:
   2:        pragma solidity ^0.8;

contracts/crowdfund/CollectionBuyCrowdfund.sol:
   2:        pragma solidity ^0.8;

contracts/crowdfund/AuctionCrowdfund.sol:
   2:        pragma solidity ^0.8;
```

## Unused `override` function arguments
For functions declared as `override`, unused arguments should have the variable name removed or commented out to avoid compiler warnings.

_There are **7** instances of this issue:_  
```solidity
contracts/proposals/ProposalExecutionEngine.sol:
  99:        function initialize(address oldImpl, bytes calldata initializeData)
  99:        function initialize(address oldImpl, bytes calldata initializeData)

contracts/tokens/ERC721Receiver.sol:
  14:        function onERC721Received(address, address, uint256, bytes memory)
  14:        function onERC721Received(address, address, uint256, bytes memory)
  14:        function onERC721Received(address, address, uint256, bytes memory)
  14:        function onERC721Received(address, address, uint256, bytes memory)

contracts/party/PartyGovernanceNFT.sol:
  88:        function tokenURI(uint256) public override view returns (string memory) {
```

## `constants` should be defined rather than using magic numbers
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) code can benefit from using readable constants instead of hex/numeric literals.

_There are **22** instances of this issue:_  
`4`:
```solidity
contracts/proposals/ArbitraryCallsProposal.sol:
 156:        if (call.data.length >= 4) {
```
`68`:
```solidity
contracts/proposals/ArbitraryCallsProposal.sol:
 208:        if (callData.length < 68) {
```
`68`:
```solidity
contracts/proposals/ArbitraryCallsProposal.sol:
 226:        if (callData.length < 68) {
```
`4`:
```solidity
contracts/proposals/ProposalExecutionEngine.sol:
 236:        if (proposalData.length < 4) {
```
`32`:
```solidity
contracts/utils/LibSafeERC721.sol:
  25:        if (!s || r.length < 32) {
```
`1e4`:
```solidity
contracts/party/PartyGovernance.sol:
 280:        if (opts.feeBps > 1e4) {
```
`1e4`:
```solidity
contracts/party/PartyGovernance.sol:
 283:        if (opts.passThresholdBps > 1e4) {
```
`1e4`:
```solidity
contracts/party/PartyGovernance.sol:
1062:        uint256 acceptanceRatio = (totalVotes * 1e4) / totalVotingPower;
```
`0.9999e4`:
```solidity
contracts/party/PartyGovernance.sol:
1066:        return acceptanceRatio >= 0.9999e4;
```
`1e4`:
```solidity
contracts/party/PartyGovernance.sol:
1078:        return uint256(voteCount) * 1e4
```
`1e18`:
```solidity
contracts/party/PartyGovernanceNFT.sol:
 112:        return votingPowerByTokenId[tokenId] * 1e18 / _getTotalVotingPower();
```
`1e18`:
```solidity
contracts/distribution/TokenDistributor.sol:
 261:        + (1e18 - 1)
```
`1e18`:
```solidity
contracts/distribution/TokenDistributor.sol:
 263:        / 1e18
```
`1e4`:
```solidity
contracts/distribution/TokenDistributor.sol:
 335:        if (args.feeBps > 1e4) {
```
`1e4`:
```solidity
contracts/distribution/TokenDistributor.sol:
 352:        uint128 fee = supply * args.feeBps / 1e4;
```
`1e4`:
```solidity
contracts/crowdfund/Crowdfund.sol:
 129:        if (opts.governanceOpts.feeBps > 1e4) {
```
`1e4`:
```solidity
contracts/crowdfund/Crowdfund.sol:
 132:        if (opts.governanceOpts.passThresholdBps > 1e4) {
```
`1e4`:
```solidity
contracts/crowdfund/Crowdfund.sol:
 135:        if (opts.splitBps > 1e4) {
```
`1e4`:
```solidity
contracts/crowdfund/Crowdfund.sol:
 370:        votingPower = ((1e4 - splitBps_) * ethUsed) / 1e4;
```
`1e4`:
```solidity
contracts/crowdfund/Crowdfund.sol:
 370:        votingPower = ((1e4 - splitBps_) * ethUsed) / 1e4;
```
`1e4`:
```solidity
contracts/crowdfund/Crowdfund.sol:
 374:        votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up
```
`1e4`:
```solidity
contracts/crowdfund/Crowdfund.sol:
 374:        votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up
```

## `event` is missing `indexed` fields
Each `event` should use three `indexed` fields if there are three or more fields.
  
_There are **20** instances of this issue:_  
```solidity
contracts/proposals/FractionalizeProposal.sol:
  22:        event FractionalV1VaultCreated(
  23:            IERC721 indexed token,
  24:            uint256 indexed tokenId,
  25:            uint256 vaultId,
  26:            IERC20 vault,
  27:            uint256 listPrice
  28:        );

contracts/proposals/ListOnZoraProposal.sol:
  45:        event ZoraAuctionCreated(
  46:            uint256 auctionId,
  47:            IERC721 token,
  48:            uint256 tokenId,
  49:            uint256 startingPrice,
  50:            uint40 duration,
  51:            uint40 timeoutTime
  52:        );

contracts/proposals/ArbitraryCallsProposal.sol:
  35:        event ArbitraryCallExecuted(uint256 proposalId, uint256 idx, uint256 count);

contracts/proposals/ListOnOpenseaProposal.sol:
  63:        event OpenseaOrderListed(
  64:            IOpenseaExchange.OrderParameters orderParams,
  65:            bytes32 orderHash,
  66:            IERC721 token,
  67:            uint256 tokenId,
  68:            uint256 listPrice,
  69:            uint256 expiry
  70:        );


  71:        event OpenseaOrderSold(
  72:            bytes32 orderHash,
  73:            IERC721 token,
  74:            uint256 tokenId,
  75:            uint256 listPrice
  76:        );


  77:        event OpenseaOrderExpired(
  78:            bytes32 orderHash,
  79:            IERC721 token,
  80:            uint256 tokenId,
  81:            uint256 expiry
  82:        );


  84:        event OrderValidated(
  85:            bytes32 orderHash,
  86:            address indexed offerer,
  87:            address indexed zone,
  88:            IOpenseaExchange.OfferItem[] offer,
  89:            IOpenseaExchange.ConsiderationItem[] consideration,
  90:            IOpenseaExchange.OrderType orderType,
  91:            uint256 startTime,
  92:            uint256 endTime,
  93:            bytes32 zoneHash,
  94:            uint256 salt,
  95:            bytes32 conduitKey,
  96:            uint256 counter
  97:        );

contracts/tokens/IERC1155.sol:
  20:        event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

contracts/tokens/IERC721.sol:
   8:        event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

contracts/tokens/IERC20.sol:
   6:        event Transfer(address indexed owner, address indexed to, uint256 amount);
   7:        event Approval(address indexed owner, address indexed spender, uint256 allowance);

contracts/party/PartyGovernance.sol:
 151:        event Proposed(
 152:            uint256 proposalId,
 153:            address proposer,
 154:            Proposal proposal
 155:        );


 156:        event ProposalAccepted(
 157:            uint256 proposalId,
 158:            address voter,
 159:            uint256 weight
 160:        );

 162:        event PartyInitialized(GovernanceOpts opts, IERC721[] preciousTokens, uint256[] preciousTokenIds);
 165:        event ProposalExecuted(uint256 indexed proposalId, address executor, bytes nextProgressData);
 167:        event DistributionCreated(ITokenDistributor.TokenType tokenType, address token, uint256 tokenId);

contracts/distribution/ITokenDistributor.sol:
  37:        event DistributionFeeClaimed(
  38:            ITokenDistributorParty indexed party,
  39:            address indexed feeRecipient,
  40:            TokenType tokenType,
  41:            address token,
  42:            uint256 amount
  43:        );

contracts/crowdfund/BuyCrowdfundBase.sol:
  52:        event Won(Party party, IERC721 token, uint256 tokenId, uint256 settledPrice);

contracts/crowdfund/Crowdfund.sol:
  82:        event Burned(address contributor, uint256 ethUsed, uint256 ethOwed, uint256 votingPower);
  83:        event Contributed(address contributor, uint256 amount, address delegate, uint256 previousTotalContributions);
```

