## [G001] Don't Initialize Variables with Default Value

#### Instances: `43` found
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
party-contracts-c4/contracts/utils/PartyHelpers.sol::64 => for (uint256 i = 0; i < members.length; i++) {
party-contracts-c4/contracts/utils/PartyHelpers.sol::85 => for (uint256 i = 0; i < voters.length; i++) {
party-contracts-c4/contracts/utils/PartyHelpers.sol::115 => for (uint256 i = 0; i < count; i++) {
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::80 => for (uint256 i = 0; i < ids.length; ) {
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::118 => for (uint256 i = 0; i < owners.length; ++i) {
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::168 => for (uint256 i = 0; i < idsLength; ) {
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::198 => for (uint256 i = 0; i < idsLength; ) {
party-contracts-c4/deploy/deploy.sol::310 => for (uint256 i=0; i < parts.length; ++i) {
party-contracts-c4/sol-tests/crowdfund/Crowdfund.t.sol::77 => for (uint256 i = 0; i < count; ++i) {
party-contracts-c4/sol-tests/distribution/TokenDistributorUnit.t.sol::373 => for (uint256 i = 0; i < members.length; ++i) {
party-contracts-c4/sol-tests/distribution/TokenDistributorUnit.t.sol::392 => for (uint256 i = 0; i < members.length; ++i) {
party-contracts-c4/sol-tests/distribution/TokenDistributorUnit.t.sol::454 => for (uint256 i = 0; i < 8; ++i) {
party-contracts-c4/sol-tests/distribution/TokenDistributorUnit.t.sol::662 => uint256 sharesSum = 0;
party-contracts-c4/sol-tests/distribution/TokenDistributorUnit.t.sol::663 => for (uint256 i = 0; i < count; ++i) {
party-contracts-c4/sol-tests/distribution/TokenDistributorUnit.t.sol::666 => for (uint256 i = 0; i < count; ++i) {
party-contracts-c4/sol-tests/distribution/TokenDistributorUnit.t.sol::669 => for (uint256 i = 0; i < count; ++i) {
party-contracts-c4/sol-tests/gatekeepers/AllowListGateKeeper.t.sol::14 => for (uint i = 0; i < 4; i++) {
party-contracts-c4/sol-tests/party/PartyFactory.t.sol::47 => for (uint256 i = 0; i < count; ++i) {
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::341 => for (uint256 i = 0; i < count; ++i) {
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::85 => for (uint256 i = 0; i < 2; ++i) {
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::135 => for (uint256 i = 0; i < count; ++i) {
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::159 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::175 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::191 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::225 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::243 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::112 => for (uint256 i = 0; i < count; ++i) {
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::189 => for (uint256 i = 0; i < data.fees.length; ++i) {
party-contracts-c4/sol-tests/proposals/OpenseaTestUtils.sol::52 => for (uint256 i = 0; i < fees.length; ++i) {
party-contracts-c4/sol-tests/proposals/OpenseaTestUtils.sol::94 => for (uint256 i = 0; i < fees.length; ++i) {
```
## [G002] Cache Array Length Outside of Loop

#### Instances: `27` found
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
party-contracts-c4/contracts/utils/PartyHelpers.sol::64 => for (uint256 i = 0; i < members.length; i++) {
party-contracts-c4/contracts/utils/PartyHelpers.sol::85 => for (uint256 i = 0; i < voters.length; i++) {
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::80 => for (uint256 i = 0; i < ids.length; ) {
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::118 => for (uint256 i = 0; i < owners.length; ++i) {
party-contracts-c4/deploy/deploy.sol::79 => for (i = 0; i < deployConstants.adminAddresses.length; ++i) {
party-contracts-c4/deploy/deploy.sol::310 => for (uint256 i=0; i < parts.length; ++i) {
party-contracts-c4/sol-tests/distribution/TokenDistributorUnit.t.sol::373 => for (uint256 i = 0; i < members.length; ++i) {
party-contracts-c4/sol-tests/distribution/TokenDistributorUnit.t.sol::392 => for (uint256 i = 0; i < members.length; ++i) {
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::159 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::175 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::191 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::225 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::243 => for (uint256 i = 0; i < calls.length; ++i) {
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::189 => for (uint256 i = 0; i < data.fees.length; ++i) {
party-contracts-c4/sol-tests/proposals/OpenseaTestUtils.sol::52 => for (uint256 i = 0; i < fees.length; ++i) {
party-contracts-c4/sol-tests/proposals/OpenseaTestUtils.sol::94 => for (uint256 i = 0; i < fees.length; ++i) {
```
## [G003] Use != 0 instead of > 0 for Unsigned Integer Comparison

`!= 0` costs less gas compared to `> 0` for unsigned integers in require statements with the optimizer enabled (6 gas)
For uints the minimum value would be 0 and never a negative value. Since it cannot be a negative value, then the check `> 0` is essentially checking that the value is not equal to 0 therefore `>0` can be replaced with `!=0` which saves gas.
#### Instances: `2` found
```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::144 => if (initialBalance > 0) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::471 => if (votingPower > 0) {
```
## [G004] >= costs less gas than >

he compiler uses opcodes `GT` and `ISZERO` for solidity code that uses `>`, but only requires `LT` for `>=`, which saves 3 gas
#### Instances: `2` found
```
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::144 => if (initialBalance > 0) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::471 => if (votingPower > 0) {
```
## [G005] `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

#### Instances: `24` found
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
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::51 => balanceOf[from][id] -= amount;
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::52 => balanceOf[to][id] += amount;
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::84 => balanceOf[from][id] -= amount;
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::85 => balanceOf[to][id] += amount;
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::145 => balanceOf[to][id] += amount;
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::169 => balanceOf[to][ids[i]] += amounts[i];
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::199 => balanceOf[from][ids[i]] -= amounts[i];
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::216 => balanceOf[from][id] -= amount;
party-contracts-c4/sol-tests/crowdfund/Crowdfund.t.sol::612 => defaultGovernanceOpts.voteDuration += 1;
party-contracts-c4/sol-tests/crowdfund/Crowdfund.t.sol::614 => defaultGovernanceOpts.executionDelay += 1;
party-contracts-c4/sol-tests/crowdfund/Crowdfund.t.sol::616 => defaultGovernanceOpts.passThresholdBps += 1;
party-contracts-c4/sol-tests/proposals/DummyCallTarget.sol::11 => return _x += x;
party-contracts-c4/sol-tests/proposals/DummySimpleProposalEngineImpl.sol::48 => _getStorage().numExecutedProposals += 1;
party-contracts-c4/sol-tests/proposals/OpenseaTestUtils.sol::53 => totalValue += fees[i];
```
## [G006] `++i`/`i++` should be `unchecked{++i}`/`unchecked{i++}` when it is not possible for them to overflow, as is the case when used in `for`- and `while`-loops

The `unchecked` keyword is new in solidity version 0.8.0, so this applies to that version or higher, which these instances are. This saves 30-40 gas PER LOOP
#### Instances: `6` found
```
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::62 => for (uint256 i; i < hosts.length; i++) {
party-contracts-c4/contracts/utils/PartyHelpers.sol::64 => for (uint256 i = 0; i < members.length; i++) {
party-contracts-c4/contracts/utils/PartyHelpers.sol::85 => for (uint256 i = 0; i < voters.length; i++) {
party-contracts-c4/contracts/utils/PartyHelpers.sol::115 => for (uint256 i = 0; i < count; i++) {
party-contracts-c4/sol-tests/distribution/TokenDistributor.t.sol::32 => for (uint160 i; i < 10; i++) {
party-contracts-c4/sol-tests/gatekeepers/AllowListGateKeeper.t.sol::14 => for (uint i = 0; i < 4; i++) {
```
## [G007] Using `bools` for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.Refer https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27 . Use uint256(1) and uint256(2) for true/false
#### Instances: `5` found
```
party-contracts-c4/contracts/globals/Globals.sol::12 => mapping(uint256 => mapping(bytes32 => bool)) private _includedWordValues;
party-contracts-c4/contracts/party/PartyGovernance.sol::207 => mapping(address => bool) public isHost;
party-contracts-c4/contracts/vendor/solmate/ERC1155.sol::24 => mapping(address => mapping(address => bool)) public isApprovedForAll;
party-contracts-c4/contracts/vendor/solmate/ERC721.sol::50 => mapping(address => mapping(address => bool)) public isApprovedForAll;
party-contracts-c4/sol-tests/DummyERC721.sol::11 => mapping (address => mapping (address => bool)) public isApprovedForAll;
```
## [G008] ++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too)

Saves 6 gas PER LOOP
#### Instances: `6` found
```
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::62 => for (uint256 i; i < hosts.length; i++) {
party-contracts-c4/contracts/utils/PartyHelpers.sol::64 => for (uint256 i = 0; i < members.length; i++) {
party-contracts-c4/contracts/utils/PartyHelpers.sol::85 => for (uint256 i = 0; i < voters.length; i++) {
party-contracts-c4/contracts/utils/PartyHelpers.sol::115 => for (uint256 i = 0; i < count; i++) {
party-contracts-c4/sol-tests/distribution/TokenDistributor.t.sol::32 => for (uint160 i; i < 10; i++) {
party-contracts-c4/sol-tests/gatekeepers/AllowListGateKeeper.t.sol::14 => for (uint i = 0; i < 4; i++) {
```
## [G009] abi.encode() is less efficient than abi.encodePacked()

#### Instances: `47` found
```
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::164 => return abi.encode(ListOnOpenseaStep.ListedOnZora, ZoraProgressData({
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::219 => return abi.encode(ListOnOpenseaStep.ListedOnOpenSea, orderHash, expiry);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::115 => return abi.encode(ZoraStep.ListedOnZora, ZoraProgressData({
party-contracts-c4/contracts/renderers/DummyERC721Renderer.sol::9 => return string(abi.encode(address(this), tokenId));
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol::23 => abi.encode(s, r).rawRevert();
party-contracts-c4/sol-tests/TestUtils.sol::12 => _nonce = uint256(keccak256(abi.encode(
party-contracts-c4/sol-tests/TestUtils.sol::34 => bytes memory seed = abi.encode(
party-contracts-c4/sol-tests/crowdfund/Crowdfund.t.sol::792 => cf.contribute{ value: contributor1.balance }(delegate1, abi.encode(new bytes32[](0)));
party-contracts-c4/sol-tests/crowdfund/Crowdfund.t.sol::802 => abi.encode(new bytes32[](0))
party-contracts-c4/sol-tests/crowdfund/Crowdfund.t.sol::804 => cf.contribute{ value: contributor2.balance }(delegate2, abi.encode(new bytes32[](0)));
party-contracts-c4/sol-tests/distribution/TokenDistributorUnit.t.sol::452 => assertEq(abi.encode(di).length / 32, 7);
party-contracts-c4/sol-tests/distribution/TokenDistributorUnit.t.sol::611 => bytes15 expectedHash = bytes15(keccak256(abi.encode(di)));
party-contracts-c4/sol-tests/gatekeepers/AllowListGateKeeper.t.sol::50 => assertTrue(gk.isAllowed(member, gateId, abi.encode(new bytes32[](0))));
party-contracts-c4/sol-tests/gatekeepers/AllowListGateKeeper.t.sol::56 => assertFalse(gk.isAllowed(_randomAddress(), gateId, abi.encode(new bytes32[](0))));
party-contracts-c4/sol-tests/gatekeepers/AllowListGateKeeper.t.sol::65 => bytes memory userData = abi.encode(proof);
party-contracts-c4/sol-tests/gatekeepers/AllowListGateKeeper.t.sol::79 => bytes memory userData = abi.encode(proof);
party-contracts-c4/sol-tests/gatekeepers/AllowListGateKeeper.t.sol::91 => bytes memory userData = abi.encode(proof);
party-contracts-c4/sol-tests/gatekeepers/AllowListGateKeeper.t.sol::105 => bytes memory userData = abi.encode(proof);
party-contracts-c4/sol-tests/gatekeepers/AllowListGateKeeper.t.sol::123 => bytes memory userData1 = abi.encode(proof1);
party-contracts-c4/sol-tests/gatekeepers/AllowListGateKeeper.t.sol::128 => bytes memory userData2 = abi.encode(proof2);
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::44 => nextProgressData = completed ? bytes("") : abi.encode(currStep + 1);
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::374 => proposalData: abi.encode(numSteps)
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::545 => _expectProposalExecutedEvent(proposalId, undelegatedVoter, abi.encode(1));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::566 => progressData: abi.encode(1),
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::580 => abi.encode(1),
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1382 => abi.encode(1),
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::2302 => bytes32 expectedHash = keccak256(abi.encode(
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::2316 => bytes32 expectedHash = keccak256(abi.encode(
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::2317 => keccak256(abi.encode(preciousTokens[0], preciousTokens[1])),
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::2318 => keccak256(abi.encode(preciousTokenIds[0], preciousTokenIds[1]))
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::105 => proposalData: abi.encode(calls),
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::138 => ? abi.encode(_randomBytes32()) : bytes('');
party-contracts-c4/sol-tests/proposals/FractionalizeProposalForked.t.sol::73 => proposalData: abi.encode(FractionalizeProposal.FractionalizeProposalData({
party-contracts-c4/sol-tests/proposals/FractionalizeProposalForked.t.sol::97 => bytes memory revertData = abi.encode(address(new EmptyContract()));
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::146 => proposalData: abi.encode(proposalData),
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::247 => params.progressData = abi.encode(ListOnOpenseaProposal.ListOnOpenseaStep.RetrievedFromZora);
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::256 => params.proposalData = abi.encode(data);
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::276 => params.proposalData = abi.encode(data);
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalForked.t.sol::89 => executeParams.proposalData = abi.encode(proposalData);
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalUnit.t.sol::59 => proposalData: abi.encode(data),
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalUnit.t.sol::60 => progressData: abi.encode(step),
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalUnit.t.sol::88 => params.proposalData = abi.encode(data);
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalUnit.t.sol::103 => params.proposalData = abi.encode(data);
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalUnit.t.sol::125 => params.proposalData = abi.encode(data);
party-contracts-c4/sol-tests/proposals/ProposalExecutionEngine.t.sol::113 => executeParams.progressData = abi.encode('poop');
party-contracts-c4/sol-tests/proposals/ProposalExecutionEngine.t.sol::165 => bytes memory initData = abi.encode('yooo');
party-contracts-c4/sol-tests/proposals/ProposalExecutionEngine.t.sol::175 => bytes memory initData = abi.encode('yooo');
party-contracts-c4/sol-tests/proposals/TestableProposalExecutionEngine.sol::59 => return t_nextProgressData = abi.encode(1);
```
## [G010] Empty blocks should be removed or emit something

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. If the block is an empty if-statement block to avoid doing subsequent checks in the else-if/else conditions, the else-if/else conditions should be nested under the negation of the if-statement, because they involve different classes of checks, which may lead to the introduction of errors when the code is later modified (`if(x){}else if(y){...}else{...}` => `if(!x){if(y){...}else{...}}`)
#### Instances: `2` found
```
party-contracts-c4/sol-tests/DummyERC1155.sol::7 => function uri(uint256 id) public override view returns (string memory) {}
party-contracts-c4/sol-tests/proposals/FractionalizeProposalForked.t.sol::36 => contract EmptyContract {}
```
## [G011] Use custom errors rather than revert()/require() strings to save gas

Custom errors are available from solidity version 0.8.4. The instances below match or exceed that version
#### Instances: `3` found
```
party-contracts-c4/deploy/deploy.sol::328 => revert("Could not write ABIs to file");
party-contracts-c4/deploy/deploy.sol::343 => revert("Could not write to file");
party-contracts-c4/sol-tests/utils/ReadOnlyDelegateCall.t.sol::22 => revert("oopsie");
```

## [G012] Using `calldata` instead of `memory` for read-only arguments in `external` functions saves gas
When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. **Each iteration of this for-loop costs at least 60 gas** (i.e. `60 * <mem_array>.length`). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution.
If the array is passed to an `internal` function which passes the array to another internal function where the array is modified and therefore `memory` is used in the external` call, it’s still more gass-efficient to use `calldata` when the `external` function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one
#### Instances: `2` found
File: [PartyFactory.sol::29-30](https://github.com/PartyDAO/party-contracts-c4/blob/d129d647796369a18e30b336e74e7d1bfc779597/contracts/party/PartyFactory.sol#L29-L30)
```
        IERC721[] memory preciousTokens,
        uint256[] memory preciousTokenIds
```
## [G013] Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html\
Use a larger size then downcast where needed
#### Instances: `169` found
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::48 => uint40 duration;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::50 => uint96 maximumBid;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::56 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::94 => uint96 public maximumBid;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::96 => uint96 public lastBid;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::99 => uint40 public expiry;
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::170 => uint96 bidAmount = market.getMinimumBid(auctionId_).safeCastUint256ToUint96();
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::210 => uint96 lastBid_ = lastBid;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::30 => uint40 duration;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::33 => uint96 maximumPrice;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::39 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol::100 => uint96 callValue,
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::27 => uint40 duration;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::30 => uint96 maximumPrice;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::36 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::60 => uint40 public expiry;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::62 => uint96 public maximumPrice;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::64 => uint96 public settledPrice;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::94 => uint96 callValue,
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::114 => uint96 settledPrice_;
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::116 => uint96 maximumPrice_ = maximumPrice;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::31 => uint40 duration;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::34 => uint96 maximumPrice;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::40 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol::116 => uint96 callValue,
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::37 => uint40 voteDuration;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::40 => uint40 executionDelay;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::43 => uint16 passThresholdBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::45 => uint16 feeBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::55 => uint16 splitBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::67 => uint96 previousTotalContributions;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::69 => uint96 amount;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::93 => uint96 public totalContributions;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::104 => uint16 public splitBps;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::380 => uint96 amount,
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::382 => uint96 previousTotalContributions,
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::28 => uint128 memberSupply;
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::30 => uint128 fee;
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::65 => uint16 feeBps
party-contracts-c4/contracts/distribution/ITokenDistributor.sol::85 => uint16 feeBps
party-contracts-c4/contracts/distribution/TokenDistributor.sol::24 => uint128 remainingMemberSupply;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::40 => uint16 feeBps;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::101 => uint16 feeBps
party-contracts-c4/contracts/distribution/TokenDistributor.sol::122 => uint16 feeBps
party-contracts-c4/contracts/distribution/TokenDistributor.sol::166 => uint128 remainingMemberSupply = state.remainingMemberSupply;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::229 => amountsClaimed = new uint128[](infos.length);
party-contracts-c4/contracts/distribution/TokenDistributor.sol::338 => uint128 supply;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::352 => uint128 fee = supply * args.feeBps / 1e4;
party-contracts-c4/contracts/distribution/TokenDistributor.sol::353 => uint128 memberSupply = supply - fee;
party-contracts-c4/contracts/market-wrapper/KoansMarketWrapper.sol::78 => uint8 minBidIncrementPercentage = market.minBidIncrementPercentage();
party-contracts-c4/contracts/party/PartyGovernance.sol::36 => using LibSafeCast for uint96;
party-contracts-c4/contracts/party/PartyGovernance.sol::75 => uint40 voteDuration;
party-contracts-c4/contracts/party/PartyGovernance.sol::78 => uint40 executionDelay;
party-contracts-c4/contracts/party/PartyGovernance.sol::81 => uint16 passThresholdBps;
party-contracts-c4/contracts/party/PartyGovernance.sol::83 => uint96 totalVotingPower;
party-contracts-c4/contracts/party/PartyGovernance.sol::85 => uint16 feeBps;
party-contracts-c4/contracts/party/PartyGovernance.sol::93 => uint40 voteDuration;
party-contracts-c4/contracts/party/PartyGovernance.sol::94 => uint40 executionDelay;
party-contracts-c4/contracts/party/PartyGovernance.sol::95 => uint16 passThresholdBps;
party-contracts-c4/contracts/party/PartyGovernance.sol::96 => uint96 totalVotingPower;
party-contracts-c4/contracts/party/PartyGovernance.sol::102 => uint40 timestamp;
party-contracts-c4/contracts/party/PartyGovernance.sol::104 => uint96 delegatedVotingPower;
party-contracts-c4/contracts/party/PartyGovernance.sol::106 => uint96 intrinsicVotingPower;
party-contracts-c4/contracts/party/PartyGovernance.sol::116 => uint40 maxExecutableTime;
party-contracts-c4/contracts/party/PartyGovernance.sol::130 => uint40 proposedTime;
party-contracts-c4/contracts/party/PartyGovernance.sol::132 => uint40 passedTime;
party-contracts-c4/contracts/party/PartyGovernance.sol::134 => uint40 executedTime;
party-contracts-c4/contracts/party/PartyGovernance.sol::136 => uint40 completedTime;
party-contracts-c4/contracts/party/PartyGovernance.sol::138 => uint96 votes; // -1 == vetoed
party-contracts-c4/contracts/party/PartyGovernance.sol::190 => uint96 constant private VETO_VALUE = uint96(int96(-1));
party-contracts-c4/contracts/party/PartyGovernance.sol::199 => uint16 public feeBps;
party-contracts-c4/contracts/party/PartyGovernance.sol::1070 => uint96 voteCount,
party-contracts-c4/contracts/party/PartyGovernance.sol::1071 => uint96 totalVotingPower,
party-contracts-c4/contracts/party/PartyGovernance.sol::1072 => uint16 passThresholdBps
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::36 => uint40 duration;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::52 => uint40 expiry;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::151 => uint40 zoraTimeout =
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::153 => uint40 zoraDuration =
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::202 => uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MIN_ORDER_DURATION));
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::203 => uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MAX_ORDER_DURATION));
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::34 => uint40 timeout;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::36 => uint40 duration;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::50 => uint40 duration,
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::51 => uint40 timeoutTime
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::94 => uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MIN_AUCTION_DURATION));
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::95 => uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_DURATION));
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::102 => uint40 maxTimeout = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_TIMEOUT));
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::133 => uint40 timeout,
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::135 => uint40 duration,
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::166 => uint40 minExpiry,
party-contracts-c4/contracts/renderers/PartyGovernanceNFTRenderer.sol::22 => uint16 feeBps;
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol::25 => uint8 curatorFeePercentage;
party-contracts-c4/contracts/vendor/markets/IZoraAuctionHouse.sol::44 => uint8 curatorFeePercentages,
party-contracts-c4/sol-tests/TestUsers.sol::56 => uint16 passThresholdBps;
party-contracts-c4/sol-tests/TestUsers.sol::57 => uint96 totalVotingPower;
party-contracts-c4/sol-tests/TestUsers.sol::60 => uint16 feeBps;
party-contracts-c4/sol-tests/crowdfund/AuctionCrowdfund.t.sol::53 => uint40 defaultDuration = 60 * 60;
party-contracts-c4/sol-tests/crowdfund/AuctionCrowdfund.t.sol::54 => uint96 defaultMaxBid = 10e18;
party-contracts-c4/sol-tests/crowdfund/AuctionCrowdfund.t.sol::56 => uint16 defaultSplitBps = 0.1e4;
party-contracts-c4/sol-tests/crowdfund/AuctionCrowdfund.t.sol::79 => uint96 initialContribution
party-contracts-c4/sol-tests/crowdfund/BuyCrowdfund.t.sol::37 => uint40 defaultDuration = 60 * 60;
party-contracts-c4/sol-tests/crowdfund/BuyCrowdfund.t.sol::38 => uint96 defaultMaxPrice = 10e18;
party-contracts-c4/sol-tests/crowdfund/BuyCrowdfund.t.sol::40 => uint16 defaultSplitBps = 0.1e4;
party-contracts-c4/sol-tests/crowdfund/BuyCrowdfund.t.sol::63 => uint96 initialContribution
party-contracts-c4/sol-tests/crowdfund/CollectionBuyCrowdfund.t.sol::38 => uint40 defaultDuration = 60 * 60;
party-contracts-c4/sol-tests/crowdfund/CollectionBuyCrowdfund.t.sol::39 => uint96 defaultMaxPrice = 10e18;
party-contracts-c4/sol-tests/crowdfund/CollectionBuyCrowdfund.t.sol::41 => uint16 defaultSplitBps = 0.1e4;
party-contracts-c4/sol-tests/crowdfund/Crowdfund.t.sol::42 => uint40 defaultDuration = 60 * 60;
party-contracts-c4/sol-tests/crowdfund/Crowdfund.t.sol::43 => uint96 defaultMaxBid = 10e18;
party-contracts-c4/sol-tests/crowdfund/Crowdfund.t.sol::45 => uint16 defaultSplitBps = 0.1e4;
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::78 => uint96 randomUint96,
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::79 => uint40 randomUint40,
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::80 => uint16 randomBps
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::249 => uint96 randomUint96,
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::250 => uint40 randomUint40,
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::251 => uint16 randomBps
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::315 => uint96 randomUint96,
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::316 => uint40 randomUint40,
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::317 => uint16 randomBps
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::379 => uint16 splitBps,
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::380 => uint16 passThresholdBps,
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::381 => uint16 feeBps
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::415 => uint16 invalidBps;
party-contracts-c4/sol-tests/distribution/TokenDistributor.t.sol::32 => for (uint160 i; i < 10; i++) {
party-contracts-c4/sol-tests/distribution/TokenDistributor.t.sol::339 => uint16 feeSplitBps,
party-contracts-c4/sol-tests/distribution/TokenDistributorUnit.t.sol::78 => uint16 constant DEFAULT_FEE_BPS = 0.02e4;
party-contracts-c4/sol-tests/party/PartyFactory.t.sol::76 => uint96 randomUint96,
party-contracts-c4/sol-tests/party/PartyFactory.t.sol::77 => uint40 randomUint40,
party-contracts-c4/sol-tests/party/PartyFactory.t.sol::78 => uint16 randomBps
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::80 => uint40 firstTime = uint40(block.timestamp);
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::85 => uint40 nextTime = firstTime + 10;
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::182 => uint40 firstTime = uint40(block.timestamp);
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::189 => uint40 nextTime = firstTime + 10;
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::528 => uint96 expectedNumVotes
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::66 => uint16 feeBps,
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::79 => uint16 feeBps
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::104 => uint16 feeBps
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::308 => uint16 feeBps,
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::349 => uint96 totalVotingPower,
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::47 => uint40 expiry,
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::48 => uint40 timeoutTime
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::123 => uint40 duration,
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::233 => uint40 listDuration = 7 days;
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::250 => uint40 minDuration = uint40(globals.getUint256(LibGlobals.GLOBAL_OS_MIN_ORDER_DURATION));
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::265 => uint40(block.timestamp) + minDuration
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::270 => uint40 maxDuration = uint40(globals.getUint256(LibGlobals.GLOBAL_OS_MAX_ORDER_DURATION));
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::285 => uint40(block.timestamp) + maxDuration
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::295 => uint40 listDuration = 7 days;
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::352 => uint40 listDuration = 7 days;
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::416 => uint40 listDuration = 7 days;
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::469 => uint40 listDuration = 7 days;
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::522 => uint40 listDuration = 7 days;
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::583 => uint40 listDuration = 7 days;
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::651 => uint40 listDuration = 7 days;
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalForked.t.sol::24 => uint40 duration,
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalForked.t.sol::25 => uint40 timeoutTime
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalForked.t.sol::40 => uint8 curatorFeePercentage,
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalUnit.t.sol::26 => uint40 duration,
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalUnit.t.sol::27 => uint40 timeoutTime
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalUnit.t.sol::38 => uint40 timeout,
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalUnit.t.sol::39 => uint40 duration,
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalUnit.t.sol::86 => uint40 minDuration = uint40(globals.getUint256(LibGlobals.GLOBAL_ZORA_MIN_AUCTION_DURATION));
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalUnit.t.sol::101 => uint40 maxDuration = uint40(globals.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_DURATION));
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalUnit.t.sol::123 => uint40 maxTimeout = uint40(globals.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_TIMEOUT));
party-contracts-c4/sol-tests/proposals/MockZoraAuctionHouse.sol::20 => uint8,
party-contracts-c4/sol-tests/utils/CrowdfundHelpers.t.sol::22 => uint40 defaultDuration = 60 * 60;
party-contracts-c4/sol-tests/utils/CrowdfundHelpers.t.sol::23 => uint96 defaultMaxBid = 10e18;
party-contracts-c4/sol-tests/utils/CrowdfundHelpers.t.sol::24 => uint96 defaultMaxPrice = 10e18;
party-contracts-c4/sol-tests/utils/CrowdfundHelpers.t.sol::26 => uint16 defaultSplitBps = 0.1e4;
party-contracts-c4/sol-tests/utils/CrowdfundHelpers.t.sol::59 => uint96 initialContribution
```
## [G014] Use assembly to check for address(0)

Use assembly to check for address(0)
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
#### Instances: `185` found
```
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol::270 => return address(party) != address(0)
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol::153 => return address(party) != address(0)
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::367 => if (splitRecipient_ == address(0)) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::388 => if (delegate == address(0)) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::392 => if (gateKeeper != IGateKeeper(address(0))) {
party-contracts-c4/contracts/crowdfund/Crowdfund.sol::474 => if (delegate == address(0)) {
party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol::116 => address(gateKeeper) == address(0) ||
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::89 => return address(0);
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::127 => if (owner == address(0)) {
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::138 => return _owners[uint256(uint160(owner))] != address(0);
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::146 => emit Transfer(address(0), owner, tokenId);
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::153 => _owners[tokenId] = address(0);
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol::154 => emit Transfer(owner, address(0), tokenId);
party-contracts-c4/contracts/party/PartyFactory.sol::36 => if (authority == address(0)) {
party-contracts-c4/contracts/party/PartyGovernance.sol::460 => if (newPartyHost != address(0)) {
party-contracts-c4/contracts/party/PartyGovernance.sol::885 => _adjustVotingPower(from, -powerI192, address(0));
party-contracts-c4/contracts/party/PartyGovernance.sol::886 => _adjustVotingPower(to, powerI192, address(0));
party-contracts-c4/contracts/party/PartyGovernance.sol::898 => oldDelegate = oldDelegate == address(0) ? voter : oldDelegate;
party-contracts-c4/contracts/party/PartyGovernance.sol::900 => delegate = delegate == address(0) ? oldDelegate : delegate;
party-contracts-c4/contracts/party/PartyGovernance.sol::931 => if (newDelegate == address(0) || oldDelegate == address(0)) {
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol::107 => return (address(0), 0); // Just to make the compiler happy.
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol::176 => if (op != address(0)) {
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol::69 => vault.updateCurator(address(0));
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::266 => orderParams.orderType = orderParams.zone == address(0)
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::287 => cons.token = address(0);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::294 => cons.token = address(0);
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol::367 => token.approve(address(0), tokenId);
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::149 => payable(address(0)),
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol::151 => IERC20(address(0)) // Indicates ETH sale
party-contracts-c4/sol-tests/DummyERC721.sol::23 => require(owner != address(0), 'DummyERC721/INVALID_TOKEN');
party-contracts-c4/sol-tests/DummyERC721.sol::32 => require(owner != address(0), 'INVALID_TOKEN');
party-contracts-c4/sol-tests/DummyERC721.sol::53 => emit Transfer(address(0), owner, id);
party-contracts-c4/sol-tests/DummyERC721.sol::100 => require(to != address(0), 'DummyERC721/BAD_TRANSFER');
party-contracts-c4/sol-tests/DummyERC721.sol::106 => getApproved[tokenId] = address(0);
party-contracts-c4/sol-tests/crowdfund/AuctionCrowdfund.t.sol::191 => assertEq(address(party_), address(0));
party-contracts-c4/sol-tests/crowdfund/AuctionCrowdfund.t.sol::216 => assertEq(address(party_), address(0));
party-contracts-c4/sol-tests/crowdfund/AuctionCrowdfund.t.sol::242 => assertEq(address(party_), address(0));
party-contracts-c4/sol-tests/crowdfund/Crowdfund.t.sol::204 => assertEq(cf.delegationsByContributor(initialContributor), address(0));
party-contracts-c4/sol-tests/crowdfund/Crowdfund.t.sol::482 => assertEq(address(cf.party()), address(0));
party-contracts-c4/sol-tests/crowdfund/Crowdfund.t.sol::568 => assertEq(address(cf.party()), address(0));
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::72 => return (IGateKeeper(address(0)), bytes12(_randomBytes32()), createGateCallData);
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::141 => address(opts.gateKeeper) == address(0) ? gateKeeperId :  bytes12(uint96(1))
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::160 => splitRecipient: payable(address(0)),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::162 => initialContributor: address(0),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::163 => initialDelegate: address(0),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::164 => gateKeeper: IGateKeeper(address(0)),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::172 => feeRecipient: payable(address(0))
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::189 => nftContract: IERC721(address(0)),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::193 => splitRecipient: payable(address(0)),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::195 => initialContributor: address(0),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::196 => initialDelegate: address(0),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::197 => gateKeeper: IGateKeeper(address(0)),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::205 => feeRecipient: payable(address(0))
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::227 => splitRecipient: payable(address(0)),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::229 => initialContributor: address(0),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::230 => initialDelegate: address(0),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::231 => gateKeeper: IGateKeeper(address(0)),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::239 => feeRecipient: payable(address(0))
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::308 => address(opts.gateKeeper) == address(0) ? gateKeeperId :  bytes12(uint96(1))
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::373 => address(opts.gateKeeper) == address(0) ? gateKeeperId :  bytes12(uint96(1))
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::399 => splitRecipient: payable(address(0)),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::401 => initialContributor: address(0),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::402 => initialDelegate: address(0),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::403 => gateKeeper: IGateKeeper(address(0)),
party-contracts-c4/sol-tests/crowdfund/CrowdfundFactory.t.sol::411 => feeRecipient: payable(address(0))
party-contracts-c4/sol-tests/crowdfund/FoundationForked.t.sol::68 => splitRecipient: payable(address(0)),
party-contracts-c4/sol-tests/crowdfund/FoundationForked.t.sol::71 => initialDelegate: address(0),
party-contracts-c4/sol-tests/crowdfund/FoundationForked.t.sol::72 => gateKeeper: IGateKeeper(address(0)),
party-contracts-c4/sol-tests/crowdfund/FoundationForked.t.sol::152 => assertEq(address(pb.party()), address(0));
party-contracts-c4/sol-tests/crowdfund/FoundationForked.t.sol::176 => assertEq(address(pb.party()), address(0));
party-contracts-c4/sol-tests/crowdfund/MockMarketWrapper.sol::101 => if (lastBidder != address(0)) {
party-contracts-c4/sol-tests/crowdfund/MockMarketWrapper.sol::138 => if (auc.winner == address(0)) {
party-contracts-c4/sol-tests/crowdfund/MockMarketWrapper.sol::175 => if (auc.winner != address(0)) {
party-contracts-c4/sol-tests/crowdfund/MockMarketWrapper.sol::186 => if (callbackTarget != address(0)) {
party-contracts-c4/sol-tests/crowdfund/NounsForked.t.sol::62 => splitRecipient: payable(address(0)),
party-contracts-c4/sol-tests/crowdfund/NounsForked.t.sol::65 => initialDelegate: address(0),
party-contracts-c4/sol-tests/crowdfund/NounsForked.t.sol::66 => gateKeeper: IGateKeeper(address(0)),
party-contracts-c4/sol-tests/crowdfund/NounsForked.t.sol::144 => assertEq(address(pb.party()), address(0));
party-contracts-c4/sol-tests/crowdfund/NounsForked.t.sol::166 => assertEq(address(pb.party()), address(0));
party-contracts-c4/sol-tests/crowdfund/ZoraForked.t.sol::55 => payable(address(0)),
party-contracts-c4/sol-tests/crowdfund/ZoraForked.t.sol::57 => IERC20(address(0)) // Indicates ETH sale
party-contracts-c4/sol-tests/crowdfund/ZoraForked.t.sol::77 => splitRecipient: payable(address(0)),
party-contracts-c4/sol-tests/crowdfund/ZoraForked.t.sol::80 => initialDelegate: address(0),
party-contracts-c4/sol-tests/crowdfund/ZoraForked.t.sol::81 => gateKeeper: IGateKeeper(address(0)),
party-contracts-c4/sol-tests/crowdfund/ZoraForked.t.sol::169 => assertEq(address(pb.party()), address(0));
party-contracts-c4/sol-tests/crowdfund/ZoraForked.t.sol::200 => assertEq(address(pb.party()), address(0));
party-contracts-c4/sol-tests/distribution/DummyTokenDistributorParty.sol::21 => if (foundOwner == address(0)) {
party-contracts-c4/sol-tests/distribution/TokenDistributorUnit.t.sol::44 => contract TestTokenDistributorHash is TokenDistributor(IGlobals(address(0))) {
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::55 => host2: address(0),
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::128 => progressData: abi.encodePacked([address(0)])
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::143 => host2: address(0),
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::234 => progressData: abi.encodePacked([address(0)])
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::249 => host2: address(0),
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::297 => progressData: abi.encodePacked([address(0)])
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::307 => host2: address(0),
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::393 => progressData: abi.encodePacked([address(0)])
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::458 => progressData: abi.encodePacked([address(0)])
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::466 => host2: address(0),
party-contracts-c4/sol-tests/party/PartyGovernance.t.sol::490 => host2: address(0),
party-contracts-c4/sol-tests/party/PartyGovernanceNFT.t.sol::55 => host2: address(0),
party-contracts-c4/sol-tests/party/PartyGovernanceNFT.t.sol::72 => host2: address(0),
party-contracts-c4/sol-tests/party/PartyGovernanceNFT.t.sol::95 => host2: address(0),
party-contracts-c4/sol-tests/party/PartyGovernanceNFT.t.sol::107 => assertEq(party.mintAuthority(), address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceNFT.t.sol::114 => host2: address(0),
party-contracts-c4/sol-tests/party/PartyGovernanceNFT.t.sol::138 => host2: address(0),
party-contracts-c4/sol-tests/party/PartyGovernanceNFT.t.sol::163 => host2: address(0),
party-contracts-c4/sol-tests/party/PartyGovernanceNFT.t.sol::184 => host2: address(0),
party-contracts-c4/sol-tests/party/PartyGovernanceNFT.t.sol::207 => host2: address(0),
party-contracts-c4/sol-tests/party/PartyGovernanceNFT.t.sol::218 => assertEq(receiver, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::452 => gov.rawAdjustVotingPower(undelegatedVoter, 51e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::513 => gov.rawAdjustVotingPower(undelegatedVoter, 51e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::596 => gov.rawAdjustVotingPower(undelegatedVoter, 100e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::652 => gov.rawAdjustVotingPower(undelegatedVoter1, 75e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::654 => gov.rawAdjustVotingPower(undelegatedVoter2, 25e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::713 => gov.rawAdjustVotingPower(undelegatedVoter, int192(uint192(vp)), address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::766 => gov.rawAdjustVotingPower(undelegatedVoter, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::817 => gov.rawAdjustVotingPower(undelegatedVoter, 51e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::853 => gov.rawAdjustVotingPower(undelegatedVoter, 51e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::893 => gov.rawAdjustVotingPower(undelegatedVoter, 51e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::945 => gov.rawAdjustVotingPower(undelegatedVoter, 51e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::986 => gov.rawAdjustVotingPower(voter, 100e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1027 => gov.rawAdjustVotingPower(voter, 100e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1068 => gov.rawAdjustVotingPower(voter, 100e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1116 => gov.rawAdjustVotingPower(undelegatedVoter, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1143 => gov.rawAdjustVotingPower(undelegatedVoter, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1163 => gov.rawAdjustVotingPower(undelegatedVoter, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1210 => gov.rawAdjustVotingPower(undelegatedVoter, 51e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1258 => gov.rawAdjustVotingPower(undelegatedVoter, 51e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1301 => gov.rawAdjustVotingPower(undelegatedVoter, 51e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1344 => gov.rawAdjustVotingPower(undelegatedVoter, 51e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1398 => gov.rawAdjustVotingPower(undelegatedVoter, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1441 => gov.rawAdjustVotingPower(delegate, 25e18, address(0)); // self-delegated
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1445 => gov.rawAdjustVotingPower(undelegatedVoter, 25e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1487 => gov.rawAdjustVotingPower(delegate, 30e18, address(0)); // self-delegated
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1491 => gov.rawAdjustVotingPower(undelegatedVoter, 10e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1526 => gov.rawAdjustVotingPower(undelegatedVoter1, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1528 => gov.rawAdjustVotingPower(undelegatedVoter2, 1e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1562 => gov.rawAdjustVotingPower(undelegatedVoter, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1646 => gov.rawAdjustVotingPower(undelegatedVoter1, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1648 => gov.rawAdjustVotingPower(undelegatedVoter2, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1692 => gov.rawAdjustVotingPower(delegate, 30e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1696 => gov.rawAdjustVotingPower(undelegatedVoter, 1e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1802 => gov.rawAdjustVotingPower(undelegatedVoter, 51e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1809 => gov.rawAdjustVotingPower(undelegatedVoter, -51e18 - 1, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1821 => gov.rawAdjustVotingPower(voter, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1834 => gov.rawAdjustVotingPower(voter, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1837 => gov.rawAdjustVotingPower(voter, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1849 => gov.rawAdjustVotingPower(voter, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1867 => gov.rawAdjustVotingPower(voter, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1898 => gov.rawAdjustVotingPower(voter, 1, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1901 => gov.rawAdjustVotingPower(voter, 1, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1904 => gov.rawAdjustVotingPower(voter, 1, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1932 => gov.rawAdjustVotingPower(delegate2, 20e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::1992 => gov.rawAdjustVotingPower(voter1, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::2003 => gov.delegateVotingPower(address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::2069 => gov.rawAdjustVotingPower(pastMember, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::2096 => gov.rawAdjustVotingPower(pastMember, 50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::2121 => gov.rawAdjustVotingPower(voter1, -50e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::2124 => gov.rawAdjustVotingPower(voter1, 75e18, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::2125 => gov.rawAdjustVotingPower(voter2, 1, address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::2169 => gov.rawAdjustVotingPower(voter1, int192(int256(votesNeededToPass / 2)), address(0));
party-contracts-c4/sol-tests/party/PartyGovernanceUnit.t.sol::2171 => gov.rawAdjustVotingPower(voter2, int192(int256(votesNeededToPass / 2 + 1)), address(0));
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::447 => (address(0), preciousTokenId)
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::452 => assertEq(preciousToken.getApproved(preciousTokenId), address(0));
party-contracts-c4/sol-tests/proposals/ArbitraryCallsProposal.t.sol::531 => (address(0), _randomUint256())
party-contracts-c4/sol-tests/proposals/FractionalizeProposalForked.t.sol::82 => assertEq(expectedVault.curator(), address(0));
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::164 => orderParams.orderType = orderParams.zone == address(0)
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::185 => cons.token = address(0);
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::192 => cons.token = address(0);
party-contracts-c4/sol-tests/proposals/ListOnOpenseaProposalForked.t.sol::576 => assertEq(token.getApproved(tokenId), address(0));
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalForked.t.sol::128 => address(0),
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalForked.t.sol::130 => address(0)
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalIntegration.t.sol::52 => IOpenseaExchange(address(0)),
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalIntegration.t.sol::53 => IOpenseaConduitController(address(0)),
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalIntegration.t.sol::55 => IFractionalV1VaultFactory(address(0))
party-contracts-c4/sol-tests/proposals/ListOnZoraProposalIntegration.t.sol::79 => host2: address(0),
party-contracts-c4/sol-tests/proposals/OpenseaTestUtils.sol::76 => order.parameters.orderType = params.zone == address(0)
party-contracts-c4/sol-tests/proposals/OpenseaTestUtils.sol::90 => considerations[0].token = address(0);
party-contracts-c4/sol-tests/proposals/OpenseaTestUtils.sol::96 => considerations[1 + i].token = address(0);
party-contracts-c4/sol-tests/utils/LibSafeERC721.t.sol::28 => assertEq(token.safeOwnerOf(999), address(0));
party-contracts-c4/sol-tests/utils/LibSafeERC721.t.sol::32 => assertEq(eoa.safeOwnerOf(tokenId), address(0));
party-contracts-c4/sol-tests/utils/LibSafeERC721.t.sol::36 => assertEq(emptyContract.safeOwnerOf(tokenId), address(0));
party-contracts-c4/sol-tests/utils/LibSafeERC721.t.sol::40 => assertEq(badERC721.safeOwnerOf(tokenId), address(0));
party-contracts-c4/sol-tests/utils/PartyGovernanceHelpers.t.sol::67 => host2: address(0),
party-contracts-c4/sol-tests/utils/PartyGovernanceHelpers.t.sol::109 => host2: address(0),
party-contracts-c4/sol-tests/utils/PartyGovernanceHelpers.t.sol::156 => host2: address(0),
```