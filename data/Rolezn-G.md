## (1) Abi.encode() Is Less Efficient Than Abi.encodepacked()

Severity: Gas Optimizations

## Proof Of Concept

	return abi.encode(ListOnOpenseaStep.ListedOnZora, ZoraProgressData({
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L164

	return abi.encode(ListOnOpenseaStep.ListedOnOpenSea, orderHash, expiry);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L219

	return abi.encode(ZoraStep.ListedOnZora, ZoraProgressData({
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L115

	abi.encode(s, r).rawRevert();
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/ReadOnlyDelegateCall.sol#L13


## (2) Require() Should Be Used Instead Of Assert()

Severity: Gas Optimizations

## Proof Of Concept


    assert(false); // Will not be reached.
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundNFT.sol#L166

    assert(tokenType == TokenType.Erc20);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L385

    assert(tokenType == TokenType.Erc20);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L411

    assert(tokenType == ITokenDistributor.TokenType.Erc20);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L504

    assert(false); // Will not be reached.
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernanceNFT.sol#L179

    assert(vault.balanceOf(address(this)) == supply);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/FractionalizeProposal.sol#L67

    assert(step == ListOnOpenseaStep.ListedOnOpenSea);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L221

    assert(SEAPORT.validate(orders));
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L302

    assert(step == ZoraStep.ListedOnZora);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L120

    assert(currentInProgressProposalId == 0);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ProposalExecutionEngine.sol#L142

    assert(false);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/utils/ReadOnlyDelegateCall.sol#L30


## (3) <Array>.length Should Not Be Looked Up In Every Loop Of A For-loop

Severity: Gas Optimizations

The overheads outlined below are PER LOOP, excluding the first loop

storage arrays incur a Gwarmaccess (100 gas)
memory arrays use MLOAD (3 gas)
calldata arrays use CALLDATALOAD (3 gas)

Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset

## Proof Of Concept

	for (uint256 i = 0; i < contributors.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L180

	for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L300

	for (uint256 i = 0; i < infos.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L230

	for (uint256 i = 0; i < infos.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L239

	for (uint256 i=0; i < opts.hosts.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L306

	for (uint256 i = 0; i < hadPreciouses.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L52

	for (uint256 i = 0; i < calls.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L61

	for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L14

	for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L32

	for (uint256 i = 0; i < fees.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L291






## (4) Using Calldata Instead Of Memory For Read-only Arguments In External Functions Saves Gas

Severity: Gas Optimizations

When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution. Structs have the same overhead as an array of length one


## Proof Of Concept

	function createParty(
        address authority,
        Party.PartyOptions memory opts,
        IERC721[] memory preciousTokens,
        uint256[] memory preciousTokenIds
    )
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyFactory.sol#L26

	function _initialize(
        GovernanceOpts memory opts,
        IERC721[] memory preciousTokens,
        uint256[] memory preciousTokenIds
    )
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L271

	function execute(
        uint256 proposalId,
        Proposal memory proposal,
        IERC721[] memory preciousTokens,
        uint256[] memory preciousTokenIds,
        bytes calldata progressData,
        bytes calldata extraData
    )
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L653

	function _executeProposal(
        uint256 proposalId,
        Proposal memory proposal,
        IERC721[] memory preciousTokens,
        uint256[] memory preciousTokenIds,
        uint256 flags,
        bytes memory progressData,
        bytes memory extraData
    )
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L804

	function _setPreciousList(
        IERC721[] memory preciousTokens,
        uint256[] memory preciousTokenIds
    )
        private
    {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1082

	function _isPreciousListCorrect(
        IERC721[] memory preciousTokens,
        uint256[] memory preciousTokenIds
    )
        private
        view
        returns (bool)
    {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1094

	function _hashPreciousList(
        IERC721[] memory preciousTokens,
        uint256[] memory preciousTokenIds
    )
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1105

	function _initialize(
        string memory name_,
        string memory symbol_,
        PartyGovernance.GovernanceOpts memory governanceOpts,
        IERC721[] memory preciousTokens,
        uint256[] memory preciousTokenIds,
        address mintAuthority_
    )
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernanceNFT.sol#L49

	 function _executeSingleArbitraryCall(
        uint256 idx,
        ArbitraryCall memory call,
        IERC721[] memory preciousTokens,
        uint256[] memory preciousTokenIds,
        bool isUnanimous,
        uint256 ethAvailable
    )
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L93

	function _isCallAllowed(
        ArbitraryCall memory call,
        bool isUnanimous,
        IERC721[] memory preciousTokens,
        uint256[] memory preciousTokenIds
    )
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L142

	function isTokenPrecious(IERC721 token, IERC721[] memory preciousTokens)
        internal
        pure
        returns (bool)
    {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L9

	function isTokenIdPrecious(
        IERC721 token,
        uint256 tokenId,
        IERC721[] memory preciousTokens,
        uint256[] memory preciousTokenIds
    )
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L22

	function _listOnOpensea(
        IERC721 token,
        uint256 tokenId,
        uint256 listPrice,
        uint256 expiry,
        uint256[] memory fees,
        address payable[] memory feeRecipients
    )
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L238



## (5) Empty Blocks Should Be Removed Or Emit Something

Severity: Gas Optimizations

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. If the contract is meant to be extended, the contract should be abstract and the function signatures be added without any default implementation. If the block is an empty if-statement block to avoid doing subsequent checks in the else-if/else conditions, the else-if/else conditions should be nested under the negation of the if-statement, because they involve different classes of checks, which may lead to the introduction of errors when the code is later modified (if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}})

## Proof Of Concept

	receive() external payable {}
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L143

	receive() external payable {}
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/Party.sol#L47




## (6) It Costs More Gas To Initialize Variables To Zero Than To Let The Default Of Zero Be Applied

Severity: Gas Optimizations

## Proof Of Concept


	for (uint256 i = 0; i < contributors.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L180

	for (uint256 i = 0; i < numContributions; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L242

	for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L300

	for (uint256 i = 0; i < numContributions; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L348

	for (uint256 i = 0; i < infos.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L230

	for (uint256 i = 0; i < infos.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L239

	for (uint256 i=0; i < opts.hosts.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L306

	for (uint256 i = 0; i < hadPreciouses.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L52

	for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L14

	for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L32

	for (uint256 i = 0; i < fees.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L291






## (7) Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate

Severity: Gas Optimizations

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

## Proof Of Concept


	mapping(address => bool) public isHost;
    mapping(address => address) public delegationsByVoter;
    mapping(address => VotingPowerSnapshot[]) private _votingPowerSnapshotsByVoter;

https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L207


## (8) Multiplication/division By Two Should Use Bit Shifting

Severity: Gas Optimizations

<x> * 2 is equivalent to <x> << 1 and <x> / 2 is the same as <x> >> 1. The MUL and DIV opcodes cost 5 gas, whereas SHL and SHR only cost 3 gas

## Proof Of Concept


	uint256 mid = (low + high) / 2;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L434


## (9) Not Using The Named Return Variables When A Function Returns, Wastes Deployment Gas

Severity: Gas Optimizations

## Proof Of Concept

    function _getFinalPrice()
        internal
        override
        view
        returns (uint256 price)
    {
        return lastBid;
    }
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L275

    function _prepareGate(
        IGateKeeper gateKeeper,
        bytes12 gateKeeperId,
        bytes memory createGateCallData
    )
        private
        returns (bytes12 newGateKeeperId)
    {
        if (
            address(gateKeeper) == address(0) ||
            gateKeeperId != bytes12(0)
        ) {
            // Using an existing gate on the gatekeeper
            // or not using a gate at all.
            return gateKeeperId;
        }
        // Call the gate creation function on the gatekeeper.
        (bool s, bytes memory r) = address(gateKeeper).call(createGateCallData);
        if (!s) {
            r.rawRevert();
        }
        // Result is always a bytes12.
        return abi.decode(r, (bytes12));
    }
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundFactory.sol#L107

    function _getBalanceId(TokenType tokenType, address token)
        private
        pure
        returns (bytes32 balanceId)
    {
        if (tokenType == TokenType.Native) {
            return bytes32(uint256(uint160(NATIVE_TOKEN_ADDRESS)));
        }
        assert(tokenType == TokenType.Erc20);
        return bytes32(uint256(uint160(token)));
    }
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L403



## (10) <x> += <y> Costs More Gas Than <x> = <x> + <y> For State Variables

Severity: Gas Optimizations

## Proof Of Concept


	ethContributed += contributions[i].amount;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L243

	ethOwed += c.amount;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L352

	ethUsed += c.amount;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L355

	ethUsed += partialEthUsed;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L359

	votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L374

	totalContributions += amount;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L411

	lastContribution.amount += amount;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L427

	_storedBalances[balanceId] -= amount;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L381

	values.votes += votingPower;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L595

	newDelegateDelegatedVotingPower -= oldSnap.intrinsicVotingPower;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L959

	ethAvailable -= calls[i].value;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L72





## (11) Public Functions To External

Severity: Gas Optimizations

The following functions could be set external to save gas and improve code quality.
External call cost is less expensive than of public functions.

## Proof Of Concept


	function getCrowdfundLifecycle() public override view returns (CrowdfundLifecycle) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L254

	function getCrowdfundLifecycle() public override view returns (CrowdfundLifecycle) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L150

	function getCrowdfundLifecycle() public virtual view returns (CrowdfundLifecycle);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L251

	function tokenURI(uint256) public override view returns (string memory) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernanceNFT.sol#L88






## (12) Help The Optimizer By Saving A Storage Variable’s Reference Instead Of Repeatedly Fetching It

Severity: Gas Optimizations

To help the optimizer, declare a storage type variable and use it instead of repeatedly fetching the reference in a map or an array.
The effect can be quite significant.
As an example, instead of repeatedly calling someMap[someIndex], save its reference like this: SomeStruct storage someStruct = someMap[someIndex] and use it.

## Proof Of Concept


	Contribution[] storage contributions = _contributionsByContributor[contributor];
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L240

	Contribution[] storage contributions = _contributionsByContributor[contributor];
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L346

	Contribution[] storage contributions = _contributionsByContributor[contributor];
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L421

	address delegate = delegationsByContributor[contributor];
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L473

	owner = _owners[tokenId];
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CrowdfundNFT.sol#L126

	if (state.hasPartyTokenClaimed[partyTokenId]) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L155




## (13) Use of non-uint64 state variable is less efficient than uint256

Severity: Gas Optimizations

Lower than uint256 size storage variables are less gas efficient.
Using uint64 does not give any efficiency, actually, it is the opposite as EVM operates on default of 256-bit values so uint64 is more expensive in this case as it needs a conversion. It only gives improvements in cases where you can pack variables together, e.g. structs.

## Proof Of Concept


	uint40 duration;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L48

	uint96 maximumBid;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L50

	uint16 splitBps;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L56

	uint96 public maximumBid;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L93

	uint96 public lastBid;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L95

	uint40 public expiry;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L98

	uint96 bidAmount = market.getMinimumBid(auctionId_).safeCastUint256ToUint96();
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L169

	uint96 lastBid_ = lastBid;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/AuctionCrowdfund.sol#L211

	uint40 duration;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfund.sol#L30

	uint96 maximumPrice;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfund.sol#L33

	uint16 splitBps;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfund.sol#L39

	uint40 duration;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L27

	uint96 maximumPrice;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L30

	uint16 splitBps;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L36

	uint40 public expiry;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L60

	uint96 public maximumPrice;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L62

	uint96 public settledPrice;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L64

	uint96 settledPrice_;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L114

	uint96 maximumPrice_ = maximumPrice;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/BuyCrowdfundBase.sol#L116

	uint40 duration;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L31

	uint96 maximumPrice;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L34

	uint16 splitBps;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L40

	uint40 voteDuration;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L37

	uint40 executionDelay;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L40

	uint16 passThresholdBps;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L43

	uint16 feeBps;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L45

	uint16 splitBps;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L55

	uint96 previousTotalContributions;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L67

	uint96 amount;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L69

	uint96 public totalContributions;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L93

	uint16 public splitBps;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L104

	uint96 initialBalance = address(this).balance.safeCastUint256ToUint96();
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L143

	uint128 memberSupply;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/ITokenDistributor.sol#L28

	uint128 fee;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/ITokenDistributor.sol#L30

	uint128 remainingMemberSupply;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L24

	uint16 feeBps;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L40

	uint128 remainingMemberSupply = state.remainingMemberSupply;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L166

	uint128 supply;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L338

	uint128 fee = supply * args.feeBps / 1e4;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L352

	uint128 memberSupply = supply - fee;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L353

	uint40 voteDuration;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L75

	uint40 executionDelay;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L78

	uint16 passThresholdBps;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L81

	uint96 totalVotingPower;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L83

	uint16 feeBps;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L85

	uint40 timestamp;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L102

	uint96 delegatedVotingPower;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L104

	uint96 intrinsicVotingPower;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L106

	uint40 maxExecutableTime;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L116

	uint40 cancelDelay;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L119

	uint40 proposedTime;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L130

	uint40 passedTime;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L132

	uint40 executedTime;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L134

	uint40 completedTime;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L136

	uint96 votes;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L138

	uint96 constant private VETO_VALUE = uint96(int96(-1));
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L190

	uint16 public feeBps;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L199

	uint96 votingPower = getVotingPowerAt(msg.sender, values.proposedTime, snapIndex);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L594

	uint40 t = uint40(block.timestamp);
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L1036

	uint40 duration;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L36

	uint40 expiry;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L52

	uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_ZORA_AUCTION_TIMEOUT));
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L152

	uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_ZORA_AUCTION_DURATION));
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L154

	uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MIN_ORDER_DURATION));
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L202

	uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MAX_ORDER_DURATION));
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L203

	uint40 timeout;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L34

	uint40 duration;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L36

	uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MIN_AUCTION_DURATION));
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L94

	uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_DURATION));
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L95

	uint40 maxTimeout = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_TIMEOUT));
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnZoraProposal.sol#L102

	uint8 curatorFeePercentage;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/vendor/markets/IZoraAuctionHouse.sol#L25






## (14) ++i/i++ Should Be Unchecked{++i}/unchecked{i++} When It Is Not Possible For Them To Overflow, As Is The Case When Used In For- And While-loops

Severity: Gas Optimizations

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas PER LOOP

## Proof Of Concept


	for (uint256 i = 0; i < contributors.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L180

	for (uint256 i = 0; i < numContributions; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L242

	for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L300

	for (uint256 i = 0; i < numContributions; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/crowdfund/Crowdfund.sol#L348

	for (uint256 i = 0; i < infos.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L230

	for (uint256 i = 0; i < infos.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L239

	for (uint256 i=0; i < opts.hosts.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L306

	for (uint256 i = 0; i < hadPreciouses.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L52

	for (uint256 i = 0; i < calls.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ArbitraryCallsProposal.sol#L61

	for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L14

	for (uint256 i = 0; i < preciousTokens.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/LibProposal.sol#L32

	for (uint256 i = 0; i < fees.length; ++i) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/proposals/ListOnOpenseaProposal.sol#L291





## (15) Use assembly to check for address(0)

Severity: Gas Optimizations

Save 6 gas per instance if using assembly to check for address(0)

e.g.
assembly {
	if iszero(_addr) {
		mstore(0x00, "AddressZero")
		revert(0x00, 0x20)
	}
}

## Proof Of Concept


	if (newPartyHost != address(0)) {
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L460






## (16) Using Bools For Storage Incurs Overhead

Severity: Gas Optimizations

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

## Proof Of Concept

    mapping(uint256 => bool) hasPartyTokenClaimed;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/distribution/TokenDistributor.sol#L30

    mapping(uint256 => mapping(bytes32 => bool)) private _includedWordValues;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/globals/Globals.sol#L12

    mapping (address => bool) hasVoted;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L148

    mapping(address => bool) public isHost;
https://github.com/PartyDAO/party-contracts-c4/tree/main/contracts/party/PartyGovernance.sol#L207


