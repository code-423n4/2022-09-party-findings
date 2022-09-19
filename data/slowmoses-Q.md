## (1) Solidity Version

The most recent version of solidity should be used.

All files state “pragma solidity ^0.8;”.
0.8.17 is already available.

All non library and non interface files should use a fixed version of solidity rather than a floating on.

## (2) Missing Indexed Fields in Events

Each event should use three indexed fields if there are three or more fields in the event.

***

event FractionalV1VaultCreated(
    IERC721 indexed token,
    uint256 indexed tokenId,
    uint256 vaultId,
    IERC20 vault,
    uint256 listPrice
);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/FractionalizeProposal.sol#L22-L28

event ZoraAuctionCreated(
    uint256 auctionId,
    IERC721 token,
    uint256 tokenId,
    uint256 startingPrice,
    uint40 duration,
    uint40 timeoutTime
);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L45-L52

event Won(Party party, IERC721 token, uint256 tokenId, uint256 settledPrice);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L52

event OpenseaOrderListed(
    IOpenseaExchange.OrderParameters orderParams,
    bytes32 orderHash,
    IERC721 token,
    uint256 tokenId,
    uint256 listPrice,
    uint256 expiry
);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L63-L70

event OpenseaOrderSold(
    bytes32 orderHash,
    IERC721 token,
    uint256 tokenId,
    uint256 listPrice
);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L71-L76

event OpenseaOrderExpired(
    bytes32 orderHash,
    IERC721 token,
    uint256 tokenId,
    uint256 expiry
);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L77-L82

event OrderValidated(
    bytes32 orderHash,
    address indexed offerer,
    address indexed zone,
    IOpenseaExchange.OfferItem[] offer,
    IOpenseaExchange.ConsiderationItem[] consideration,
    IOpenseaExchange.OrderType orderType,
    uint256 startTime,
    uint256 endTime,
    bytes32 zoneHash,
    uint256 salt,
    bytes32 conduitKey,
    uint256 counter
);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L84-L97

event Burned(address contributor, uint256 ethUsed, uint256 ethOwed, uint256 votingPower);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L82

event Contributed(address contributor, uint256 amount, address delegate, uint256 previousTotalContributions);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L83

event DistributionFeeClaimed(
    ITokenDistributorParty indexed party,
    address indexed feeRecipient,
    TokenType tokenType,
    address token,
    uint256 amount
);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/ITokenDistributor.sol#L37-L43

## (3) Avoid Magic Numbers

Checking against magic numbers should be avoided.
Consider using a properly named constant instead.

***

if (args.feeBps > 1e4) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L335

if (opts.splitBps > 1e4) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L135

if (opts.feeBps > 1e4) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L280

if (opts.passThresholdBps > 1e4) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L283

if (call.data.length >= 4) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L156

## (4) Confusing Number Format

Using 9999 instead of 0.9999e4 would be preferable.
Needlessly confusing number formats are a source of subtle errors.

return acceptanceRatio >= 0.9999e4;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1066

## (5) Compiler Warnings

Running “yarn build” results in a number of warnings (see blow).
Finished code must compile without warnings and especially without easily avoidable warnings. Many warnings below should be easily avoidable.

---------------

yarn run v1.22.19
$ yarn build:sol && yarn build:ts
$ forge build --extra-output storageLayout
[⠰] Compiling...
[⠘] Compiling 126 files with 0.8.17
[⠰] Solc 0.8.17 finished in 18.31s
Compiler run successful (with warnings)
warning[3628]: Warning: This contract has a payable fallback function, but no receive ether function. Consider adding a receive ether function.
 --> contracts/utils/Proxy.sol:8:1:
  |
8 | contract Proxy {
  | ^ (Relevant source part starts here and spans across multiple lines).
Note: The payable fallback function is defined here.
  --> contracts/utils/Proxy.sol:25:5:
   |
25 |     fallback() external payable {
   |     ^ (Relevant source part starts here and spans across multiple lines).



warning[9302]: Warning: Return value of low-level calls not used.
  --> sol-tests/utils/Implementation.t.sol:24:9:
   |
24 |         address(this).call(abi.encodeCall(this.initialize, ()));
   |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^



warning[5740]: Warning: Unreachable code.
  --> contracts/crowdfund/CrowdfundNFT.sol:52:5:
   |
52 |     {}
   |     ^^



warning[5740]: Warning: Unreachable code.
  --> contracts/crowdfund/CrowdfundNFT.sol:59:5:
   |
59 |     {}
   |     ^^



warning[5740]: Warning: Unreachable code.
  --> contracts/crowdfund/CrowdfundNFT.sol:66:5:
   |
66 |     {}
   |     ^^



warning[5740]: Warning: Unreachable code.
  --> contracts/crowdfund/CrowdfundNFT.sol:73:5:
   |
73 |     {}
   |     ^^



warning[5740]: Warning: Unreachable code.
  --> contracts/crowdfund/CrowdfundNFT.sol:80:5:
   |
80 |     {}
   |     ^^



warning[5805]: Warning: "this" used in constructor. Note that external functions of a contract cannot be called while it is being constructed.
  --> sol-tests/utils/Implementation.t.sol:24:43:
   |
24 |         address(this).call(abi.encodeCall(this.initialize, ()));
   |                                           ^^^^



$ tsc -b
Done in 19.96s.

=========================================================================

## (6) Revert and Require Should Have Descriptive Reason Strings

***

require(msg.sender == address(this));
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/ReadOnlyDelegateCall.sol#L20

require(proposalType != ProposalType.Invalid);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L246

require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L247

