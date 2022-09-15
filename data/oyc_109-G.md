## [G-01] Don't Initialize Variables with Default Value

Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it's default value costs unnecesary gas.

```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::180 => for (uint256 i = 0; i < contributors.length; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::242 => for (uint256 i = 0; i < numContributions; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::300 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::348 => for (uint256 i = 0; i < numContributions; ++i) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::230 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::239 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4/contracts/party/PartyGovernance.sol::306 => for (uint256 i=0; i < opts.hosts.length; ++i) {
party-contracts-c4/contracts/party/PartyGovernance.sol::432 => uint256 low = 0;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::52 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::61 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::78 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
party-contracts-c4/contracts/proposals/LibProposal.sol::14 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/proposals/LibProposal.sol::32 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::291 => for (uint256 i = 0; i < fees.length; ++i) {
```

## [G-02] Cache Array Length Outside of Loop

Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

```
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::62 => for (uint256 i; i < hosts.length; i++) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::180 => for (uint256 i = 0; i < contributors.length; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::300 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::230 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::239 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4/contracts/party/PartyGovernance.sol::306 => for (uint256 i=0; i < opts.hosts.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::52 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::61 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::78 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
party-contracts-c4/contracts/proposals/LibProposal.sol::14 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/proposals/LibProposal.sol::32 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::291 => for (uint256 i = 0; i < fees.length; ++i) {
```

## [G-03] Use Shift Right/Left instead of Division/Multiplication if possible

A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

```
party-contracts-c4/contracts/party/PartyGovernance.sol::434 => uint256 mid = (low + high) / 2;
```

## [G-04] Using private rather than public for constants, saves gas

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

```
party-contracts-c4/contracts/distribution/TokenDistributor.sol::59 => IGlobals public immutable GLOBALS;
party-contracts-c4/contracts/party/PartyFactory.sol::18 => IGlobals public immutable GLOBALS;
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol::31 => IFractionalV1VaultFactory public immutable VAULT_FACTORY;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::100 => IOpenseaExchange public immutable SEAPORT;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::102 => IOpenseaConduitController public immutable CONDUIT_CONTROLLER;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::64 => IZoraAuctionHouse public immutable ZORA;
party-contracts-c4/contracts/utils/Implementation.sol::9 => address public immutable IMPL;
party-contracts-c4/contracts/utils/Proxy.sol::12 => Implementation public immutable IMPL;
```

## [G-05] Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::149 => function bid() external onlyDelegateCall {
party-contracts-c4/contracts/globals/Globals.sol::27 => function transferMultiSig(address newMultiSig) external onlyMultisig {
party-contracts-c4/contracts/globals/Globals.sol::59 => function setBytes32(uint256 key, bytes32 value) external onlyMultisig {
party-contracts-c4/contracts/globals/Globals.sol::63 => function setUint256(uint256 key, uint256 value) external onlyMultisig {
party-contracts-c4/contracts/globals/Globals.sol::67 => function setAddress(uint256 key, address value) external onlyMultisig {
party-contracts-c4/contracts/globals/Globals.sol::71 => function setIncludesBytes32(uint256 key, bytes32 value, bool isIncluded) external onlyMultisig {
party-contracts-c4/contracts/globals/Globals.sol::75 => function setIncludesUint256(uint256 key, uint256 value, bool isIncluded) external onlyMultisig {
party-contracts-c4/contracts/globals/Globals.sol::79 => function setIncludesAddress(uint256 key, address value, bool isIncluded) external onlyMultisig {
party-contracts-c4/contracts/party/PartyGovernance.sol::451 => function delegateVotingPower(address delegate) external onlyDelegateCall {
party-contracts-c4/contracts/party/PartyGovernance.sol::458 => function abdicate(address newPartyHost) external onlyHost onlyDelegateCall {
party-contracts-c4/contracts/party/PartyGovernance.sol::616 => function veto(uint256 proposalId) external onlyHost onlyDelegateCall {
party-contracts-c4/contracts/party/PartyGovernance.sol::800 => function disableEmergencyExecute() external onlyPartyDaoOrHost onlyDelegateCall {
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::169 => function abdicate() external onlyMinter onlyDelegateCall {
```

## [G-06] Empty blocks should be removed or emit something

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. 

```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::104 => constructor(IGlobals globals) Crowdfund(globals) {}
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::144 => receive() external payable {}
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::62 => constructor(IGlobals globals) BuyCrowdfundBase(globals) {}
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::67 => constructor(IGlobals globals) Crowdfund(globals) {}
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::77 => constructor(IGlobals globals) BuyCrowdfundBase(globals) {}
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::52 => {}
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::59 => {}
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::66 => {}
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::73 => {}
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::80 => {}
party-contracts-c4/contracts/party/Party.sol::28 => constructor(IGlobals globals) PartyGovernanceNFT(globals) {}
party-contracts-c4/contracts/party/Party.sol::47 => receive() external payable {}
```

## [G-07] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::56 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::39 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::36 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::40 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::43 => uint16 passThresholdBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::45 => uint16 feeBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::55 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::104 => uint16 public splitBps;
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::151 => uint256 tokenId = uint256(uint160(owner));
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::28 => uint128 memberSupply;
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::30 => uint128 fee;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::24 => uint128 remainingMemberSupply;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::40 => uint16 feeBps;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::166 => uint128 remainingMemberSupply = state.remainingMemberSupply;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::338 => uint128 supply;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::352 => uint128 fee = supply * args.feeBps / 1e4;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::353 => uint128 memberSupply = supply - fee;
party-contracts-c4/contracts/party/PartyGovernance.sol::81 => uint16 passThresholdBps;
party-contracts-c4/contracts/party/PartyGovernance.sol::85 => uint16 feeBps;
party-contracts-c4/contracts/party/PartyGovernance.sol::95 => uint16 passThresholdBps;
party-contracts-c4/contracts/party/PartyGovernance.sol::199 => uint16 public feeBps;
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol::25 => uint8 curatorFeePercentage;
```

## [G-08] Using bools for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.
Use uint256(1) and uint256(2) for true/false instead

```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::106 => bool private _splitRecipientHasBurned;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::62 => bool public emergencyActionsDisabled;
party-contracts-c4/contracts/globals/Globals.sol::12 => mapping(uint256 => mapping(bytes32 => bool)) private _includedWordValues;
party-contracts-c4/contracts/party/PartyGovernance.sol::197 => bool public emergencyExecuteDisabled;
party-contracts-c4/contracts/party/PartyGovernance.sol::207 => mapping(address => bool) public isHost;
```

## [G-09] ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, for example when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop

```
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::62 => for (uint256 i; i < hosts.length; i++) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::180 => for (uint256 i = 0; i < contributors.length; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::242 => for (uint256 i = 0; i < numContributions; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::300 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::348 => for (uint256 i = 0; i < numContributions; ++i) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::230 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::239 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::357 => distributionId: ++lastDistributionIdPerParty[args.party],
party-contracts-c4/contracts/party/PartyGovernance.sol::306 => for (uint256 i=0; i < opts.hosts.length; ++i) {
party-contracts-c4/contracts/party/PartyGovernance.sol::532 => proposalId = ++lastProposalId;
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::129 => uint256 tokenId = ++tokenCount;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::52 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::61 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::78 => for (uint256 i = 0; i < hadPreciouses.length; ++i) {
party-contracts-c4/contracts/proposals/LibProposal.sol::14 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/proposals/LibProposal.sol::32 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::291 => for (uint256 i = 0; i < fees.length; ++i) {
```

## [G-10] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

use <x> = <x> + <y> or <x> = <x> - <y> instead to save gas

```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::243 => ethContributed += contributions[i].amount;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::352 => ethOwed += c.amount;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::355 => ethUsed += c.amount;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::359 => ethUsed += partialEthUsed;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::374 => votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::411 => totalContributions += amount;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::427 => lastContribution.amount += amount;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::381 => _storedBalances[balanceId] -= amount;
party-contracts-c4/contracts/party/PartyGovernance.sol::595 => values.votes += votingPower;
party-contracts-c4/contracts/party/PartyGovernance.sol::959 => newDelegateDelegatedVotingPower -= oldSnap.intrinsicVotingPower;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::72 => ethAvailable -= calls[i].value;
```

## [G-11] abi.encode() is less efficient than abi.encodePacked()

use abi.encodePacked() where possible to save gas

```
party-contracts-c4/contracts/party/PartyGovernance.sol::400 => // keccak256(abi.encode(
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::164 => return abi.encode(ListOnOpenseaStep.ListedOnZora, ZoraProgressData({
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::219 => return abi.encode(ListOnOpenseaStep.ListedOnOpenSea, orderHash, expiry);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::115 => return abi.encode(ZoraStep.ListedOnZora, ZoraProgressData({
party-contracts-c4/contracts/renderers/DummyERC721Renderer.sol::9 => return string(abi.encode(address(this), tokenId));
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::23 => abi.encode(s, r).rawRevert();
```

## [G-12] Do not calculate constants

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.
Consequences: each usage of a constant costs more gas on each access. Since these are not real constants, they can't be referenced from a real constant environment (e.g. from assembly, or from another library)

```
party-contracts-c4/contracts/party/PartyGovernance.sol::190 => uint96 constant private VETO_VALUE = uint96(int96(-1));
```

## [G-13] Prefix increments cheaper than Postfix increments

++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too)
Saves 5 gas PER LOOP

```
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::62 => for (uint256 i; i < hosts.length; i++) {
```

## [G-14] Usage of assert() instead of require()

Between solc 0.4.10 and 0.8.0, require() used REVERT (0xfd) opcode which refunded remaining gas on failure while assert() used INVALID (0xfe) opcode which consumed all the supplied gas.
require() should be used for checking error conditions on inputs and return values while assert() should be used for invariant checking (properly functioning code should never reach a failing assert statement, unless there is a bug in your contract you should fix).

```
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::166 => assert(false); // Will not be reached.
party-contracts-c4/contracts/distribution/TokenDistributor.sol::385 => assert(tokenType == TokenType.Erc20);
party-contracts-c4/contracts/distribution/TokenDistributor.sol::411 => assert(tokenType == TokenType.Erc20);
party-contracts-c4/contracts/party/PartyGovernance.sol::504 => assert(tokenType == ITokenDistributor.TokenType.Erc20);
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::179 => assert(false); // Will not be reached.
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol::67 => assert(vault.balanceOf(address(this)) == supply);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::221 => assert(step == ListOnOpenseaStep.ListedOnOpenSea);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::302 => assert(SEAPORT.validate(orders));
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::120 => assert(step == ZoraStep.ListedOnZora);
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::142 => assert(currentInProgressProposalId == 0);
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::30 => assert(false);
```

## [G-15] Public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents' functions and change the visibility from external to public and can save gas by doing so.

```
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::88 => function tokenURI(uint256) public override view returns (string memory) {
```

## [G-16] Not using the named return variables when a function returns, wastes deployment gas

It is not necessary to have both a named return and a return statement.

```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::287 => returns (uint256 price)
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::113 => returns (bytes12 newGateKeeperId)
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::133 => function balanceOf(address owner) external view returns (uint256 numTokens) {
party-contracts-c4/contracts/distribution/TokenDistributor.sol::406 => returns (bytes32 balanceId)
party-contracts-c4/contracts/party/PartyGovernance.sol::350 => returns (uint96 votingPower)
party-contracts-c4/contracts/party/PartyGovernance.sol::364 => returns (uint96 votingPower)
party-contracts-c4/contracts/party/PartyGovernance.sol::425 => returns (uint256 index)
party-contracts-c4/contracts/party/PartyGovernance.sol::565 => returns (uint256 totalVotes)
party-contracts-c4/contracts/party/PartyGovernance.sol::814 => returns (bool completed)
party-contracts-c4/contracts/party/PartyGovernance.sol::1015 => returns (ProposalStatus status)
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::70 => returns (address owner)
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::104 => returns (address, uint256)
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::150 => returns (bool isAllowed)
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::172 => returns (bool sold)
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol::110 => returns (uint256 id)
party-contracts-c4/contracts/proposals/ProposalStorage.sol::24 => returns (IProposalExecutionEngine impl)
```

## [G-17] Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

```
party-contracts-c4/contracts/party/PartyGovernance.sol::207 => mapping(address => bool) public isHost;
party-contracts-c4/contracts/party/PartyGovernance.sol::209 => mapping(address => address) public delegationsByVoter;
party-contracts-c4/contracts/party/PartyGovernance.sol::215 => mapping(address => VotingPowerSnapshot[]) private _votingPowerSnapshotsByVoter;
```

## [G-18] Use assembly to check for address(0)

Saves 6 gas per instance if using assembly to check for address(0)

e.g.
```
assembly {
 if iszero(_addr) {
  mstore(0x00, "zero address")
  revert(0x00, 0x20)
 }
}
```

instances:

```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::270 => return address(party) != address(0)
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::153 => return address(party) != address(0)
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::138 => return _owners[uint256(uint160(owner))] != address(0);
party-contracts-c4/contracts/party/PartyGovernance.sol::460 => if (newPartyHost != address(0)) {
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::176 => if (op != address(0)) {
```

## [G-19] Use selfbalance()

Use selfbalance() instead of address(this).balance when getting your contract's balance of ETH to save gas.

```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::143 => uint96 initialBalance = address(this).balance.safeCastUint256ToUint96();
party-contracts-c4/contracts/party/PartyGovernance.sol::501 => { value: address(this).balance }(this, feeRecipient, feeBps);
```

## [G-20] internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::491 => function _getPartyFactory() internal view returns (IPartyFactory) {
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::137 => function _doesTokenExistFor(address owner) internal view returns (bool) {
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::141 => function _mint(address owner) internal returns (uint256 tokenId)
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::150 => function _burn(address owner) internal {
party-contracts-c4/contracts/party/PartyGovernance.sol::916 => function _getTotalVotingPower() internal view returns (uint256) {
party-contracts-c4/contracts/proposals/ProposalStorage.sol::29 => function _setProposalExecutionEngine(IProposalExecutionEngine impl) internal {
party-contracts-c4/contracts/utils/LibSafeCast.sol::12 => function safeCastUint256ToUint96(uint256 v) internal pure returns (uint96) {
party-contracts-c4/contracts/utils/LibSafeCast.sol::19 => function safeCastUint256ToUint128(uint256 v) internal pure returns (uint128) {
party-contracts-c4/contracts/utils/LibSafeCast.sol::26 => function safeCastUint256ToInt192(uint256 v) internal pure returns (int192) {
party-contracts-c4/contracts/utils/LibSafeCast.sol::33 => function safeCastUint96ToInt192(uint96 v) internal pure returns (int192) {
party-contracts-c4/contracts/utils/LibSafeCast.sol::37 => function safeCastInt192ToUint96(int192 i192) internal pure returns (uint96) {
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::27 => function _readOnlyDelegateCall(address impl, bytes memory callData) internal view {
```

## [G-21] internal functions not called by the contract should be removed to save deployment gas

If the functions are required by an interface, the contract should inherit from that interface and use the override keyword

```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::491 => function _getPartyFactory() internal view returns (IPartyFactory) {
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::141 => function _mint(address owner) internal returns (uint256 tokenId)
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::150 => function _burn(address owner) internal {
party-contracts-c4/contracts/party/PartyGovernance.sol::916 => function _getTotalVotingPower() internal view returns (uint256) {
party-contracts-c4/contracts/proposals/ProposalStorage.sol::29 => function _setProposalExecutionEngine(IProposalExecutionEngine impl) internal {
party-contracts-c4/contracts/utils/LibSafeCast.sol::12 => function safeCastUint256ToUint96(uint256 v) internal pure returns (uint96) {
party-contracts-c4/contracts/utils/LibSafeCast.sol::19 => function safeCastUint256ToUint128(uint256 v) internal pure returns (uint128) {
party-contracts-c4/contracts/utils/LibSafeCast.sol::26 => function safeCastUint256ToInt192(uint256 v) internal pure returns (int192) {
party-contracts-c4/contracts/utils/LibSafeCast.sol::33 => function safeCastUint96ToInt192(uint96 v) internal pure returns (int192) {
party-contracts-c4/contracts/utils/LibSafeCast.sol::37 => function safeCastInt192ToUint96(int192 i192) internal pure returns (uint96) {
```