# Summary

In general, the codebase is well-written but looks a bit overcomplicated (usage of assembler, many structures with the same fields, custom implementation of common patterns). Contracts can be contributed using "standard" libraries like solmate or openzeppilin instead of a custom implementation of common patterns (eg. reentrancy protection). Or, if a custom implementation is preferred, they should be better documented.

## Low-Risk Issues

|       | Issue                                                                                                  | Instances |
| :---: | :----------------------------------------------------------------------------------------------------- | :-------: |
| **1** | require()/revert() statements should have descriptive reason strings/error                             |     4     |
| **2** | Critical changes should use two-step procedure                                                         |     1     |
| **3** | Missing checks for address(0x0) when assigning values to address immutable variables or RBAC variables |     6     |
| **4** | Unspecific Compiler Version Pragma                                                                     |    26     |

## Total: 37 instances over 4 issues

---

Low-Risk Issues:

#### 1. **require()/revert() statements should have descriptive reason strings/error (4 instances)**

##### Impact:

An empty revert string doesn't give the user any information about the reason for the revert, so they can't fix it and use contracts, or they need to lose some gas to try and figure it out by trial and error.

##### Fix:

##### - scope/ProposalExecutionEngine.sol:[246-247](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L246-L247)

```diff
diff --git a/scope/ProposalExecutionEngine.sol b/scope/ProposalExecutionEngine.sol
index 7961b28..ac3d19b 100644
--- a/scope/ProposalExecutionEngine.sol
+++ b/scope/ProposalExecutionEngine.sol
@@ -243,8 +243,8 @@ contract ProposalExecutionEngine is
  243, 243:             mstore(add(proposalData, 4), sub(mload(proposalData), 4))
  244, 244:             offsetProposalData := add(proposalData, 4)
  245, 245:         }
- 246     :-        require(proposalType != ProposalType.Invalid);
- 247     :-        require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));
+      246:+        require(proposalType != ProposalType.Invalid, "Invalid proposal type");
+      247:+        require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes), "Invalid proposal type");
  248, 248:     }
  249, 249:
  250, 250:     // Upgrade implementation to the latest version.
```

##### - contracts/utils/ReadOnlyDelegateCall.sol:[20](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L20)

```diff
diff --git a/scope/ReadOnlyDelegateCall.sol b/scope/ReadOnlyDelegateCall.sol
index c2e6313..bc99cd1 100644
--- a/scope/ReadOnlyDelegateCall.sol
+++ b/scope/ReadOnlyDelegateCall.sol
@@ -17,7 +17,7 @@ contract ReadOnlyDelegateCall {
   17,  17:     // Delegatecall into implement and revert with the raw result.
   18,  18:     function delegateCallAndRevert(address impl, bytes memory callData) external {
   19,  19:         // Attempt to gate to only `_readOnlyDelegateCall()` invocations.
-  20     :-        require(msg.sender == address(this));
+       20:+        require(msg.sender == address(this), "Only Delegate Call");
   21,  21:         (bool s, bytes memory r) = impl.delegatecall(callData);
   22,  22:         // Revert with success status and return data.
   23,  23:         abi.encode(s, r).rawRevert();
```

#### 2. **Critical changes should use a two-step procedure (1 instance)**

##### Impact:

Any errors/typos in the address can lead to access blocking

##### Fix:

##### - contracts/globals/Globals.sol:[28](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L28)

```diff
diff --git a/contracts/globals/Globals.sol b/contracts/globals/Globals.sol
index 71b1f89..487a869 100644
--- a/contracts/globals/Globals.sol
+++ b/contracts/globals/Globals.sol
@@ -6,12 +6,15 @@ import "./IGlobals.sol";
    6,   6: /// @notice Contract storing global configuration values.
    7,   7: contract Globals is IGlobals {
    8,   8:     address public multiSig;
+        9:+
+       10:+    address private pendingMultiSig;
    9,  11:     // key -> word value
   10,  12:     mapping(uint256 => bytes32) private _wordValues;
   11,  13:     // key -> word value -> isIncluded
   12,  14:     mapping(uint256 => mapping(bytes32 => bool)) private _includedWordValues;
   13,  15:
   14,  16:     error OnlyMultiSigError();
+       17:+    error OnlyPendingMultiSig();
   15,  18:
   16,  19:     modifier onlyMultisig() {
   17,  20:         if (msg.sender != multiSig) {
@@ -25,7 +28,13 @@ contract Globals is IGlobals {
   25,  28:     }
   26,  29:
   27,  30:     function transferMultiSig(address newMultiSig) external onlyMultisig {
-  28     :-        multiSig = newMultiSig;
+       31:+        pendingMultiSig = newMultiSig;
+       32:+    }
+       33:+
+       34:+    function acceptMultiSig() external {
+       35:+        if (msg.sender != pendingMultiSig) revert OnlyPendingMultiSig();
+       36:+        multiSig = pendingMultiSig;
+       37:+        delete pendingMultiSig;
   29,  38:     }
   30,  39:
   31,  40:     function getBytes32(uint256 key) external view returns (bytes32) {
```

#### 3. **Missing checks for address(0x0) when assigning values to address immutable variables or RBAC variables (6 instances)**

##### Impact:

Loss of access

##### Fix:

##### - contracts/globals/Globals.sol:[24](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L24), [28](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L28)

```diff
diff --git a/contracts/globals/Globals.sol b/contracts/globals/Globals.sol
index 71b1f89..d08499b 100644
--- a/contracts/globals/Globals.sol
+++ b/contracts/globals/Globals.sol
@@ -12,6 +12,7 @@ contract Globals is IGlobals {
   12,  12:     mapping(uint256 => mapping(bytes32 => bool)) private _includedWordValues;
   13,  13:
   14,  14:     error OnlyMultiSigError();
+       15:+    error ZeroAddress();
   15,  16:
   16,  17:     modifier onlyMultisig() {
   17,  18:         if (msg.sender != multiSig) {
@@ -21,10 +22,12 @@ contract Globals is IGlobals {
   21,  22:     }
   22,  23:
   23,  24:     constructor(address multiSig_) {
+       25:+        if (multiSig_ == address(0)) revert ZeroAddress();
   24,  26:         multiSig = multiSig_;
   25,  27:     }
   26,  28:
   27,  29:     function transferMultiSig(address newMultiSig) external onlyMultisig {
+       30:+        if (newMultiSig == address(0)) revert ZeroAddress();
   28,  31:         multiSig = newMultiSig;
   29,  32:     }
   30,  33:
```

##### Impact:

The implementation logic requires that the contract be created with a constructor and assigned to global objects before initialization, but it is possible to call initialization via factories on an uncreated contract (Implementation(address(0))) and this call can succeed, but any other function will revert.

I think it's better to check in the getter because the setter can use the null address to remove the implementation.

##### Fix:

##### - contracts/globals/Globals.sol:[35](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L35),[39](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L39),[43](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L43),[47](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L47),

```diff
diff --git a/contracts/globals/Globals.sol b/contracts/globals/Globals.sol
index d08499b..6c917f0 100644
--- a/contracts/globals/Globals.sol
+++ b/contracts/globals/Globals.sol
@@ -32,19 +32,27 @@ contract Globals is IGlobals {
   32,  32:     }
   33,  33:
   34,  34:     function getBytes32(uint256 key) external view returns (bytes32) {
-  35     :-        return _wordValues[key];
+       35:+        bytes32 value_;
+       36:+        if ((value_ = _wordValues[key]) == bytes32(0)) revert ZeroAddress();
+       37:+        return value_;
   36,  38:     }
   37,  39:
   38,  40:     function getUint256(uint256 key) external view returns (uint256) {
-  39     :-        return uint256(_wordValues[key]);
+       41:+        bytes32 value_;
+       42:+        if ((value_ = _wordValues[key]) == bytes32(0)) revert ZeroAddress();
+       43:+        return uint256(value_);
   40,  44:     }
   41,  45:
   42,  46:     function getAddress(uint256 key) external view returns (address) {
-  43     :-        return address(uint160(uint256(_wordValues[key])));
+       47:+        bytes32 value_;
+       48:+        if ((value_ = _wordValues[key]) == bytes32(0)) revert ZeroAddress();
+       49:+        return address(uint160(uint256(value_)));
   44,  50:     }
   45,  51:
   46,  52:     function getImplementation(uint256 key) external view returns (Implementation) {
-  47     :-        return Implementation(address(uint160(uint256(_wordValues[key]))));
+       53:+        bytes32 value_;
+       54:+        if ((value_ = _wordValues[key]) == bytes32(0)) revert ZeroAddress();
+       55:+        return Implementation(address(uint160(uint256(value_))));
   48,  56:     }
   49,  57:
   50,  58:     function getIncludesBytes32(uint256 key, bytes32 value) external view returns (bool) {
```

##### Minimal PoC:

```solidity
contract ZeroImplPoC is Test {
    Globals globals = new Globals(address(this));

    // @note the party variable exists, but the constructor has not been run
    Party party;

    AuctionCrowdfund auctionCrowdfund = new AuctionCrowdfund(globals);
    CrowdfundFactory crowdfundFactory = new CrowdfundFactory(globals);
    AuctionCrowdfund.AuctionCrowdfundOptions auctionCrowdfundOptions;
    Crowdfund.FixedGovernanceOpts fixedGovernanceOpts;

    PartyFactory partyFactory = new PartyFactory(globals);

    MockMarketWrapper market = new MockMarketWrapper();
    DummyERC721 tokenToBuy;
    IGateKeeper defaultGateKeeper;
    bytes12 defaultGateKeeperId;
    ProposalExecutionEngine eng =
        new ProposalExecutionEngine(
            globals,
            IOpenseaExchange(vm.addr(0x053)),
            IOpenseaConduitController(vm.addr(0x0533)),
            IZoraAuctionHouse(vm.addr(0x507aa4)),
            IFractionalV1VaultFactory(vm.addr(0xf14f))
        );

    address initialContributor = vm.addr(0x1417141);
    address initialDelegate = vm.addr(0xd417141);
    address host1 = vm.addr(0x40571);
    address host2 = vm.addr(0x40572);
    address host3 = vm.addr(0x40573);

    address contributor1 = vm.addr(0xc04761b1);

    address delegate1 = vm.addr(0xd04761b1);

    address bidder = vm.addr(0xb1d37);

    address splitRecipient = vm.addr(0x5117);

    uint256 auctionId;
    uint256 tokenId;

    function setup_options() internal {
        fixedGovernanceOpts.hosts.push(host1);
        fixedGovernanceOpts.hosts.push(host2);
        fixedGovernanceOpts.hosts.push(host3);
        fixedGovernanceOpts.voteDuration = 1 days;
        fixedGovernanceOpts.executionDelay = 0.5 days;
        fixedGovernanceOpts.passThresholdBps = 0.51e4;

        (auctionId, tokenId) = market.createAuction(1337);
        tokenToBuy = market.nftContract();

        auctionCrowdfundOptions = AuctionCrowdfund.AuctionCrowdfundOptions({
            name: "AuditCrowdfund",
            symbol: "AC",
            auctionId: auctionId,
            market: market,
            nftContract: tokenToBuy,
            nftTokenId: tokenId,
            duration: 1 hours,
            maximumBid: 10e18,
            splitRecipient: payable(splitRecipient),
            splitBps: 1e3,
            initialContributor: initialContributor,
            initialDelegate: initialDelegate,
            gateKeeper: defaultGateKeeper,
            gateKeeperId: defaultGateKeeperId,
            governanceOpts: fixedGovernanceOpts
        });
    }

    function setup_globals() internal {
        globals.setAddress(
            LibGlobals.GLOBAL_AUCTION_CF_IMPL,
            address(auctionCrowdfund)
        );
        globals.setAddress(
            LibGlobals.GLOBAL_PARTY_FACTORY,
            address(partyFactory)
        );
        globals.setAddress(
            LibGlobals.GLOBAL_PROPOSAL_ENGINE_IMPL,
            address(eng)
        );
        globals.setAddress(LibGlobals.GLOBAL_PARTY_IMPL, address(party));
    }

    function test_zero_impl() public {
        setup_options();
        setup_globals();

        vm.deal(initialContributor, 1 ether);
        vm.prank(initialContributor);
        auctionCrowdfund = crowdfundFactory.createAuctionCrowdfund{value: 100}(
            auctionCrowdfundOptions,
            ""
        );

        vm.deal(contributor1, 1 ether);
        vm.prank(contributor1);
        auctionCrowdfund.contribute{value: 1337}(delegate1, "");

        vm.prank(bidder);
        auctionCrowdfund.bid();

        market.endAuction(auctionId);

        // @note will not revert.
        party = auctionCrowdfund.finalize(fixedGovernanceOpts);

        auctionCrowdfund.burn(payable(contributor1));
        auctionCrowdfund.burn(payable(initialContributor));

        console.logAddress(globals.getAddress(LibGlobals.GLOBAL_PARTY_IMPL));

        // @note will revert
        party.getProposalExecutionEngine(); //revert without reasons

        // @note will revert
        party.getVotingPowerAt(initialDelegate, uint40(block.timestamp));

        // @note will revert
        PartyGovernance.Proposal memory proposal = PartyGovernance.Proposal({
            maxExecutableTime: uint40(block.timestamp + 1000),
            cancelDelay: 100,
            proposalData: abi.encodePacked([0])
        });
        vm.prank(initialDelegate);
        uint256 proposalId = party.propose(proposal, 13);
    }
}
```

#### 4. **Unspecific Compiler Version Pragma (26 instances)**

Avoid floating pragmas for non-library contracts.

##### Impact:

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

##### Recommendation:

Avoid floating pragmas. We highly recommend pinning a concrete compiler version (latest without security issues) in at least the top-level “deployed” contracts to make it unambiguous which compiler version is being used. Rule of thumb: a flattened source-unit should have at least one non-floating concrete solidity compiler version pragma.

##### PoC:

ArbitraryCallsProposal.sol:pragma solidity ^0.8;
AuctionCrowdfund.sol:pragma solidity ^0.8;
BuyCrowdfundBase.sol:pragma solidity ^0.8;
BuyCrowdfund.sol:pragma solidity ^0.8;
CollectionBuyCrowdfund.sol:pragma solidity ^0.8;
CrowdfundFactory.sol:pragma solidity ^0.8;
CrowdfundNFT.sol:pragma solidity ^0.8;
Crowdfund.sol:pragma solidity ^0.8;
EIP165.sol:pragma solidity ^0.8;
ERC1155Receiver.sol:pragma solidity ^0.8;
ERC721Receiver.sol:pragma solidity ^0.8;
FractionalizeProposal.sol:pragma solidity ^0.8;
FractionalV1.sol:pragma solidity ^0.8;
Globals.sol:pragma solidity ^0.8;
Implementation.sol:pragma solidity ^0.8;
ListOnOpenseaProposal.sol:pragma solidity ^0.8;
ListOnZoraProposal.sol:pragma solidity ^0.8;
PartyFactory.sol:pragma solidity ^0.8;
PartyGovernanceNFT.sol:pragma solidity ^0.8;
PartyGovernance.sol:pragma solidity ^0.8;
Party.sol:pragma solidity ^0.8;
ProposalExecutionEngine.sol:pragma solidity ^0.8;
ProposalStorage.sol:pragma solidity ^0.8;
Proxy.sol:pragma solidity ^0.8;
ReadOnlyDelegateCall.sol:pragma solidity ^0.8;
TokenDistributor.sol:pragma solidity ^0.8;
