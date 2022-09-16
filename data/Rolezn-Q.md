## (1) IERC20 approve() Is Deprecated

Severity: Low

approve is subject to a known front-running attack. It is deprecated in favor of safeIncreaseAllowance() and safeDecreaseAllowance(). If only setting the initial allowance to the value that means infinite, safeIncreaseAllowance() can be used instead.

## Proof Of Concept

	data.token.approve(address(VAULT_FACTORY), data.tokenId);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/FractionalizeProposal.sol#L53

	token.approve(conduit, tokenId);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L256

	token.approve(address(0), tokenId);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L367

	token.approve(address(ZORA), tokenId);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L143



## Recommended Mitigation Steps

Consider using safeIncreaseAllowance() / safeDecreaseAllowance() instead.


## (2) Use _safeMint instead of _mint

Severity: Low

According to openzepplin's ERC721, the use of _mint is discouraged, use _safeMint whenever possible.
https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#ERC721-_mint-address-uint256-

## Proof Of Concept

	_mint(owner, tokenId);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernanceNFT.sol#L132



## Recommended Mitigation Steps

Use _safeMint whenever possible instead of _mint


## (3) Missing Contract-existence Checks Before Low-level Calls

Severity: Low

Low-level calls return success if there is no code present at the specified address. In addition to the zero-address checks, add a check to verify that <address>.code.length > 0

## Proof Of Concept


	(bool s, bytes memory r) = address(market).delegatecall(abi.encodeCall(
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L177

	(address(_getProposalExecutionEngine())).delegatecall(abi.encodeCall(
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L766

	address(_getProposalExecutionEngine()).delegatecall(abi.encodeCall(
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L832

	(bool s, bytes memory r) = address(impl).delegatecall(
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalStorage.sol#L39

	(bool s, bytes memory r) = impl.delegatecall(callData);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/ReadOnlyDelegateCall.sol#L11



## (4) Missing Checks for Address(0x0) 

Severity: Low

## Proof Of Concept


	function burn(address payable contributor)
        public
    {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L167

	function batchBurn(address payable[] calldata contributors)
        external
    {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L175

	function transferMultiSig(address newMultiSig) external onlyMultisig {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/Globals.sol#L27

	function delegateVotingPower(address delegate) external onlyDelegateCall {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L451



## Recommended Mitigation Steps

Consider adding zero-address checks in the mentioned codebase.



## (5) Use Safetransfer Instead Of Transfer 

Severity: Low

It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

Reference: This similar medium-severity (https://consensys.net/diligence/audits/2021/01/fei-protocol/#unchecked-return-value-for-iweth-transfer-call) finding from Consensys Diligence Audit of Fei Protocol.

## Proof Of Concept


	preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L301


## Recommended Mitigation Steps

Consider using safeTransfer/safeTransferFrom or require() consistently.



## (6) Unused Receive() Function Will Lock Ether In Contract 

Severity: Low

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert

## Proof Of Concept


	receive() external payable {}
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L143

	receive() external payable {}
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/Party.sol#L47



## Recommended Mitigation Steps

The function should call another function, otherwise it should revert

## (7) Solidity ABI Decoder Bug For Multi-Dimensional Memory Arrays

Severity: Low

On April 5th, 2021, a bug in the Solidity ABI decoder v2 was reported by John Toman of the Certora development team. Certora’s bug disclosure post can be found here: Memory Isolation Violation in Deserialization Code.
The bug is fixed with Solidity version 0.8.4 released on April 21st, 2021. The bug is present in all prior versions of ABI coder v2.
You could be affected if you are using ABI coder v2 (which is the default starting from 0.8.0) and especially the function abi.decode on memory (as opposed to calldata) arrays with multi-dimensional arrays or arrays that contain structs.
The impact of the bug is that if the encoded data is specially crafted, then the decoded value can depend on values in memory outside of the data to be decoded.
This means that two calls to abi.decode with the same data can result in different values.
It is not possible to exploit this bug in a way that results in invalid memory write operations.
Using abi.decode on calldata byte arrays or the implicit use of abi.decode for function parameters is unaffected.

## Reference
https://blog.soliditylang.org/2021/04/21/decoding-from-memory-bug/

## Proof of Concept
	(ArbitraryCall[] memory calls) = abi.decode(params.proposalData, (ArbitraryCall[]));
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L44

	FractionalizeProposalData memory data =
	abi.decode(params.proposalData, (FractionalizeProposalData));
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L46-L47

	(OpenseaProposalData memory data) =
	 abi.decode(params.proposalData, (OpenseaProposalData));
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L129-L130

	(, ZoraProgressData memory zpd) =
	 abi.decode(params.progressData, (uint8, ZoraProgressData));
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L178-L179

	(, OpenseaProgressData memory opd) =
	abi.decode(params.progressData, (uint8, OpenseaProgressData));
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L224-L225

(ZoraProposalData memory data) = abi.decode(params.proposalData, (ZoraProposalData));
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L84

	(, ZoraProgressData memory pd) =
	abi.decode(params.progressData, (ZoraStep, ZoraProgressData));
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L121-L122

## Recommended Mitigation Steps
Upgrade solidity version to at least 0.8.4 or above.



## (8) Avoid Floating Pragmas: The Version Should Be Locked

Severity: Non-Critical

## Proof Of Concept

	Found usage of floating pragmas ^0.8 of Solidity in AuctionCrowdfund.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in BuyCrowdfund.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfund.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in BuyCrowdfundBase.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in CollectionBuyCrowdfund.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in Crowdfund.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in CrowdfundFactory.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundFactory.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in CrowdfundNFT.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundNFT.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ITokenDistributor.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/ITokenDistributor.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ITokenDistributorParty.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/ITokenDistributorParty.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in TokenDistributor.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IGateKeeper.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/gatekeepers/IGateKeeper.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in Globals.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/Globals.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IGlobals.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/IGlobals.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IMarketWrapper.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/market-wrapper/IMarketWrapper.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IPartyFactory.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/IPartyFactory.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in Party.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/Party.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in PartyFactory.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyFactory.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in PartyGovernance.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in PartyGovernanceNFT.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernanceNFT.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ArbitraryCallsProposal.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in FractionalizeProposal.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/FractionalizeProposal.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IProposalExecutionEngine.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/IProposalExecutionEngine.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in LibProposal.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ListOnOpenseaProposal.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ListOnZoraProposal.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ProposalExecutionEngine.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in \ProposalStorage.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalStorage.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in FractionalV1.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/vendor/FractionalV1.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IOpenseaConduitController.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/vendor/IOpenseaConduitController.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IOpenseaExchange.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/vendor/IOpenseaExchange.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ERC1155Receiver.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/ERC1155Receiver.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ERC721Receiver.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/ERC721Receiver.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IERC1155.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/IERC1155.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IERC20.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/IERC20.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IERC721.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/IERC721.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IERC721Receiver.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/IERC721Receiver.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in EIP165.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/EIP165.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in Implementation.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/Implementation.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in LibAddress.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/LibAddress.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in LibERC20Compat.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/LibERC20Compat.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in LibRawResult.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/LibRawResult.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in LibSafeCast.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/LibSafeCast.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in LibSafeERC721.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/LibSafeERC721.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in Proxy.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/Proxy.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ReadOnlyDelegateCall.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/ReadOnlyDelegateCall.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IZoraAuctionHouse.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/vendor/markets/IZoraAuctionHouse.sol#L2




## (9) Constants Should Be Defined Rather Than Using Magic Numbers

Severity: Non-Critical

## Proof Of Concept

        if (opts.governanceOpts.feeBps > 1e4) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L129

        votingPower = ((1e4 - splitBps_) * ethUsed) / 1e4;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L370

        // is entitled to, scaled by 1e18.
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L255

        if (args.feeBps > 1e4) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L335

        if (opts.feeBps > 1e4) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L280

        uint256 acceptanceRatio = (totalVotes * 1e4) / totalVotingPower;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1062

        return uint256(voteCount) * 1e4
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1078

        return votingPowerByTokenId[tokenId] * 1e18 / _getTotalVotingPower();
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernanceNFT.sol#L112

        if (callData.length < 68) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L208

        if (callData.length < 68) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L226




## (10) Event Is Missing Indexed Fields

Severity: Non-Critical

Each event should use three indexed fields if there are three or more fields.
In addition, each event should have at least one indexed fields to allow easier filtering of logs.

## Proof Of Concept


	event Won(uint256 bid, Party party);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L74

	event Won(Party party, IERC721 token, uint256 tokenId, uint256 settledPrice);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L52

	event Burned(address contributor, uint256 ethUsed, uint256 ethOwed, uint256 votingPower);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L82

	event Contributed(address contributor, uint256 amount, address delegate, uint256 previousTotalContributions);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L83

	event BuyCrowdfundCreated(BuyCrowdfund crowdfund, BuyCrowdfund.BuyCrowdfundOptions opts);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundFactory.sol#L16

	event AuctionCrowdfundCreated(AuctionCrowdfund crowdfund, AuctionCrowdfund.AuctionCrowdfundOptions opts);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundFactory.sol#L17

	event CollectionBuyCrowdfundCreated(CollectionBuyCrowdfund crowdfund, CollectionBuyCrowdfund.CollectionBuyCrowdfundOptions opts);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundFactory.sol#L18

	event DistributionFeeClaimed(
        ITokenDistributorParty indexed party,
        address indexed feeRecipient,
        TokenType tokenType,
        address token,
        uint256 amount
    );
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/ITokenDistributor.sol#L37

	event PartyCreated(Party party, address creator);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/IPartyFactory.sol#L11

	event Proposed(
        uint256 proposalId,
        address proposer,
        Proposal proposal
    );
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L151

	event ProposalAccepted(
        uint256 proposalId,
        address voter,
        uint256 weight
    );
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L156

	event PartyInitialized(GovernanceOpts opts, IERC721[] preciousTokens, uint256[] preciousTokenIds);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L162

	event ProposalExecuted(uint256 indexed proposalId, address executor, bytes nextProgressData);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L165

	event DistributionCreated(ITokenDistributor.TokenType tokenType, address token, uint256 tokenId);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L167

	event HostStatusTransferred(address oldHost, address newHost);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L169

	event ArbitraryCallExecuted(uint256 proposalId, uint256 idx, uint256 count);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L35

	event FractionalV1VaultCreated(
        IERC721 indexed token,
        uint256 indexed tokenId,
        uint256 vaultId,
        IERC20 vault,
        uint256 listPrice
    );
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/FractionalizeProposal.sol#L22

	event OpenseaOrderListed(
        IOpenseaExchange.OrderParameters orderParams,
        bytes32 orderHash,
        IERC721 token,
        uint256 tokenId,
        uint256 listPrice,
        uint256 expiry
    );
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L63

	event OpenseaOrderSold(
        bytes32 orderHash,
        IERC721 token,
        uint256 tokenId,
        uint256 listPrice
    );
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L71

	event OpenseaOrderExpired(
        bytes32 orderHash,
        IERC721 token,
        uint256 tokenId,
        uint256 expiry
    );
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L77

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
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L84

	event ZoraAuctionCreated(
        uint256 auctionId,
        IERC721 token,
        uint256 tokenId,
        uint256 startingPrice,
        uint40 duration,
        uint40 timeoutTime
    );
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L45

	event ZoraAuctionExpired(uint256 auctionId, uint256 expiry);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L53

	event ProposalEngineImplementationUpgraded(address oldImpl, address newImpl);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L66

	event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/IERC1155.sol#L20

	event Transfer(address indexed owner, address indexed to, uint256 amount);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/IERC20.sol#L6

	event Approval(address indexed owner, address indexed spender, uint256 allowance);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/IERC20.sol#L7

	event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/IERC721.sol#L8




## (11) Missing event for critical parameter change

Severity: Non-Critical

When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

## Proof Of Concept

	function setBytes32(
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/Globals.sol#L59

	function setUint256(
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/Globals.sol#L63

	function setAddress(
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/Globals.sol#L67

	function setIncludesBytes32(
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/Globals.sol#L71

	function setIncludesUint256(
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/Globals.sol#L75

	function setIncludesAddress(
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/Globals.sol#L79





## (12) Implementation contract may not be initialized

OpenZeppelin recommends that the initializer modifier be applied to constructors. 
Per OZs Post implementation contract should be initialized to avoid potential griefs or exploits.
https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5

Severity: Non-Critical

## Proof Of Concept


	constructor(IGlobals globals) Crowdfund(globals) {}
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L103

	constructor(IGlobals globals) BuyCrowdfundBase(globals) {}
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfund.sol#L62

	constructor(IGlobals globals) BuyCrowdfundBase(globals) {}
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L77

	constructor(IGlobals globals) {
        _GLOBALS = globals;
    }
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundFactory.sol#L25

	constructor(IGlobals globals) {
        _GLOBALS = globals;
    }
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundNFT.sol#L34

	constructor(IGlobals globals) {
        GLOBALS = globals;
    } 
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L93

	constructor(address multiSig_) {
        multiSig = multiSig_;
    }
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/Globals.sol#L23

	constructor(IGlobals globals) PartyGovernanceNFT(globals) {}
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/Party.sol#L28

	constructor(IGlobals globals) {
        GLOBALS = globals;
    }
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyFactory.sol#L21

	constructor(IGlobals globals) PartyGovernance(globals) ERC721('', '') {
        _GLOBALS = globals;
    }
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernanceNFT.sol#L44

	constructor(IFractionalV1VaultFactory vaultFactory) 
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/FractionalizeProposal.sol#L14

	constructor() ZoraTestUtils(ZORA) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L70

	constructor(
        IGlobals globals,
        IOpenseaExchange seaport,
        IOpenseaConduitController seaportConduitController,
        IZoraAuctionHouse zora,
        IFractionalV1VaultFactory fractionalVaultFactory
    )
        ProposalExecutionEngine(
            globals,
            seaport,
            seaportConduitController,
            zora,
            fractionalVaultFactory
        )
    {}
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L18





## (13) Use a more recent version of Solidity

Severity: Non-Critical

Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>)
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

## Proof Of Concept


	Found usage of floating pragmas ^0.8 of Solidity in AuctionCrowdfund.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in BuyCrowdfund.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfund.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in BuyCrowdfundBase.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in CollectionBuyCrowdfund.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in Crowdfund.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in CrowdfundFactory.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundFactory.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in CrowdfundNFT.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundNFT.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ITokenDistributor.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/ITokenDistributor.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ITokenDistributorParty.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/ITokenDistributorParty.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in TokenDistributor.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IGateKeeper.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/gatekeepers/IGateKeeper.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in Globals.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/Globals.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IGlobals.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/IGlobals.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IMarketWrapper.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/market-wrapper/IMarketWrapper.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IPartyFactory.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/IPartyFactory.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in Party.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/Party.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in PartyFactory.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyFactory.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in PartyGovernance.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in PartyGovernanceNFT.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernanceNFT.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ArbitraryCallsProposal.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in FractionalizeProposal.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/FractionalizeProposal.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IProposalExecutionEngine.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/IProposalExecutionEngine.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in LibProposal.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ListOnOpenseaProposal.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ListOnZoraProposal.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ProposalExecutionEngine.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in \ProposalStorage.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalStorage.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in FractionalV1.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/vendor/FractionalV1.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IOpenseaConduitController.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/vendor/IOpenseaConduitController.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IOpenseaExchange.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/vendor/IOpenseaExchange.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ERC1155Receiver.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/ERC1155Receiver.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ERC721Receiver.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/ERC721Receiver.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IERC1155.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/IERC1155.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IERC20.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/IERC20.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IERC721.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/IERC721.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IERC721Receiver.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/tokens/IERC721Receiver.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in EIP165.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/EIP165.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in Implementation.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/Implementation.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in LibAddress.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/LibAddress.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in LibERC20Compat.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/LibERC20Compat.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in LibRawResult.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/LibRawResult.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in LibSafeCast.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/LibSafeCast.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in LibSafeERC721.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/LibSafeERC721.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in Proxy.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/Proxy.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in ReadOnlyDelegateCall.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/ReadOnlyDelegateCall.sol#L2

	Found usage of floating pragmas ^0.8 of Solidity in IZoraAuctionHouse.sol
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/vendor/markets/IZoraAuctionHouse.sol#L2



## Recommended Mitigation Steps

Consider updating to a more recent solidity version.



## (14) Public Functions Not Called By The Contract Should Be Declared External Instead

Severity: Non-Critical

Contracts are allowed to override their parents’ functions and change the visibility from external to public.

## Proof Of Concept


	function burn(address payable contributor)
        public
    {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L167

	function contribute(address delegate, bytes memory gateData)
        public
        payable
    {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L191

	function createBuyCrowdfund(
        BuyCrowdfund.BuyCrowdfundOptions memory opts,
        bytes memory createGateCallData
    )
        public
        payable
        returns (BuyCrowdfund inst)
    {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundFactory.sol#L35

	function createAuctionCrowdfund(
        AuctionCrowdfund.AuctionCrowdfundOptions memory opts,
        bytes memory createGateCallData
    )
        public
        payable
        returns (AuctionCrowdfund inst)
    {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundFactory.sol#L61

	function createCollectionBuyCrowdfund(
        CollectionBuyCrowdfund.CollectionBuyCrowdfundOptions memory opts,
        bytes memory createGateCallData
    )
        public
        payable
        returns (CollectionBuyCrowdfund inst)
    {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundFactory.sol#L87

	function accept(uint256 proposalId, uint256 snapIndex)
        public
        onlyDelegateCall
        returns (uint256 totalVotes)
    {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L562



## (15) Require()/revert() Statements Should Have Descriptive Reason Strings

Severity: Non-Critical

## Proof Of Concept


	require(proposalType != ProposalType.Invalid);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L246

	require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L247

	require(proposalType != ProposalType.Invalid);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L246

	require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L247

	require(msg.sender == address(this));
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/ReadOnlyDelegateCall.sol#L10






## (16) Use of Block.Timestamp

Severity: Non-Critical

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

## Proof Of Concept


        expiry = uint40(opts.duration + block.timestamp);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L117

        if (block.timestamp >= expiry) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L268

        expiry = uint40(opts.duration + block.timestamp);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L73

        if (block.timestamp >= expiry) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L159

        if (proposal.maxExecutableTime < block.timestamp) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L681

        if (block.timestamp < cancelTime) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L753

        } else if (expiry <= block.timestamp) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L364

        if (minExpiry > uint40(block.timestamp)) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L188



## Recommended Mitigation Steps
Block timestamps should not be used for entropy or generating random numbers—i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state.

Time-sensitive logic is sometimes required; e.g., for unlocking contracts (time-locking), completing an ICO after a few weeks, or enforcing expiry dates. It is sometimes recommended to use block.number and an average block time to estimate times; with a 10 second block time, 1 week equates to approximately, 60480 blocks. Thus, specifying a block number at which to change a contract state can be more secure, as miners are unable to easily manipulate the block number.



