##### Gas Optimizations

Gas savings are estimated using the gas report of existing `yarn test:gas` tests (the sum of all deployment costs and the sum of the costs of calling all methods + `forge snapshot --diff`) and may vary depending on the implementation of the fix.

**NOTE**: with current tests, method call evaluation is very volatile (±300 000 for the same code), so I include the results of `forge snapshot --diff` because they are more stable.

|       | Issue                                                                                         | Instances | Estimated gas(deployments) | Estimated gas(avg method call) | `forge snapshot --diff` |
| :---: | :-------------------------------------------------------------------------------------------- | :-------: | :------------------------: | :----------------------------: | :---------------------: |
| **1** | Expression can be unchecked when overflow is not possible                                     |    16     |           58 069           |        11 431 ± 300 000        |         708 160         |
| **2** | Using bools for storage incurs overhead                                                       |     6     |           35 242           |       137 433 ± 300 000        |         678 523         |
| **3** | State variables should be cached in stack variables rather than re-reading them from storage  |     5     |           21 616           |        45 963 ± 300 000        |         116 899         |
| **4** | Use custom errors rather than revert()/require() strings to save gas                          |     1     |           18 424           |        -3 095 ± 300 000        |         126 535         |
| **5** | `<array>.length` should not be looked up in every loop of a for-loop                          |     3     |           8 200            |        62 475 ± 300 000        |         17 105          |
| **6** | Division by two should use bit shifting                                                       |     1     |           1 600            |       222 938 ± 300 000        |         87 318          |
| **7** | Call functions only when you need them                                                        |     1     |            600             |        -33567 ± 300 000        |           616           |
| **8** | Prefix increments are cheaper than postfix increments, especially when it's used in for-loops |     1     |            400             |        -49907 ± 300 000        |            5            |
|       | **Overall Gas Saved**                                                                         |  **34**   |        **141 372**         |       **3590 ± 300 000**       |      **1 698 279**      |

**Total: 34 instances over 8 issues**

---

#### 1. **Expression can be unchecked when overflow is not possible.(16 instances)**

- Deployment. Gas Saved: **58 069**

- Minumal Method Call. Gas Saved: **-104 ± 300 000**

- Average Method Call. Gas Saved: **11 431 ± 300 000**

- Maximum Method Call. Gas Saved: **202 526 ± 300 000**

- `forge snapshot --diff`. Gas Saved: **708 160**

##### - contracts/crowdfund/Crowdfund.sol:[180](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L180), [242](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L242), [300](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L300), [348](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L348)

```diff
diff --git a/contracts/crowdfund/Crowdfund.sol b/contracts/crowdfund/Crowdfund.sol
index 5c27346..3cd3013 100644
--- a/contracts/crowdfund/Crowdfund.sol
+++ b/contracts/crowdfund/Crowdfund.sol
@@ -177,8 +177,11 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
  177, 177:     {
  178, 178:         Party party_ = party;
  179, 179:         CrowdfundLifecycle lc = getCrowdfundLifecycle();
- 180     :-        for (uint256 i = 0; i < contributors.length; ++i) {
+      180:+        for (uint256 i = 0; i < contributors.length;) {
  181, 181:             _burn(contributors[i], lc, party_);
+      182:+            unchecked {
+      183:+                ++i;
+      184:+            }
  182, 185:         }
  183, 186:     }
  184, 187:
@@ -239,8 +242,11 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
  239, 242:         CrowdfundLifecycle lc = getCrowdfundLifecycle();
  240, 243:         Contribution[] storage contributions = _contributionsByContributor[contributor];
  241, 244:         uint256 numContributions = contributions.length;
- 242     :-        for (uint256 i = 0; i < numContributions; ++i) {
+      245:+        for (uint256 i = 0; i < numContributions;) {
  243, 246:             ethContributed += contributions[i].amount;
+      247:+            unchecked {
+      248:+                 ++i;
+      249:+            }
  244, 250:         }
  245, 251:         if (lc == CrowdfundLifecycle.Won || lc == CrowdfundLifecycle.Lost) {
  246, 252:             (ethUsed, ethOwed, votingPower) = _getFinalContribution(contributor);
@@ -297,8 +303,11 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
  297, 303:                 preciousTokenIds
  298, 304:             );
  299, 305:         // Transfer the acquired NFTs to the new party.
- 300     :-        for (uint256 i = 0; i < preciousTokens.length; ++i) {
+      306:+        for (uint256 i = 0; i < preciousTokens.length;) {
  301, 307:             preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
+      308:+            unchecked {
+      309:+                 ++i;
+      310:+            }
  302, 311:         }
  303, 312:     }
  304, 313:
@@ -345,7 +354,7 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
  345, 354:         {
  346, 355:             Contribution[] storage contributions = _contributionsByContributor[contributor];
  347, 356:             uint256 numContributions = contributions.length;
- 348     :-            for (uint256 i = 0; i < numContributions; ++i) {
+      357:+            for (uint256 i = 0; i < numContributions;) {
  349, 358:                 Contribution memory c = contributions[i];
  350, 359:                 if (c.previousTotalContributions >= totalEthUsed) {
  351, 360:                     // This entire contribution was not used.
@@ -359,6 +368,9 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
  359, 368:                     ethUsed += partialEthUsed;
  360, 369:                     ethOwed = c.amount - partialEthUsed;
  361, 370:                 }
+      371:+                unchecked {
+      372:+                     ++i;
+      373:+                }
  362, 374:             }
  363, 375:         }
  364, 376:         // one SLOAD with optimizer on
```

##### - contracts/proposals/ArbitraryCallsProposal.sol:[52](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52), [61](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61), [78](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L78)

```diff
diff --git a/contracts/proposals/ArbitraryCallsProposal.sol b/contracts/proposals/ArbitraryCallsProposal.sol
index ec164bc..8afd4d2 100644
--- a/contracts/proposals/ArbitraryCallsProposal.sol
+++ b/contracts/proposals/ArbitraryCallsProposal.sol
@@ -49,16 +49,19 @@ contract ArbitraryCallsProposal {
   49,  49:         // so we can check that we still have them later.
   50,  50:         bool[] memory hadPreciouses = new bool[](params.preciousTokenIds.length);
   51,  51:         if (!isUnanimous) {
-  52     :-            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
+       52:+            for (uint256 i = 0; i < hadPreciouses.length;) {
   53,  53:                 hadPreciouses[i] = _getHasPrecious(
   54,  54:                     params.preciousTokens[i],
   55,  55:                     params.preciousTokenIds[i]
   56,  56:                 );
+       57:+                unchecked {
+       58:+                    ++i;
+       59:+                }
   57,  60:             }
   58,  61:         }
   59,  62:         // Can only forward ETH attached to the call.
   60,  63:         uint256 ethAvailable = msg.value;
-  61     :-        for (uint256 i = 0; i < calls.length; ++i) {
+       64:+        for (uint256 i = 0; i < calls.length;) {
   62,  65:             // Execute an arbitrary call.
   63,  66:             _executeSingleArbitraryCall(
   64,  67:                 i,
@@ -71,11 +74,14 @@ contract ArbitraryCallsProposal {
   71,  74:             // Update the amount of ETH available for the subsequent calls.
   72,  75:             ethAvailable -= calls[i].value;
   73,  76:             emit ArbitraryCallExecuted(params.proposalId, i, calls.length);
+       77:+            unchecked {
+       78:+                ++i;
+       79:+            }
   74,  80:         }
   75,  81:         // If not a unanimous vote and we had a precious beforehand,
   76,  82:         // ensure that we still have it now.
   77,  83:         if (!isUnanimous) {
-  78     :-            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
+       84:+            for (uint256 i = 0; i < hadPreciouses.length;) {
   79,  85:                 if (hadPreciouses[i]) {
   80,  86:                     if (!_getHasPrecious(params.preciousTokens[i], params.preciousTokenIds[i])) {
   81,  87:                         revert PreciousLostError(
@@ -84,6 +90,9 @@ contract ArbitraryCallsProposal {
   84,  90:                         );
   85,  91:                     }
   86,  92:                 }
+       93:+                unchecked {
+       94:+                    ++i;
+       95:+                }
   87,  96:             }
   88,  97:         }
   89,  98:         // No next step, so no progressData.
```

##### - contracts/crowdfund/CollectionBuyCrowdfund.sol:[62](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62)

```diff
diff --git a/contracts/crowdfund/CollectionBuyCrowdfund.sol b/contracts/crowdfund/CollectionBuyCrowdfund.sol
index 73b7e66..c7cc5ea 100644
--- a/contracts/crowdfund/CollectionBuyCrowdfund.sol
+++ b/contracts/crowdfund/CollectionBuyCrowdfund.sol
@@ -59,11 +59,14 @@ contract CollectionBuyCrowdfund is BuyCrowdfundBase {
   59,  59:
   60,  60:     modifier onlyHost(address[] memory hosts) {
   61,  61:         bool isHost;
-  62     :-        for (uint256 i; i < hosts.length; ++i) {
+       62:+        for (uint256 i; i < hosts.length;) {
   63,  63:             if (hosts[i] == msg.sender) {
   64,  64:                 isHost = true;
   65,  65:                 break;
   66,  66:             }
+       67:+            unchecked {
+       68:+                ++i;
+       69:+            }
   67,  70:         }
   68,  71:
   69,  72:         if (!isHost) {
```

##### - contracts/proposals/LibProposal.sol:[14](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14), [32](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L32)

```diff
diff --git a/contracts/proposals/LibProposal.sol b/contracts/proposals/LibProposal.sol
index 747d53d..75e2b29 100644
--- a/contracts/proposals/LibProposal.sol
+++ b/contracts/proposals/LibProposal.sol
@@ -11,10 +11,13 @@ library LibProposal {
   11,  11:         pure
   12,  12:         returns (bool)
   13,  13:     {
-  14     :-        for (uint256 i = 0; i < preciousTokens.length; ++i) {
+       14:+        for (uint256 i = 0; i < preciousTokens.length; ) {
   15,  15:             if (token == preciousTokens[i]) {
   16,  16:                 return true;
   17,  17:             }
+       18:+            unchecked {
+       19:+                ++i;
+       20:+            }
   18,  21:         }
   19,  22:         return false;
   20,  23:     }
@@ -29,10 +32,13 @@ library LibProposal {
   29,  32:         pure
   30,  33:         returns (bool)
   31,  34:     {
-  32     :-        for (uint256 i = 0; i < preciousTokens.length; ++i) {
+       35:+        for (uint256 i = 0; i < preciousTokens.length;) {
   33,  36:             if (token == preciousTokens[i] && tokenId == preciousTokenIds[i]) {
   34,  37:                 return true;
   35,  38:             }
+       39:+            unchecked {
+       40:+                ++i;
+       41:+            }
   36,  42:         }
   37,  43:         return false;
   38,  44:     }
```

##### - contracts/proposals/ListOnOpenseaProposal.sol:[291](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291)

```diff
diff --git a/contracts/proposals/ListOnOpenseaProposal.sol b/contracts/proposals/ListOnOpenseaProposal.sol
index 6ff8f5d..f2d7e98 100644
--- a/contracts/proposals/ListOnOpenseaProposal.sol
+++ b/contracts/proposals/ListOnOpenseaProposal.sol
@@ -288,13 +288,16 @@ abstract contract ListOnOpenseaProposal is ZoraHelpers {
  288, 288:             cons.identifierOrCriteria = 0;
  289, 289:             cons.startAmount = cons.endAmount = listPrice;
  290, 290:             cons.recipient = payable(address(this));
- 291     :-            for (uint256 i = 0; i < fees.length; ++i) {
+      291:+            for (uint256 i = 0; i < fees.length;) {
  292, 292:                 cons = orderParams.consideration[1 + i];
  293, 293:                 cons.itemType = IOpenseaExchange.ItemType.NATIVE;
  294, 294:                 cons.token = address(0);
  295, 295:                 cons.identifierOrCriteria = 0;
  296, 296:                 cons.startAmount = cons.endAmount = fees[i];
  297, 297:                 cons.recipient = feeRecipients[i];
+      298:+                unchecked {
+      299:+                     ++i;
+      300:+                }
  298, 301:             }
  299, 302:         }
  300, 303:         orderHash = _getOrderHash(orderParams);
```

##### - contracts/party/PartyGovernance.sol:[306](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306), [532](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L532)

```diff
diff --git a/contracts/party/PartyGovernance.sol b/contracts/party/PartyGovernance.sol
index 5dc6483..ba458db 100644
--- a/contracts/party/PartyGovernance.sol
+++ b/contracts/party/PartyGovernance.sol
@@ -303,8 +303,11 @@ abstract contract PartyGovernance is
  303, 303:         // Set the precious list.
  304, 304:         _setPreciousList(preciousTokens, preciousTokenIds);
  305, 305:         // Set the party hosts.
- 306     :-        for (uint256 i=0; i < opts.hosts.length; ++i) {
+      306:+        for (uint256 i=0; i < opts.hosts.length;) {
  307, 307:             isHost[opts.hosts[i]] = true;
+      308:+            unchecked {
+      309:+                 ++i;
+      310:+            }
  308, 311:         }
  309, 312:         emit PartyInitialized(opts, preciousTokens, preciousTokenIds);
  310, 313:     }
@@ -529,7 +532,9 @@ abstract contract PartyGovernance is
  529, 532:         onlyDelegateCall
  530, 533:         returns (uint256 proposalId)
  531, 534:     {
- 532     :-        proposalId = ++lastProposalId;
+      535:+        unchecked {
+      536:+            proposalId = ++lastProposalId;
+      537:+        }
  533, 538:         // Store the time the proposal was created and the proposal hash.
  534, 539:         (
  535, 540:             _proposalStateByProposalId[proposalId].values,
```

##### - contracts/distribution/TokenDistributor.sol:[230](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L230), [239](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L239)

```diff
diff --git a/contracts/distribution/TokenDistributor.sol b/contracts/distribution/TokenDistributor.sol
index 1ea71c1..21a1e4a 100644
--- a/contracts/distribution/TokenDistributor.sol
+++ b/contracts/distribution/TokenDistributor.sol
@@ -227,8 +227,11 @@ contract TokenDistributor is ITokenDistributor {
  227, 227:         returns (uint128[] memory amountsClaimed)
  228, 228:     {
  229, 229:         amountsClaimed = new uint128[](infos.length);
- 230     :-        for (uint256 i = 0; i < infos.length; ++i) {
+      230:+        for (uint256 i = 0; i < infos.length;) {
  231, 231:             amountsClaimed[i] = claim(infos[i], partyTokenIds[i]);
+      232:+            unchecked {
+      233:+                 ++i;
+      234:+            }
  232, 235:         }
  233, 236:     }
  234, 237:
@@ -236,8 +239,11 @@ contract TokenDistributor is ITokenDistributor {
  236, 239:     function batchClaimFee(DistributionInfo[] calldata infos, address payable[] calldata recipients)
  237, 240:         external
  238, 241:     {
- 239     :-        for (uint256 i = 0; i < infos.length; ++i) {
+      242:+        for (uint256 i = 0; i < infos.length;) {
  240, 243:             claimFee(infos[i], recipients[i]);
+      244:+            unchecked {
+      245:+                 ++i;
+      246:+            }
  241, 247:         }
  242, 248:     }
  243, 249:
```

##### - contracts/party/PartyGovernance.sol:[440](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L440)

```diff
diff --git a/contracts/party/PartyGovernance.sol b/contracts/party/PartyGovernance.sol
index ba458db..23c3780 100644
--- a/contracts/party/PartyGovernance.sol
+++ b/contracts/party/PartyGovernance.sol
@@ -440,7 +440,9 @@ abstract contract PartyGovernance is
  440, 440:                 high = mid;
  441, 441:             } else {
  442, 442:                 // Entry is older. This is our best guess for now.
- 443     :-                low = mid + 1;
+      443:+                unchecked {
+      444:+                    low = mid + 1;
+      445:+                }
  444, 446:             }
  445, 447:         }
  446, 448:
```

#### 2. **Using bools for storage incurs overhead (6 instances)**

- Deployment. Gas Saved: **35 242**

- Minumal Method Call. Gas Saved: **109 101 ± 300 000**

- Average Method Call. Gas Saved: **137 433 ± 300 000**

- Maximum Method Call. Gas Saved: **359 680 ± 300 000**

- `forge snapshot --diff`. Gas Saved: **678 523**

**NOTE**: this project tries to pack storage variables into a single slot, but sometimes (when bool is not used at the same time as the variables it is packed with) it will be more expensive than just using uint256.(Only cases where uint256 is cheaper are included)

```
// Booleans are more expensive than uint256 or any type that takes up a full
// word because each write operation emits an extra SLOAD to first read the
// slot's contents, replace the bits taken up by the boolean, and then write
// back. This is the compiler's defense against contract upgrades and
// pointer aliasing, and it cannot be disabled.
```

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from 'false' to 'true', after having been 'true' in the past

##### - contracts/crowdfund/Crowdfund.sol:[106](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L106)

```diff
diff --git a/contracts/crowdfund/Crowdfund.sol b/contracts/crowdfund/Crowdfund.sol
index 5c27346..6d43a06 100644
--- a/contracts/crowdfund/Crowdfund.sol
+++ b/contracts/crowdfund/Crowdfund.sol
@@ -103,7 +103,7 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
103, 103:     ///         in bps, where 10,000 = 100%.
104, 104:     uint16 public splitBps;
105, 105:     // Whether the share for split recipient has been claimed through `burn()`.
- 106     :-    bool private _splitRecipientHasBurned;
+      106:+    uint256 private _splitRecipientHasBurned;
107, 107:     /// @notice Hash of party governance options passed into `initialize()`.
108, 108:     ///         Used to check whether the `GovernanceOpts` passed into
109, 109:     ///         `_createParty()` matches.
@@ -455,10 +455,10 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
455, 455:         }
456, 456:         // Split recipient can burn even if they don't have a token.
457, 457:         if (contributor == splitRecipient) {
- 458     :-            if (_splitRecipientHasBurned) {
+      458:+            if (1==_splitRecipientHasBurned) {
459, 459:                 revert SplitRecipientAlreadyBurnedError();
460, 460:             }
- 461     :-            _splitRecipientHasBurned = true;
+      461:+            _splitRecipientHasBurned = 1;
462, 462:         }
463, 463:         // Revert if already burned or does not exist.
464, 464:         if (splitRecipient != contributor || _doesTokenExistFor(contributor)) {
```

##### - contracts/party/PartyGovernance.sol:[148](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L148)

```diff
diff --git a/contracts/party/PartyGovernance.sol b/contracts/party/PartyGovernance.sol
index 5dc6483..628e803 100644
--- a/contracts/party/PartyGovernance.sol
+++ b/contracts/party/PartyGovernance.sol
@@ -145,7 +145,7 @@ abstract contract PartyGovernance is
145, 145:         // Hash of the proposal.
146, 146:         bytes32 hash;
147, 147:         // Whether a member has voted for (accepted) this proposal already.
- 148     :-        mapping (address => bool) hasVoted;
+      148:+        mapping (address => uint256) hasVoted;
149, 149:     }
150, 150:
151, 151:     event Proposed(
@@ -584,11 +584,11 @@ abstract contract PartyGovernance is
584, 584:         }
585, 585:
586, 586:         // Cannot vote twice.
- 587     :-        if (info.hasVoted[msg.sender]) {
+      587:+        if (1==info.hasVoted[msg.sender]) {
588, 588:             revert AlreadyVotedError(msg.sender);
589, 589:         }
590, 590:         // Mark the caller as having voted.
- 591     :-        info.hasVoted[msg.sender] = true;
+      591:+        info.hasVoted[msg.sender] = 1;
592, 592:
593, 593:         // Increase the total votes that have been cast on this proposal.
594, 594:         uint96 votingPower = getVotingPowerAt(msg.sender, values.proposedTime, snapIndex);
```

##### - contracts/party/PartyGovernance.sol:[197](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L197)

```diff
diff --git a/contracts/party/PartyGovernance.sol b/contracts/party/PartyGovernance.sol
index 628e803..8b28455 100644
--- a/contracts/party/PartyGovernance.sol
+++ b/contracts/party/PartyGovernance.sol
@@ -194,7 +194,7 @@ abstract contract PartyGovernance is
194, 194:     IGlobals private immutable _GLOBALS;
195, 195:
196, 196:     /// @notice Whether the DAO has emergency powers for this party.
- 197     :-    bool public emergencyExecuteDisabled;
+      197:+    uint256 public emergencyExecuteDisabled;
198, 198:     /// @notice Distribution fee bps.
199, 199:     uint16 public feeBps;
200, 200:     /// @notice Distribution fee recipient.
@@ -256,7 +256,7 @@ abstract contract PartyGovernance is
256, 256:
257, 257:     // Only if `emergencyExecuteDisabled` is not true.
258, 258:     modifier onlyWhenEmergencyExecuteAllowed() {
- 259     :-        if (emergencyExecuteDisabled) {
+      259:+        if (1==emergencyExecuteDisabled) {
260, 260:             revert OnlyWhenEmergencyActionsAllowedError();
261, 261:         }
262, 262:         _;
@@ -798,7 +798,7 @@ abstract contract PartyGovernance is
798, 798:     /// @notice Revoke the DAO's ability to call emergencyExecute().
799, 799:     /// @dev Either the DAO or the party host can call this.
800, 800:     function disableEmergencyExecute() external onlyPartyDaoOrHost onlyDelegateCall {
- 801     :-        emergencyExecuteDisabled = true;
+      801:+        emergencyExecuteDisabled = 1;
802, 802:     }
803, 803:
804, 804:     function _executeProposal(
diff --git a/contracts/renderers/PartyGovernanceNFTRenderer.sol b/contracts/renderers/PartyGovernanceNFTRenderer.sol
index e88fe38..7af05e4 100644
--- a/contracts/renderers/PartyGovernanceNFTRenderer.sol
+++ b/contracts/renderers/PartyGovernanceNFTRenderer.sol
@@ -18,7 +18,7 @@ contract PartyGovernanceNFTRenderer is IERC721Renderer {
18,  18:
19,  19:     // The renderer is called via delegateCall, so we need to declare the storage layout.
20,  20:     // Run `yarn layout` to generate the current layout.
-  21     :-    bool emergencyExecuteDisabled;
+       21:+    uint256 emergencyExecuteDisabled;
22,  22:     uint16 feeBps;
23,  23:     address payable feeRecipient;
24,  24:     bytes32 preciousListHash;
```

##### - contracts/party/PartyGovernance.sol:[207](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L207)

```diff
diff --git a/contracts/party/PartyGovernance.sol b/contracts/party/PartyGovernance.sol
index 8b28455..57a4a86 100644
--- a/contracts/party/PartyGovernance.sol
+++ b/contracts/party/PartyGovernance.sol
@@ -204,7 +204,7 @@ abstract contract PartyGovernance is
204, 204:     /// @notice The last proposal ID that was used. 0 means no proposals have been made.
205, 205:     uint256 public lastProposalId;
206, 206:     /// @notice Whether an address is a party host.
- 207     :-    mapping(address => bool) public isHost;
+      207:+    mapping(address => uint256) public isHost;
208, 208:     /// @notice The last person a voter delegated its voting power to.
209, 209:     mapping(address => address) public delegationsByVoter;
210, 210:     // Constant governance parameters, fixed from the inception of this party.
@@ -215,7 +215,7 @@ abstract contract PartyGovernance is
215, 215:     mapping(address => VotingPowerSnapshot[]) private _votingPowerSnapshotsByVoter;
216, 216:
217, 217:     modifier onlyHost() {
- 218     :-        if (!isHost[msg.sender]) {
+      218:+        if (0==isHost[msg.sender]) {
219, 219:             revert OnlyPartyHostError();
220, 220:         }
221, 221:         _;
@@ -248,7 +248,7 @@ abstract contract PartyGovernance is
248, 248:     // Only the party DAO multisig or a party host can call.
249, 249:     modifier onlyPartyDaoOrHost() {
250, 250:         address partyDao = _GLOBALS.getAddress(LibGlobals.GLOBAL_DAO_WALLET);
- 251     :-        if (msg.sender != partyDao && !isHost[msg.sender]) {
+      251:+        if (msg.sender != partyDao && 0==isHost[msg.sender]) {
252, 252:             revert OnlyPartyDaoOrHostError(msg.sender, partyDao);
253, 253:         }
254, 254:         _;
@@ -304,7 +304,7 @@ abstract contract PartyGovernance is
304, 304:         _setPreciousList(preciousTokens, preciousTokenIds);
305, 305:         // Set the party hosts.
306, 306:         for (uint256 i=0; i < opts.hosts.length; ++i) {
- 307     :-            isHost[opts.hosts[i]] = true;
+      307:+            isHost[opts.hosts[i]] = 1;
308, 308:         }
309, 309:         emit PartyInitialized(opts, preciousTokens, preciousTokenIds);
310, 310:     }
@@ -459,12 +459,12 @@ abstract contract PartyGovernance is
459, 459:         // 0 is a special case burn address.
460, 460:         if (newPartyHost != address(0)) {
461, 461:             // Cannot transfer host status to an existing host.
- 462     :-            if(isHost[newPartyHost]) {
+      462:+            if(1==isHost[newPartyHost]) {
463, 463:                 revert InvalidNewHostError();
464, 464:             }
- 465     :-            isHost[newPartyHost] = true;
+      465:+            isHost[newPartyHost] = 1;
466, 466:         }
- 467     :-        isHost[msg.sender] = false;
+      467:+        isHost[msg.sender] = 0;
468, 468:         emit HostStatusTransferred(msg.sender, newPartyHost);
469, 469:     }
470, 470:
diff --git a/contracts/renderers/PartyGovernanceNFTRenderer.sol b/contracts/renderers/PartyGovernanceNFTRenderer.sol
index 7af05e4..4930ba9 100644
--- a/contracts/renderers/PartyGovernanceNFTRenderer.sol
+++ b/contracts/renderers/PartyGovernanceNFTRenderer.sol
@@ -23,7 +23,7 @@ contract PartyGovernanceNFTRenderer is IERC721Renderer {
23,  23:     address payable feeRecipient;
24,  24:     bytes32 preciousListHash;
25,  25:     uint256 lastProposalId;
-  26     :-    mapping(address => bool) isHost;
+       26:+    mapping(address => uint256) isHost;
27,  27:     mapping(address => address) delegationsByVoter;
28,  28:     PartyGovernance.GovernanceValues _governanceValues;
29,  29:     mapping(uint256 => PartyGovernance.ProposalState) _proposalStateByProposalId;
diff --git a/sol-tests/party/PartyGovernanceUnit.t.sol b/sol-tests/party/PartyGovernanceUnit.t.sol
index e762347..c5e9ad0 100644
--- a/sol-tests/party/PartyGovernanceUnit.t.sol
+++ b/sol-tests/party/PartyGovernanceUnit.t.sol
@@ -2022,10 +2022,10 @@ contract PartyGovernanceUnitTest is Test, TestUtils {
2022,2022:         gov.abdicate(newHost);
2023,2023:
2024,2024:         // Assert old host is no longer host
-2025     :-        assertEq(gov.isHost(host), false);
+     2025:+        assertEq(gov.isHost(host), 0);
2026,2026:
2027,2027:         // Assert new host is host
-2028     :-        assertEq(gov.isHost(newHost), true);
+     2028:+        assertEq(gov.isHost(newHost), 1);
2029,2029:     }
2030,2030:
2031,2031:     // You cannot transfer host status to an existing host
```

##### - contracts/distribution/TokenDistributor.sol:[30](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L30)

```diff
diff --git a/contracts/distribution/TokenDistributor.sol b/contracts/distribution/TokenDistributor.sol
index 1ea71c1..532f90e 100644
--- a/contracts/distribution/TokenDistributor.sol
+++ b/contracts/distribution/TokenDistributor.sol
@@ -27,7 +27,7 @@ contract TokenDistributor is ITokenDistributor {
   27,  27:         // Whether the distribution's feeRecipient has claimed its fee.
   28,  28:         bool wasFeeClaimed;
   29,  29:         // Whether a governance token has claimed its distribution share.
-  30     :-        mapping(uint256 => bool) hasPartyTokenClaimed;
+       30:+        mapping(uint256 => uint256) hasPartyTokenClaimed;
   31,  31:     }
   32,  32:
   33,  33:     // Arguments for `_createDistribution()`.
@@ -152,11 +152,11 @@ contract TokenDistributor is ITokenDistributor {
  152, 152:             revert InvalidDistributionInfoError(info);
  153, 153:         }
  154, 154:         // The partyTokenId must not have claimed its distribution yet.
- 155     :-        if (state.hasPartyTokenClaimed[partyTokenId]) {
+      155:+        if (1==state.hasPartyTokenClaimed[partyTokenId]) {
  156, 156:             revert DistributionAlreadyClaimedByPartyTokenError(info.distributionId, partyTokenId);
  157, 157:         }
  158, 158:         // Mark the partyTokenId as having claimed their distribution.
- 159     :-        state.hasPartyTokenClaimed[partyTokenId] = true;
+      159:+        state.hasPartyTokenClaimed[partyTokenId] = 1;
  160, 160:
  161, 161:         // Compute amount owed to partyTokenId.
  162, 162:         amountClaimed = getClaimAmount(info.party, info.memberSupply, partyTokenId);
@@ -282,7 +282,7 @@ contract TokenDistributor is ITokenDistributor {
  282, 282:         external
  283, 283:         view returns (bool)
  284, 284:     {
- 285     :-        return _distributionStates[party][distributionId].hasPartyTokenClaimed[partyTokenId];
+      285:+        return 1==_distributionStates[party][distributionId].hasPartyTokenClaimed[partyTokenId];
  286, 286:     }
  287, 287:
  288, 288:     /// @inheritdoc ITokenDistributor
```

##### - contracts/distribution/TokenDistributor.sol:[62](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L62)

```diff
diff --git a/contracts/distribution/TokenDistributor.sol b/contracts/distribution/TokenDistributor.sol
index 532f90e..e49f356 100644
--- a/contracts/distribution/TokenDistributor.sol
+++ b/contracts/distribution/TokenDistributor.sol
@@ -59,7 +59,7 @@ contract TokenDistributor is ITokenDistributor {
   59,  59:     IGlobals public immutable GLOBALS;
   60,  60:
   61,  61:     /// @notice Whether the DAO is no longer allowed to call emergency functions.
-  62     :-    bool public emergencyActionsDisabled;
+       62:+    uint256 public emergencyActionsDisabled;
   63,  63:     /// @notice Last distribution ID for a party.
   64,  64:     mapping(ITokenDistributorParty => uint256) public lastDistributionIdPerParty;
   65,  65:     /// Last known balance of a token, identified by an ID derived from the token.
@@ -83,7 +83,7 @@ contract TokenDistributor is ITokenDistributor {
   83,  83:
   84,  84:     // emergencyActionsDisabled == false
   85,  85:     modifier onlyIfEmergencyActionsAllowed() {
-  86     :-        if (emergencyActionsDisabled) {
+       86:+        if (1==emergencyActionsDisabled) {
   87,  87:             revert EmergencyActionsNotAllowedError();
   88,  88:         }
   89,  89:         _;
@@ -325,7 +325,7 @@ contract TokenDistributor is ITokenDistributor {
  325, 325:
  326, 326:     /// @notice DAO-only function to disable emergency functions forever.
  327, 327:     function disableEmergencyActions() onlyPartyDao external {
- 328     :-        emergencyActionsDisabled = true;
+      328:+        emergencyActionsDisabled = 1;
  329, 329:     }
  330, 330:
  331, 331:     function _createDistribution(CreateDistributionArgs memory args)
```

#### 3. **State variables should be cached in stack variables rather than re-reading them from storage (5 instances)**

- Deployment. Gas Saved: **21 616**

- Minumal Method Call. Gas Saved: **23 247 ± 300 000**

- Average Method Call. Gas Saved: **45 963 ± 300 000**

- Maximum Method Call. Gas Saved: **357 682 ± 300 000**

- `forge snapshot --diff`. Gas Saved: **116 899**

SLOADs are expensive (100 gas after the 1st one) compared to MLOADs/MSTOREs (3 gas each). Storage values read multiple times should instead be cached in memory the first time (costing 1 SLOAD) and then read from this cache to avoid multiple SLOADs.

##### - contracts/crowdfund/Crowdfund.sol:[271](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L271), [276](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L276), [392](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L392)

```diff
diff --git a/contracts/crowdfund/Crowdfund.sol b/contracts/crowdfund/Crowdfund.sol
index 5c27346..5b38227 100644
--- a/contracts/crowdfund/Crowdfund.sol
+++ b/contracts/crowdfund/Crowdfund.sol
@@ -268,13 +268,14 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
  268, 268:         internal
  269, 269:         returns (Party party_)
  270, 270:     {
- 271     :-        if (party != Party(payable(0))) {
- 272     :-            revert PartyAlreadyExistsError(party);
+      271:+        if ((party_ = party) != Party(payable(0))) {
+      272:+            revert PartyAlreadyExistsError(party_);
  273, 273:         }
  274, 274:         {
  275, 275:             bytes16 governanceOptsHash_ = _hashFixedGovernanceOpts(governanceOpts);
- 276     :-            if (governanceOptsHash_ != governanceOptsHash) {
- 277     :-                revert InvalidGovernanceOptionsError(governanceOptsHash_, governanceOptsHash);
+      276:+            bytes32 _governanceOptsHash;
+      277:+            if (governanceOptsHash_ != (_governanceOptsHash = governanceOptsHash)) {
+      278:+                revert InvalidGovernanceOptionsError(governanceOptsHash_, _governanceOptsHash);
  278, 279:             }
  279, 280:         }
  280, 281:         party = party_ = partyFactory
@@ -389,11 +390,12 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
  389, 390:             revert InvalidDelegateError();
  390, 391:         }
  391, 392:         // Must not be blocked by gatekeeper.
- 392     :-        if (gateKeeper != IGateKeeper(address(0))) {
- 393     :-            if (!gateKeeper.isAllowed(contributor, gateKeeperId, gateData)) {
+      393:+        IGateKeeper _gateKeeper;
+      394:+        if ((_gateKeeper = gateKeeper) != IGateKeeper(address(0))) {
+      395:+            if (!_gateKeeper.isAllowed(contributor, gateKeeperId, gateData)) {
  394, 396:                 revert NotAllowedByGateKeeperError(
  395, 397:                     contributor,
- 396     :-                    gateKeeper,
+      398:+                    _gateKeeper,
  397, 399:                     gateKeeperId,
  398, 400:                     gateData
  399, 401:                 );
```

##### - contracts/crowdfund/AuctionCrowdfund.sol:[162](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L162)

```diff
diff --git a/contracts/crowdfund/AuctionCrowdfund.sol b/contracts/crowdfund/AuctionCrowdfund.sol
index e997e8a..8c9bc73 100644
--- a/contracts/crowdfund/AuctionCrowdfund.sol
+++ b/contracts/crowdfund/AuctionCrowdfund.sol
@@ -159,15 +159,16 @@ contract AuctionCrowdfund is Implementation, Crowdfund {
  159, 159:         _bidStatus = AuctionCrowdfundStatus.Busy;
  160, 160:         // Make sure the auction is not finalized.
  161, 161:         uint256 auctionId_ = auctionId;
- 162     :-        if (market.isFinalized(auctionId_)) {
+      162:+        IMarketWrapper _market;
+      163:+        if ((_market=market).isFinalized(auctionId_)) {
  163, 164:             revert AuctionFinalizedError(auctionId_);
  164, 165:         }
  165, 166:         // Only bid if we are not already the highest bidder.
- 166     :-        if (market.getCurrentHighestBidder(auctionId_) == address(this)) {
+      167:+        if (_market.getCurrentHighestBidder(auctionId_) == address(this)) {
  167, 168:             revert AlreadyHighestBidderError();
  168, 169:         }
  169, 170:         // Get the minimum necessary bid to be the highest bidder.
- 170     :-        uint96 bidAmount = market.getMinimumBid(auctionId_).safeCastUint256ToUint96();
+      171:+        uint96 bidAmount = _market.getMinimumBid(auctionId_).safeCastUint256ToUint96();
  171, 172:         // Make sure the bid is less than the maximum bid.
  172, 173:         if (bidAmount > maximumBid) {
  173, 174:             revert ExceedsMaximumBidError(bidAmount, maximumBid);
@@ -175,7 +176,7 @@ contract AuctionCrowdfund is Implementation, Crowdfund {
  175, 176:         lastBid = bidAmount;
  176, 177:         // No need to check that we have `bidAmount` since this will attempt to
  177, 178:         // transfer `bidAmount` ETH to the auction platform.
- 178     :-        (bool s, bytes memory r) = address(market).delegatecall(abi.encodeCall(
+      179:+        (bool s, bytes memory r) = address(_market).delegatecall(abi.encodeCall(
  179, 180:             IMarketWrapper.bid,
  180, 181:             (auctionId_, bidAmount)
  181, 182:         ));
```

##### - contracts/crowdfund/BuyCrowdfundBase.sol:[118](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfundBase.sol#L118)

```diff
diff --git a/contracts/crowdfund/BuyCrowdfundBase.sol b/contracts/crowdfund/BuyCrowdfundBase.sol
index e886e57..fa0d445 100644
--- a/contracts/crowdfund/BuyCrowdfundBase.sol
+++ b/contracts/crowdfund/BuyCrowdfundBase.sol
@@ -115,7 +115,7 @@ abstract contract BuyCrowdfundBase is Implementation, Crowdfund {
  115, 115:         {
  116, 116:             uint96 maximumPrice_ = maximumPrice;
  117, 117:             if (maximumPrice_ != 0 && callValue > maximumPrice_) {
- 118     :-                revert MaximumPriceError(callValue, maximumPrice);
+      118:+                revert MaximumPriceError(callValue, maximumPrice_);
  119, 119:             }
  120, 120:             // If the purchase would be free, set the settled price to
  121, 121:             // `totalContributions` so everybody who contributed wins.
```

#### 4. **Use custom errors rather than revert()/require() strings to save gas (1 instance)**

- Deployment. Gas Saved: **18 424**

- Minumal Method Call. Gas Saved: **20 249 ± 300 000**

- Average Method Call. Gas Saved: **-3 095 ± 300 000**

- Maximum Method Call. Gas Saved: **-114 589 ± 300 000**

- `forge snapshot --diff`. Gas Saved: **126 535**

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas

##### - contracts/crowdfund/CrowdfundNFT.sol:[29](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundNFT.sol#L29)

```diff
diff --git a/contracts/crowdfund/CrowdfundNFT.sol b/contracts/crowdfund/CrowdfundNFT.sol
index 8c6208f..02bc2c6 100644
--- a/contracts/crowdfund/CrowdfundNFT.sol
+++ b/contracts/crowdfund/CrowdfundNFT.sol
@@ -11,6 +11,7 @@ import "../globals/LibGlobals.sol";
11,  11: contract CrowdfundNFT is IERC721, EIP165, ReadOnlyDelegateCall {
12,  12:     error AlreadyBurnedError(address owner, uint256 tokenId);
13,  13:     error InvalidTokenError(uint256 tokenId);
+       14:+    error AlwaysRevert();
14,  15:
15,  16:     // The `Globals` contract storing global configuration values. This contract
16,  17:     // is immutable and it’s address will never change.
@@ -26,7 +27,7 @@ contract CrowdfundNFT is IERC721, EIP165, ReadOnlyDelegateCall {
26,  27:     mapping (uint256 => address) private _owners;
27,  28:
28,  29:     modifier alwaysRevert() {
-  29     :-        revert('ALWAYS FAILING');
+       30:+        revert AlwaysRevert();
30,  31:         _; // Compiler requires this.
31,  32:     }
32,  33:
```

#### 5. **`<array>.length` should not be looked up in every loop of a for-loop (3 instances)**

- Deployment. Gas Saved: **8 200**

- Minumal Method Call. Gas Saved: **69 470 ± 300 000**

- Average Method Call. Gas Saved: **62 475 ± 300 000**

- Maximum Method Call. Gas Saved: **48 162 ± 300 000**

- `forge snapshot --diff`. Gas Saved: **17 105**

The overheads outlined below are PER LOOP, excluding the first loop

`storage` arrays incur a Gwarmaccess (100 gas)
`memory` arrays use MLOAD (3 gas)
`calldata` arrays use CALLDATALOAD (3 gas)
Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset

##### - contracts/proposals/ArbitraryCallsProposal.sol:[52](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52)

```diff
diff --git a/contracts/proposals/ArbitraryCallsProposal.sol b/contracts/proposals/ArbitraryCallsProposal.sol
index ec164bc..7f933c5 100644
--- a/contracts/proposals/ArbitraryCallsProposal.sol
+++ b/contracts/proposals/ArbitraryCallsProposal.sol
@@ -49,7 +49,8 @@ contract ArbitraryCallsProposal {
   49,  49:         // so we can check that we still have them later.
   50,  50:         bool[] memory hadPreciouses = new bool[](params.preciousTokenIds.length);
   51,  51:         if (!isUnanimous) {
-  52     :-            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
+       52:+            uint256 len = hadPreciouses.length;
+       53:+            for (uint256 i = 0; i < len; ++i) {
   53,  54:                 hadPreciouses[i] = _getHasPrecious(
   54,  55:                     params.preciousTokens[i],
   55,  56:                     params.preciousTokenIds[i]
```

##### - contracts/party/PartyGovernance.sol:[306](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306)

```diff
diff --git a/contracts/party/PartyGovernance.sol b/contracts/party/PartyGovernance.sol
index 5dc6483..cbbf3b4 100644
--- a/contracts/party/PartyGovernance.sol
+++ b/contracts/party/PartyGovernance.sol
@@ -303,7 +303,8 @@ abstract contract PartyGovernance is
  303, 303:         // Set the precious list.
  304, 304:         _setPreciousList(preciousTokens, preciousTokenIds);
  305, 305:         // Set the party hosts.
- 306     :-        for (uint256 i=0; i < opts.hosts.length; ++i) {
+      306:+        uint256 len = opts.hosts.length;
+      307:+        for (uint256 i=0; i < len; ++i) {
  307, 308:             isHost[opts.hosts[i]] = true;
  308, 309:         }
  309, 310:         emit PartyInitialized(opts, preciousTokens, preciousTokenIds);
```

##### - contracts/crowdfund/Crowdfund.sol:[180](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L180)

```diff
diff --git a/contracts/crowdfund/Crowdfund.sol b/contracts/crowdfund/Crowdfund.sol
index 5c27346..e167e39 100644
--- a/contracts/crowdfund/Crowdfund.sol
+++ b/contracts/crowdfund/Crowdfund.sol
@@ -177,7 +177,8 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
  177, 177:     {
  178, 178:         Party party_ = party;
  179, 179:         CrowdfundLifecycle lc = getCrowdfundLifecycle();
- 180     :-        for (uint256 i = 0; i < contributors.length; ++i) {
+      180:+        uint256 len = contributors.length;
+      181:+        for (uint256 i = 0; i < len; ++i) {
  181, 182:             _burn(contributors[i], lc, party_);
  182, 183:         }
  183, 184:     }
```

#### 6. **Division by two should use bit shifting (1 instance)**

- Deployment. Gas Saved: **1 600**

- Minumal Method Call. Gas Saved: **203 819 ± 300 000**

- Average Method Call. Gas Saved: **222 938 ± 300 000**

- Maximum Method Call. Gas Saved: **313 121 ± 300 000**

- `forge snapshot --diff`. Gas Saved: **87 318**

`<x> / 2` is the same as `<x> >> 1`. The DIV opcode costs 5 gas, whereas SHR only costs 3 gas

##### - contracts/party/PartyGovernance.sol:[434](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L434)

```diff
diff --git a/contracts/party/PartyGovernance.sol b/contracts/party/PartyGovernance.sol
index 5dc6483..cb21f7c 100644
--- a/contracts/party/PartyGovernance.sol
+++ b/contracts/party/PartyGovernance.sol
@@ -431,7 +431,7 @@ abstract contract PartyGovernance is
  431, 431:         uint256 high = snaps.length;
  432, 432:         uint256 low = 0;
  433, 433:         while (low < high) {
- 434     :-            uint256 mid = (low + high) / 2;
+      434:+            uint256 mid = (low + high) >> 1;
  435, 435:             if (snaps[mid].timestamp > timestamp) {
  436, 436:                 // Entry is too recent.
  437, 437:                 high = mid;
```

#### 7. **Call functions only when you need them (1 instance)**

- Deployment. Gas Saved: **600**

- Minumal Method Call. Gas Saved: **-21986 ± 300 000**

- Average Method Call. Gas Saved: **-33567 ± 300 000**

- Maximum Method Call. Gas Saved: **44774 ± 300 000**

- `forge snapshot --diff`. Gas Saved: **616**

##### - contracts/crowdfund/Crowdfund.sol:[239](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L239)

```diff
diff --git a/contracts/crowdfund/Crowdfund.sol b/contracts/crowdfund/Crowdfund.sol
index 5c27346..de29ea6 100644
--- a/contracts/crowdfund/Crowdfund.sol
+++ b/contracts/crowdfund/Crowdfund.sol
@@ -236,12 +236,12 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
  236, 236:             uint256 votingPower
  237, 237:         )
  238, 238:     {
- 239     :-        CrowdfundLifecycle lc = getCrowdfundLifecycle();
  240, 239:         Contribution[] storage contributions = _contributionsByContributor[contributor];
  241, 240:         uint256 numContributions = contributions.length;
  242, 241:         for (uint256 i = 0; i < numContributions; ++i) {
  243, 242:             ethContributed += contributions[i].amount;
  244, 243:         }
+      244:+        CrowdfundLifecycle lc = getCrowdfundLifecycle();
  245, 245:         if (lc == CrowdfundLifecycle.Won || lc == CrowdfundLifecycle.Lost) {
  246, 246:             (ethUsed, ethOwed, votingPower) = _getFinalContribution(contributor);
  247, 247:         }
```

#### 8. **Prefix increments are cheaper than postfix increments, especially when it's used in for-loops (1 instances).**

- Deployment. Gas Saved: **400**

- Minumal Method Call. Gas Saved: **-22938 ± 300 000**

- Average Method Call. Gas Saved: **-49907 ± 300 000**

- Maximum Method Call. Gas Saved: **-246113 ± 300 000**

- `forge snapshot --diff`. Gas Saved: **5**

##### - contracts/crowdfund/CollectionBuyCrowdfund.sol:[62](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62)

```diff
diff --git a/contracts/crowdfund/CollectionBuyCrowdfund.sol b/contracts/crowdfund/CollectionBuyCrowdfund.sol
index a0517eb..73b7e66 100644
--- a/contracts/crowdfund/CollectionBuyCrowdfund.sol
+++ b/contracts/crowdfund/CollectionBuyCrowdfund.sol
@@ -59,7 +59,7 @@ contract CollectionBuyCrowdfund is BuyCrowdfundBase {
   59,  59:
   60,  60:     modifier onlyHost(address[] memory hosts) {
   61,  61:         bool isHost;
-  62     :-        for (uint256 i; i < hosts.length; i++) {
+       62:+        for (uint256 i; i < hosts.length; ++i) {
   63,  63:             if (hosts[i] == msg.sender) {
   64,  64:                 isHost = true;
   65,  65:                 break;
```
