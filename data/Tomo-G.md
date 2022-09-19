# Gas Optimization

## ✅ G-1: Don't Initialize Variables with Default Value

### 📝 Description

Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it's default value costs unnecesary gas. Not overwriting the default for [stack variables](https://gist.github.com/IllIllI000/e075d189c1b23dce256cd166e28f3397) saves 8 gas. Storage and memory variables have larger savings

### 💡 Recommendation

Delete useless variable declarations to save gas.

### 🔍 Findings:

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180` [for (uint256 i = 0; i < contributors.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L242` [for (uint256 i = 0; i < numContributions; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L242)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300` [for (uint256 i = 0; i < preciousTokens.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L348` [for (uint256 i = 0; i < numContributions; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L348)

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230` [for (uint256 i = 0; i < infos.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230)

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239` [for (uint256 i = 0; i < infos.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L306` [for (uint256 i=0; i < opts.hosts.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L306)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L432` [uint256 low = 0;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L432)

`party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52` [for (uint256 i = 0; i < hadPreciouses.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52)

`party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61` [for (uint256 i = 0; i < calls.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61)

`party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78` [for (uint256 i = 0; i < hadPreciouses.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78)

`party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L14` [for (uint256 i = 0; i < preciousTokens.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L14)

`party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L32` [for (uint256 i = 0; i < preciousTokens.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L32)

`party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291` [for (uint256 i = 0; i < fees.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291)

## ✅ G-2: Cache Array Length Outside of Loop

### 📝 Description

Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.
You save 3 gas by not reading `array.length` - 3 gas per instance - 27 gas saved

### 💡 Recommendation

### 🔍 Findings:

`party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62` [for (uint256 i; i < hosts.length; i++) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180` [for (uint256 i = 0; i < contributors.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300` [for (uint256 i = 0; i < preciousTokens.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300)

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230` [for (uint256 i = 0; i < infos.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230)

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239` [for (uint256 i = 0; i < infos.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L306` [for (uint256 i=0; i < opts.hosts.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L306)

`party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52` [for (uint256 i = 0; i < hadPreciouses.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52)

`party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61` [for (uint256 i = 0; i < calls.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61)

`party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78` [for (uint256 i = 0; i < hadPreciouses.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78)

`party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L14` [for (uint256 i = 0; i < preciousTokens.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L14)

`party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L32` [for (uint256 i = 0; i < preciousTokens.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L32)

`party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291` [for (uint256 i = 0; i < fees.length; ++i) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291)

## ✅ G-3: Use != 0 instead of > 0 for Unsigned Integer Comparison

### 📝 Description

Use != 0 when comparing uint variables to zero, which cannot hold values below zero

### 💡 Recommendation

You should change from `> 0` to `!=0`.

### 🔍 Findings:

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L144` [if (initialBalance > 0) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L144)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L471` [if (votingPower > 0) {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L471)

## ✅ G-4: Use Shift Right/Left instead of Division/Multiplication if possible

### 📝 Description

A division/multiplication by any number `x` being a power of 2 can be calculated by shifting `log2(x)` to the right/left.
While the `DIV` opcode uses 5 gas, the `SHR` opcode only uses 3 gas.
urthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

### 💡 Recommendation

You should change multiplication and division by powers of two to bit shift.

### 🔍 Findings:

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L434` [uint256 mid = (low + high) / 2;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L434)

## ✅ G-5: Use assembly balance ETH

### 📝 Description

You can save 159 gas per instance if using assembly to check for balance of address

### 💡 Recommendation

Please follow [this link](https://gist.github.com/Tomosuke0930/4547e012ece4575e88e2a6d2baf498fa) to make corrections

### 🔍 Findings:

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L143` [uint96 initialBalance = address(this).balance.safeCastUint256ToUint96();](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L143)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L199` [// We cannot use `address(this).balance - msg.value` as the previous](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L199)

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L111` [currentTokenBalance: address(this).balance,](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L111)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L501` [{ value: address(this).balance }(this, feeRecipient, feeBps);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L501)

## ✅ G-6: Use assembly to hash

### 📝 Description

See [this issue](https://github.com/PartyDAO/2022-06-putty-findings/issues/59)
You can save about 5000 gas per instance in deploying contracrt.
You can save about 80 gas per instance if using assembly to execute the function.

### 💡 Recommendation

Please follow [this link](https://gist.github.com/Tomosuke0930/72beb469f08c954b232e58b8da03ef28) to make corrections

### 🔍 Findings:

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L331` [mstore(opts, keccak256(add(mload(opts), 0x20), mul(mload(mload(opts)), 32)))](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L331)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L333` [h := keccak256(opts, 0xC0)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L333)

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L397` [keccak256(info, 0xe0),](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L397)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L405` [bytes32 dataHash = keccak256(proposal.proposalData);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L405)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L412` [proposalHash := keccak256(proposal, 0x60)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L412)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1114` [mstore(0x00, keccak256(](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1114)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1118` [mstore(0x20, keccak256(](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1118)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1122` [h := keccak256(0x00, 0x40)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1122)

`party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L121` [bytes32 resultHash = keccak256(r);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L121)

`party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L184` [bytes32 errHash = keccak256(errData);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L184)

`party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L80` [uint256 private constant \_STORAGE_SLOT = uint256(keccak256('ProposalExecutionEngine.Storage'));](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L80)

`party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L145` [keccak256(params.progressData),](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L145)

`party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L150` [bytes32 progressDataHash = keccak256(params.progressData);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L150)

`party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L178` [stor.nextProgressDataHash = keccak256(nextProgressData);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L178)

`party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L19` [uint256 private constant SHARED_STORAGE_SLOT = uint256(keccak256("ProposalStorage.SharedProposalStorage"));](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L19)

## ✅ G-7: Empty blocks should be removed or emit something

### 📝 Description

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting.

### 💡 Recommendation

Empty blocks should be removed or emit something (The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting.

### 🔍 Findings:

`party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L144` [receive() external payable {}](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L144)

`party-contracts-c4/blob/main/contracts/party/Party.sol#L47` [receive() external payable {}](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L47)

## ✅ G-8: Functions guaranteed to revert when called by normal users can be marked payable

### 📝 Description

See [this issue](https://github.com/PartyDAO/2022-06-putty-findings/issues/59)
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost
Saves 2400 gas per instance in deploying contracrt.
Saves about 20 gas per instance if using assembly to executing the function.

### 💡 Recommendation

You should add the keyword `payable` to those functions.

### 🔍 Findings:

`party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L113` [onlyConstructor](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L113)

`party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L149` [function bid() external onlyDelegateCall {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L149)

`party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L198` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L198)

`party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L71` [onlyConstructor](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L71)

`party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L99` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L99)

`party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L86` [onlyConstructor](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L86)

`party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L121` [onlyHost(governanceOpts.hosts)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L121)

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L305` [onlyPartyDao](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L305)

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L306` [onlyIfEmergencyActionsAllowed](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L306)

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L319` [onlyPartyDao](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L319)

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L320` [onlyIfEmergencyActionsAllowed](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L320)

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L327` [function disableEmergencyActions() onlyPartyDao external {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L327)

`party-contracts-c4/blob/main/contracts/globals/Globals.sol#L27` [function transferMultiSig(address newMultiSig) external onlyMultisig {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L27)

`party-contracts-c4/blob/main/contracts/globals/Globals.sol#L59` [function setBytes32(uint256 key, bytes32 value) external onlyMultisig {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L59)

`party-contracts-c4/blob/main/contracts/globals/Globals.sol#L63` [function setUint256(uint256 key, uint256 value) external onlyMultisig {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L63)

`party-contracts-c4/blob/main/contracts/globals/Globals.sol#L67` [function setAddress(uint256 key, address value) external onlyMultisig {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L67)

`party-contracts-c4/blob/main/contracts/globals/Globals.sol#L71` [function setIncludesBytes32(uint256 key, bytes32 value, bool isIncluded) external onlyMultisig {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L71)

`party-contracts-c4/blob/main/contracts/globals/Globals.sol#L75` [function setIncludesUint256(uint256 key, uint256 value, bool isIncluded) external onlyMultisig {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L75)

`party-contracts-c4/blob/main/contracts/globals/Globals.sol#L79` [function setIncludesAddress(uint256 key, address value, bool isIncluded) external onlyMultisig {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L79)

`party-contracts-c4/blob/main/contracts/party/Party.sol#L35` [onlyConstructor](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L35)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L451` [function delegateVotingPower(address delegate) external onlyDelegateCall {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L451)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L458` [function abdicate(address newPartyHost) external onlyHost onlyDelegateCall {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L458)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L489` [onlyActiveMember](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L489)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L490` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L490)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L528` [onlyActiveMember](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L528)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L529` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L529)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L564` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L564)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L616` [function veto(uint256 proposalId) external onlyHost onlyDelegateCall {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L616)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L663` [onlyActiveMember](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L663)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L664` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L664)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L724` [onlyActiveMember](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L724)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L725` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L725)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L790` [onlyPartyDao](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L790)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L791` [onlyWhenEmergencyExecuteAllowed](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L791)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L792` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L792)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L800` [function disableEmergencyExecute() external onlyPartyDaoOrHost onlyDelegateCall {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L800)

`party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L126` [onlyMinter](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L126)

`party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L127` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L127)

`party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L139` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L139)

`party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L150` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L150)

`party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L161` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L161)

`party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L169` [function abdicate() external onlyMinter onlyDelegateCall {](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L169)

`party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L102` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L102)

`party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L118` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L118)

`party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L185` [onlyDelegateCall](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L185)

## ✅ G-9: Use x=x+y instad of x+=y

### 📝 Description

You can save about 35 gas per instance if you change from `x+=y**`\*\* to `x=x+y`
This works equally well for subtraction, multiplication and division.

### 💡 Recommendation

You should change from `x+=y` to `x=x+y`.

### 🔍 Findings:

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L243` [ethContributed += contributions[i].amount;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L243)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L352` [ethOwed += c.amount;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L352)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L355` [ethUsed += c.amount;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L355)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L359` [ethUsed += partialEthUsed;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L359)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L374` [votingPower += (splitBps\_ \* totalEthUsed + (1e4 - 1)) / 1e4; // round up](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L374)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L411` [totalContributions += amount;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L411)

`party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L427` [lastContribution.amount += amount;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L427)

`party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L381` [\_storedBalances[balanceId] -= amount;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L381)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L595` [values.votes += votingPower;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L595)

`party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L959` [newDelegateDelegatedVotingPower -= oldSnap.intrinsicVotingPower;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L959)

`party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L72` [ethAvailable -= calls[i].value;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L72)
