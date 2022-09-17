contracts/crowdfund/CollectionBuyCrowdfund.sol
1. Use pre increment
2. Use unchecked for loop increment
3. Store array length in local variable

```git diff
diff --git a/contracts/crowdfund/CollectionBuyCrowdfund.sol b/contracts/crowdfund/CollectionBuyCrowdfund.sol
index a0517eb..f62f1ea 100644
--- a/contracts/crowdfund/CollectionBuyCrowdfund.sol
+++ b/contracts/crowdfund/CollectionBuyCrowdfund.sol
@@ -59,11 +59,15 @@ contract CollectionBuyCrowdfund is BuyCrowdfundBase {
 
     modifier onlyHost(address[] memory hosts) {
         bool isHost;
-        for (uint256 i; i < hosts.length; i++) {
+        uint256 _len = hosts.length;
+        for (uint256 i; i < _len;) {
             if (hosts[i] == msg.sender) {
                 isHost = true;
                 break;
             }
+            unchecked {
+                ++i;
+            }
         }
 
         if (!isHost) {
```

contracts/crowdfund/Crowdfund.sol

1. Use unchecked for loop increment
2. No need to initialize variable to 0
3. Use unchecked if non under/over flow conditions
```git diff
diff --git a/contracts/crowdfund/Crowdfund.sol b/contracts/crowdfund/Crowdfund.sol
index 5c27346..8c6b32e 100644
--- a/contracts/crowdfund/Crowdfund.sol
+++ b/contracts/crowdfund/Crowdfund.sol
@@ -177,8 +177,11 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
     {
         Party party_ = party;
         CrowdfundLifecycle lc = getCrowdfundLifecycle();
-        for (uint256 i = 0; i < contributors.length; ++i) {
+        for (uint256 i; i < contributors.length;) {
             _burn(contributors[i], lc, party_);
+            unchecked {
+                ++i;
+            }
         }
     }
 
@@ -239,8 +242,11 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
         CrowdfundLifecycle lc = getCrowdfundLifecycle();
         Contribution[] storage contributions = _contributionsByContributor[contributor];
         uint256 numContributions = contributions.length;
-        for (uint256 i = 0; i < numContributions; ++i) {
+        for (uint256 i; i < numContributions;) {
             ethContributed += contributions[i].amount;
+            unchecked {
+                ++i;
+            }
         }
         if (lc == CrowdfundLifecycle.Won || lc == CrowdfundLifecycle.Lost) {
             (ethUsed, ethOwed, votingPower) = _getFinalContribution(contributor);
@@ -297,8 +303,11 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
                 preciousTokenIds
             );
         // Transfer the acquired NFTs to the new party.
-        for (uint256 i = 0; i < preciousTokens.length; ++i) {
+        for (uint256 i; i < preciousTokens.length;) {
             preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
+            unchecked {
+                ++i;
+            }
         }
     }
 
@@ -345,7 +354,7 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
         {
             Contribution[] storage contributions = _contributionsByContributor[contributor];
             uint256 numContributions = contributions.length;
-            for (uint256 i = 0; i < numContributions; ++i) {
+            for (uint256 i; i < numContributions;) {
                 Contribution memory c = contributions[i];
                 if (c.previousTotalContributions >= totalEthUsed) {
                     // This entire contribution was not used.
@@ -359,6 +368,9 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
                     ethUsed += partialEthUsed;
                     ethOwed = c.amount - partialEthUsed;
                 }
+                unchecked {
+                    ++i;
+                }
             }
         }
         // one SLOAD with optimizer on
@@ -425,7 +437,9 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
                 if (lastContribution.previousTotalContributions == previousTotalContributions) {
                     // No one else has contributed since so just reuse the last entry.
                     lastContribution.amount += amount;
-                    contributions[numContributions - 1] = lastContribution;
+                    unchecked {
+                        contributions[numContributions - 1] = lastContribution;
+                    }
                     return;
                 }
             }
```
contracts/distribution/TokenDistributor.sol

1. Use unchecked for loop increment
2. Store array length in local variable
3. Use unchecked if non under/over flow conditions

```git diff
diff --git a/contracts/distribution/TokenDistributor.sol b/contracts/distribution/TokenDistributor.sol
index 1ea71c1..0c60404 100644
--- a/contracts/distribution/TokenDistributor.sol
+++ b/contracts/distribution/TokenDistributor.sol
@@ -167,7 +167,9 @@ contract TokenDistributor is ITokenDistributor {
         amountClaimed = amountClaimed > remainingMemberSupply
             ? remainingMemberSupply
             : amountClaimed;
-        state.remainingMemberSupply = remainingMemberSupply - amountClaimed;
+        unchecked {
+            state.remainingMemberSupply = remainingMemberSupply - amountClaimed;
+        }
 
         // Transfer tokens owed.
         _transfer(
@@ -226,9 +228,13 @@ contract TokenDistributor is ITokenDistributor {
         external
         returns (uint128[] memory amountsClaimed)
     {
-        amountsClaimed = new uint128[](infos.length);
-        for (uint256 i = 0; i < infos.length; ++i) {
+        uint256 _len = infos.length;
+        amountsClaimed = new uint128[](_len);
+        for (uint256 i; i < _len;) {
             amountsClaimed[i] = claim(infos[i], partyTokenIds[i]);
+            unchecked {
+                ++i;
+            }
         }
     }
 
@@ -236,8 +242,11 @@ contract TokenDistributor is ITokenDistributor {
     function batchClaimFee(DistributionInfo[] calldata infos, address payable[] calldata recipients)
         external
     {
-        for (uint256 i = 0; i < infos.length; ++i) {
+        for (uint256 i; i < infos.length;) {
             claimFee(infos[i], recipients[i]);
+            unchecked {
+                ++i;
+            }
         }
     }
```
contracts/party/PartyGovernance.sol

1. Use unchecked for loop increment
2. No need to initialize variable to 0
3. Use unchecked if non under/over flow conditions
```git diff
diff --git a/contracts/party/PartyGovernance.sol b/contracts/party/PartyGovernance.sol
index 5dc6483..b8f9374 100644
--- a/contracts/party/PartyGovernance.sol
+++ b/contracts/party/PartyGovernance.sol
@@ -303,8 +303,11 @@ abstract contract PartyGovernance is
         // Set the precious list.
         _setPreciousList(preciousTokens, preciousTokenIds);
         // Set the party hosts.
-        for (uint256 i=0; i < opts.hosts.length; ++i) {
+        for (uint256 i; i < opts.hosts.length;) {
             isHost[opts.hosts[i]] = true;
+            unchecked {
+                ++i;
+            }
         }
         emit PartyInitialized(opts, preciousTokens, preciousTokenIds);
     }
@@ -430,19 +433,23 @@ abstract contract PartyGovernance is
         // ref: https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/Checkpoints.sol#L39
         uint256 high = snaps.length;
         uint256 low = 0;
-        while (low < high) {
-            uint256 mid = (low + high) / 2;
-            if (snaps[mid].timestamp > timestamp) {
-                // Entry is too recent.
-                high = mid;
-            } else {
-                // Entry is older. This is our best guess for now.
-                low = mid + 1;
+        unchecked {
+            while (low < high) {
+                uint256 mid = (low + high) / 2;
+                if (snaps[mid].timestamp > timestamp) {
+                    // Entry is too recent.
+                    high = mid;
+                } else {
+                    // Entry is older. This is our best guess for now.
+                    low = mid + 1;
+                }
             }
         }
 
         // Return `type(uint256).max` if no valid voting snapshots found.
-        return high == 0 ? type(uint256).max : high - 1;
+        unchecked {
+            return high == 0 ? type(uint256).max : high - 1;
+        }
     }
 
     /// @notice Pledge your intrinsic voting power to a new delegate, removing it from
@@ -529,7 +536,9 @@ abstract contract PartyGovernance is
         onlyDelegateCall
         returns (uint256 proposalId)
     {
-        proposalId = ++lastProposalId;
+        unchecked {
+            proposalId = ++lastProposalId;
+        }
         // Store the time the proposal was created and the proposal hash.
         (
             _proposalStateByProposalId[proposalId].values,
@@ -592,7 +601,9 @@ abstract contract PartyGovernance is
 
         // Increase the total votes that have been cast on this proposal.
         uint96 votingPower = getVotingPowerAt(msg.sender, values.proposedTime, snapIndex);
-        values.votes += votingPower;
+        unchecked {
+            values.votes += votingPower;
+        }
         info.values = values;
         emit ProposalAccepted(proposalId, msg.sender, votingPower);
 
@@ -976,11 +987,13 @@ abstract contract PartyGovernance is
         VotingPowerSnapshot[] storage voterSnaps = _votingPowerSnapshotsByVoter[voter];
         uint256 n = voterSnaps.length;
         // If same timestamp as last entry, overwrite the last snapshot, otherwise append.
-        if (n != 0) {
-            VotingPowerSnapshot memory lastSnap = voterSnaps[n - 1];
-            if (lastSnap.timestamp == snap.timestamp) {
-                voterSnaps[n - 1] = snap;
-                return;
+        unchecked {
+            if (n != 0) {
+                VotingPowerSnapshot memory lastSnap = voterSnaps[n - 1];
+                if (lastSnap.timestamp == snap.timestamp) {
+                    voterSnaps[n - 1] = snap;
+                    return;
+                }
             }
         }
         voterSnaps.push(snap);
@@ -994,7 +1007,9 @@ abstract contract PartyGovernance is
         VotingPowerSnapshot[] storage voterSnaps = _votingPowerSnapshotsByVoter[voter];
         uint256 n = voterSnaps.length;
         if (n != 0) {
-            snap = voterSnaps[n - 1];
+            unchecked {
+                snap = voterSnaps[n - 1];
+            }
         }
     }
```

contracts/proposals/ArbitraryCallsProposal.sol

1. Use unchecked for loop increment
2. No need to initialize variable to 0
3. Use unchecked if non under/over flow conditions
4.  Store array length in local variable
```git diff
diff --git a/contracts/proposals/ArbitraryCallsProposal.sol b/contracts/proposals/ArbitraryCallsProposal.sol
index ec164bc..44c7885 100644
--- a/contracts/proposals/ArbitraryCallsProposal.sol
+++ b/contracts/proposals/ArbitraryCallsProposal.sol
@@ -48,17 +48,21 @@ contract ArbitraryCallsProposal {
         // If not unanimous, keep track of which preciouses we had before the calls
         // so we can check that we still have them later.
         bool[] memory hadPreciouses = new bool[](params.preciousTokenIds.length);
+        uint256 _len = hadPreciouses.length;
         if (!isUnanimous) {
-            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
+            for (uint256 i; i < _len;) {
                 hadPreciouses[i] = _getHasPrecious(
                     params.preciousTokens[i],
                     params.preciousTokenIds[i]
                 );
+                unchecked {
+                    ++i;
+                }
             }
         }
         // Can only forward ETH attached to the call.
         uint256 ethAvailable = msg.value;
-        for (uint256 i = 0; i < calls.length; ++i) {
+        for (uint256 i; i < calls.length;) {
             // Execute an arbitrary call.
             _executeSingleArbitraryCall(
                 i,
@@ -71,11 +75,14 @@ contract ArbitraryCallsProposal {
             // Update the amount of ETH available for the subsequent calls.
             ethAvailable -= calls[i].value;
             emit ArbitraryCallExecuted(params.proposalId, i, calls.length);
+            unchecked {
+                ++i;
+            }
         }
         // If not a unanimous vote and we had a precious beforehand,
         // ensure that we still have it now.
         if (!isUnanimous) {
-            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
+            for (uint256 i; i < _len;) {
                 if (hadPreciouses[i]) {
                     if (!_getHasPrecious(params.preciousTokens[i], params.preciousTokenIds[i])) {
                         revert PreciousLostError(
@@ -84,6 +91,9 @@ contract ArbitraryCallsProposal {
                         );
                     }
                 }
+                unchecked {
+                    ++i;
+                }
             }
         }
         // No next step, so no progressData.
```

contracts/proposals/LibProposal.sol

1. Use unchecked for loop increment
2. No need to initialize variable to 0
```git diff
diff --git a/contracts/proposals/LibProposal.sol b/contracts/proposals/LibProposal.sol
index 747d53d..6a04976 100644
--- a/contracts/proposals/LibProposal.sol
+++ b/contracts/proposals/LibProposal.sol
@@ -11,10 +11,13 @@ library LibProposal {
         pure
         returns (bool)
     {
-        for (uint256 i = 0; i < preciousTokens.length; ++i) {
+        for (uint256 i; i < preciousTokens.length;) {
             if (token == preciousTokens[i]) {
                 return true;
             }
+            unchecked {
+                ++i;
+            }
         }
         return false;
     }
@@ -29,10 +32,13 @@ library LibProposal {
         pure
         returns (bool)
     {
-        for (uint256 i = 0; i < preciousTokens.length; ++i) {
+        for (uint256 i; i < preciousTokens.length;) {
             if (token == preciousTokens[i] && tokenId == preciousTokenIds[i]) {
                 return true;
             }
+            unchecked {
+                ++i;
+            }
         }
         return false;
     }
```
contracts/proposals/ListOnOpenseaProposal.sol

1. Use unchecked for loop increment
2. No need to initialize variable to 0
3. Use unchecked if non under/over flow conditions
```git diff
diff --git a/contracts/proposals/ListOnOpenseaProposal.sol b/contracts/proposals/ListOnOpenseaProposal.sol
index 6ff8f5d..5af4bef 100644
--- a/contracts/proposals/ListOnOpenseaProposal.sol
+++ b/contracts/proposals/ListOnOpenseaProposal.sol
@@ -268,7 +268,9 @@ abstract contract ListOnOpenseaProposal is ZoraHelpers {
             : IOpenseaExchange.OrderType.FULL_RESTRICTED;
         orderParams.salt = 0;
         orderParams.conduitKey = conduitKey;
-        orderParams.totalOriginalConsiderationItems = 1 + fees.length;
+        unchecked {
+            orderParams.totalOriginalConsiderationItems = 1 + fees.length;
+        }
         // What we are selling.
         orderParams.offer = new IOpenseaExchange.OfferItem[](1);
         {
@@ -288,13 +290,18 @@ abstract contract ListOnOpenseaProposal is ZoraHelpers {
             cons.identifierOrCriteria = 0;
             cons.startAmount = cons.endAmount = listPrice;
             cons.recipient = payable(address(this));
-            for (uint256 i = 0; i < fees.length; ++i) {
-                cons = orderParams.consideration[1 + i];
+            for (uint256 i ; i < fees.length;) {
+                unchecked {
+                    cons = orderParams.consideration[1 + i];
+                }
                 cons.itemType = IOpenseaExchange.ItemType.NATIVE;
                 cons.token = address(0);
                 cons.identifierOrCriteria = 0;
                 cons.startAmount = cons.endAmount = fees[i];
                 cons.recipient = feeRecipients[i];
+                unchecked {
+                    ++i;
+                }
             }
         }

```