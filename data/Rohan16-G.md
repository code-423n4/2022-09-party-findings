# G1. Using `calldata` instead of `memory` for read-only arguments in `external` functions saves gas
### Description
When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. **Each iteration of this for-loop costs at least 60 gas** (i.e. `60 * <mem_array>.length`). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution.

If the array is passed to an `internal` function which passes the array to another internal function where the array is modified and therefore `memory` is used in the `external` call, it’s still more gass-efficient to use `calldata` when the `external` function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one
 ### Findings
 // Links to github files
[PartyFactory.sol](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyFactory.sol#L30)

```
contracts/party/PartyFactory.sol:29:        IERC721[] memory preciousTokens,
contracts/party/PartyFactory.sol:30:        uint256[] memory preciousTokenIds
```

[PartyGovernance.sol](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L657)
```
contracts/party/PartyGovernance.sol:656:        IERC721[] memory preciousTokens,
contracts/party/PartyGovernance.sol:657:        uint256[] memory preciousTokenIds,
```
[IPartyFactory.sol](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/IPartyFactory.sol#L29)
```
contracts/party/IPartyFactory.sol:28:        IERC721[] memory preciousTokens,
contracts/party/IPartyFactory.sol:29:        uint256[] memory preciousTokenIds
```
### Recommended Mitigation Steps
use `calldata` instead of `memory`Use a more recent version of solidity

----
# G2. ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas [PER LOOP](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)
### Findings
 // Links to github files
[CollectionBuyCrowdfund.sol:L62](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62)
```
contracts/crowdfund/CollectionBuyCrowdfund.sol:62:        for (uint256 i; i < hosts.length; i++) {
```
----
# G3. Inline a modifier that’s only used once
### Description
As notDelegated() is only used once in this contract (in function proxiableUUID()), it should get inlined to save gas:
### Finidings
 // Links to github files
[PartyGovernance.sol:L258](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L258)

``` 
contracts/party/PartyGovernance.sol:258:    modifier onlyWhenEmergencyExecuteAllowed() {
contracts/party/PartyGovernance.sol:791:        onlyWhenEmergencyExecuteAllowed

```
[CollectionBuyCrowdfund.sol:L60](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L60)
```
contracts/crowdfund/CollectionBuyCrowdfund.sol:60:    modifier onlyHost(address[] memory hosts) {
contracts/crowdfund/CollectionBuyCrowdfund.sol:121:        onlyHost(governanceOpts.hosts)
```

----
# G4.`<ARRAY>.LENGTH` SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP

The overheads outlined below are `PER LOOP`, excluding the first loop
storage arrays incur a Gwarmaccess (100 gas) memory arrays use `MLOAD` (3 gas)
calldata arrays use `CALLDATALOAD` (3 gas) Caching the length changes each of these to a `DUP<N>` (3 gas), and gets rid of the extra `DUP<N>` needed to store the stack offset.

 ### Findings
```
contracts/proposals/ArbitraryCallsProposal.sol:52:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
contracts/proposals/ArbitraryCallsProposal.sol:61:        for (uint256 i = 0; i < calls.length; ++i) {
contracts/proposals/ArbitraryCallsProposal.sol:78:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
contracts/proposals/LibProposal.sol:14:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
contracts/proposals/LibProposal.sol:32:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
contracts/proposals/ListOnOpenseaProposal.sol:291:            for (uint256 i = 0; i < fees.length; ++i) {
contracts/party/PartyGovernance.sol:306:        for (uint256 i=0; i < opts.hosts.length; ++i) {
contracts/crowdfund/Crowdfund.sol:180:        for (uint256 i = 0; i < contributors.length; ++i) {
contracts/crowdfund/Crowdfund.sol:300:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
contracts/crowdfund/CollectionBuyCrowdfund.sol:62:        for (uint256 i; i < hosts.length; i++) {
contracts/distribution/TokenDistributor.sol:230:        for (uint256 i = 0; i < infos.length; ++i) {
contracts/distribution/TokenDistributor.sol:239:        for (uint256 i = 0; i < infos.length; ++i) {
```
----
# G5. IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED
If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for `(uint256 i = 0; i < numIterations; ++i)` { should be replaced with for `(uint256 i; i < numIterations; ++i) {`
 ### Findings
 [ArbitraryCallsProposal.sol:L52](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52)
```
contracts/proposals/ArbitraryCallsProposal.sol:52:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
contracts/proposals/ArbitraryCallsProposal.sol:61:        for (uint256 i = 0; i < calls.length; ++i) {
contracts/proposals/ArbitraryCallsProposal.sol:78:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```
[LibProposal.sol:L14](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14)
```
contracts/proposals/LibProposal.sol:14:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
contracts/proposals/LibProposal.sol:32:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
```
[ListOnOpenseaProposal.sol:L291](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291)
```
contracts/proposals/ListOnOpenseaProposal.sol:291:            for (uint256 i = 0; i < fees.length; ++i) {
```
[PartyGovernance.sol:L306](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306)
```
contracts/party/PartyGovernance.sol:306:        for (uint256 i=0; i < opts.hosts.length; ++i) {
```

[Crowdfund.sol](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol)
```
contracts/crowdfund/Crowdfund.sol:180:        for (uint256 i = 0; i < contributors.length; ++i) {
contracts/crowdfund/Crowdfund.sol:242:        for (uint256 i = 0; i < numContributions; ++i) {
contracts/crowdfund/Crowdfund.sol:300:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
contracts/crowdfund/Crowdfund.sol:348:            for (uint256 i = 0; i < numContributions; ++i) {
```
[TokenDistributor.sol](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol)
```
contracts/distribution/TokenDistributor.sol:230:        for (uint256 i = 0; i < infos.length; ++i) {
contracts/distribution/TokenDistributor.sol:239:        for (uint256 i = 0; i < infos.length; ++i) {
```
----
# G6. Variables: No need to explicitly initialize variables with default values

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

We can use `uint number;` instead of `uint number = 0;`
### Findings 
 // Links to github files
[PartyGovernance.sol:L432](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L432)
```
contracts/party/PartyGovernance.sol:432:        uint256 low = 0;
```
### Recommended Mitigation Steps:
I suggest removing explicit initializations for default values.

----
# G7. Use `abi.encodePacked()` not `abi.encode()`
### Description
Changing `abi.encode` to `abi.encodePacked` can save gas. `abi.encode` pads extra null bytes at the end of the call data which is normally unnecessary. In general, `abi.encodePacked` is more gas-efficient.
 ### Findings
 // Links to github files

 [ListOnZoraProposal.sol:L115](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L115)
```
contracts/proposals/ListOnZoraProposal.sol:115:            return abi.encode(ZoraStep.ListedOnZora, ZoraProgressData({
```
[ListOnOpenseaProposal.sol](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol)
```
contracts/proposals/ListOnOpenseaProposal.sol:164:                    return abi.encode(ListOnOpenseaStep.ListedOnZora, ZoraProgressData({
contracts/proposals/ListOnOpenseaProposal.sol:219:            return abi.encode(ListOnOpenseaStep.ListedOnOpenSea, orderHash, expiry);
```
[ReadOnlyDelegateCall.sol:L23](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L23)
```
contracts/utils/ReadOnlyDelegateCall.sol:23:        abi.encode(s, r).rawRevert();
```
### Recommended Mitigation Steps
You should change **abi.encode** to **abi.encodePacked**

----
# G8. Multiple address mappings can be combined into a single `mapping` of an `address` to a `struct`, where appropriate
### Description
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (**20000 gas**) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~**42 gas per access** due to [not having to recalculate the key's keccak256 hash](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0) (Gkeccak256 - **30 gas**) and that calculation's associated stack operations.

*There are 1 instances of this issue:*
 ### Instances:
 // Links to github files
[PartyGovernance.sol:L207-L209](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L207-L209)
 ### Actual Codes Used:
```
contracts/party/PartyGovernance.sol:207:    mapping(address => bool) public isHost;
contracts/party/PartyGovernance.sol:208:    /// @notice The last person a voter delegated its voting power to.
contracts/party/PartyGovernance.sol:209:    mapping(address => address) public delegationsByVoter;
```
----
# G9. Using private rather than public for constants / immutable, saves gas
### Description
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves approximate of 200-400 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it’s used, and not adding another entry to the method ID table
 ### Findings
 // Links to github files
[ListOnZoraProposal.sol:L64](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L64)

```
contracts/proposals/ListOnZoraProposal.sol:64:    IZoraAuctionHouse public immutable ZORA;
```

[ListOnOpenseaProposal.sol](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#)
```
contracts/proposals/ListOnOpenseaProposal.sol:100:    IOpenseaExchange public immutable SEAPORT;
contracts/proposals/ListOnOpenseaProposal.sol:102:    IOpenseaConduitController public immutable CONDUIT_CONTROLLER;
```

[FractionalizeProposal.sol](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/FractionalizeProposal.sol)
```
contracts/proposals/FractionalizeProposal.sol:31:    IFractionalV1VaultFactory public immutable VAULT_FACTORY;
```
[PartyFactory.sol:L18](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyFactory.sol#L18)
```
contracts/party/PartyFactory.sol:18:    IGlobals public immutable GLOBALS;
```
[Implementation.sol:L9](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/Implementation.sol#L9)
```
contracts/utils/Implementation.sol:9:    address public immutable IMPL;
```
[Proxy.sol:L12](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/Proxy.sol#L12)

```
contracts/utils/Proxy.sol:12:    Implementation public immutable IMPL;
```
[TokenDistributor.sol:L59](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L59)
```
contracts/distribution/TokenDistributor.sol:59:    IGlobals public immutable GLOBALS;
```
----
# G10. `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

 ### Findings
[PartyGovernance.sol:L595](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L595)

```
contracts/party/PartyGovernance.sol:595:        values.votes += votingPower;
```
[Crowdfund.sol](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol)
```
contracts/crowdfund/Crowdfund.sol:243:            ethContributed += contributions[i].amount;
contracts/crowdfund/Crowdfund.sol:352:                    ethOwed += c.amount;
contracts/crowdfund/Crowdfund.sol:355:                    ethUsed += c.amount;
contracts/crowdfund/Crowdfund.sol:359:                    ethUsed += partialEthUsed;
contracts/crowdfund/Crowdfund.sol:374:            votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up
contracts/crowdfund/Crowdfund.sol:411:            totalContributions += amount;
contracts/crowdfund/Crowdfund.sol:427:                    lastContribution.amount += amount;
```
----
# G11. Use a recent version solidity
### Description
Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()/require()` strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

### Findings
```
contracts/vendor/markets/IZoraAuctionHouse.sol:2:pragma solidity ^0.8;
contracts/gatekeepers/IGateKeeper.sol:2:pragma solidity ^0.8;
contracts/proposals/ArbitraryCallsProposal.sol:2:pragma solidity ^0.8;
contracts/proposals/ProposalExecutionEngine.sol:2:pragma solidity ^0.8;
contracts/proposals/vendor/FractionalV1.sol:2:pragma solidity ^0.8;
contracts/proposals/vendor/IOpenseaConduitController.sol:2:pragma solidity ^0.8;
contracts/proposals/vendor/IOpenseaExchange.sol:2:pragma solidity ^0.8;
contracts/proposals/ListOnZoraProposal.sol:2:pragma solidity ^0.8;
contracts/proposals/ProposalStorage.sol:2:pragma solidity ^0.8;
contracts/proposals/LibProposal.sol:2:pragma solidity ^0.8;
contracts/proposals/ListOnOpenseaProposal.sol:2:pragma solidity ^0.8;
contracts/proposals/IProposalExecutionEngine.sol:2:pragma solidity ^0.8;
contracts/proposals/FractionalizeProposal.sol:2:pragma solidity ^0.8;
contracts/party/PartyFactory.sol:2:pragma solidity ^0.8;
contracts/party/PartyGovernance.sol:2:pragma solidity ^0.8;
contracts/party/IPartyFactory.sol:2:pragma solidity ^0.8;
contracts/party/PartyGovernanceNFT.sol:2:pragma solidity ^0.8;
contracts/party/Party.sol:2:pragma solidity ^0.8;
contracts/globals/IGlobals.sol:2:pragma solidity ^0.8;
contracts/globals/Globals.sol:2:pragma solidity ^0.8;
contracts/crowdfund/Crowdfund.sol:2:pragma solidity ^0.8;
contracts/crowdfund/BuyCrowdfund.sol:2:pragma solidity ^0.8;
contracts/crowdfund/CollectionBuyCrowdfund.sol:2:pragma solidity ^0.8;
contracts/crowdfund/CrowdfundFactory.sol:2:pragma solidity ^0.8;
contracts/crowdfund/AuctionCrowdfund.sol:2:pragma solidity ^0.8;
contracts/crowdfund/BuyCrowdfundBase.sol:2:pragma solidity ^0.8;
contracts/crowdfund/CrowdfundNFT.sol:2:pragma solidity ^0.8;
contracts/tokens/IERC721Receiver.sol:2:pragma solidity ^0.8;
contracts/tokens/IERC1155.sol:2:pragma solidity ^0.8;
contracts/tokens/ERC1155Receiver.sol:2:pragma solidity ^0.8;
contracts/tokens/ERC721Receiver.sol:2:pragma solidity ^0.8;
contracts/tokens/IERC20.sol:2:pragma solidity ^0.8;
contracts/tokens/IERC721.sol:2:pragma solidity ^0.8;
contracts/utils/ReadOnlyDelegateCall.sol:2:pragma solidity ^0.8;
contracts/utils/LibSafeERC721.sol:2:pragma solidity ^0.8;
contracts/utils/LibERC20Compat.sol:2:pragma solidity ^0.8;
contracts/utils/LibRawResult.sol:2:pragma solidity ^0.8;
contracts/utils/LibAddress.sol:2:pragma solidity ^0.8;
contracts/utils/LibSafeCast.sol:2:pragma solidity ^0.8;
contracts/utils/EIP165.sol:2:pragma solidity ^0.8;
contracts/utils/Implementation.sol:2:pragma solidity ^0.8;
contracts/utils/Proxy.sol:2:pragma solidity ^0.8;
contracts/market-wrapper/IMarketWrapper.sol:2:pragma solidity ^0.8;
contracts/distribution/ITokenDistributorParty.sol:2:pragma solidity ^0.8;
contracts/distribution/TokenDistributor.sol:2:pragma solidity ^0.8;
contracts/distribution/ITokenDistributor.sol:2:pragma solidity ^0.8;
```

----
