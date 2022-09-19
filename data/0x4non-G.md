# Gas

## Save up to **56.177%** of gas (accordign to test snapshot) using unchecked on safe operation math

> Since the release of Solidity 0.8.0, arithmetic overflow and underflow are taken care of by the Solidity compiler. In this sense, the contracts are more secure (from the arithmetic perspective) but a bit more expensive, in terms of gas. This is because behind the curtains there are op-codes checking if the number obtained post-operation makes sense (in an addition, for example, the result must be bigger than at least one of the terms).

There are some places in the code where you can assume that the operation is safe and it wouldnt overflow or underflow.

For testing this i have create a snapshot;
```bash
forge snapshot --snap base-gas
# make all the changes in the files
# then test gas change
forge snapshot --diff base-gas
```

This are some of the places i think is safe to use unchecked
```diff
diff --git a/contracts/crowdfund/CollectionBuyCrowdfund.sol b/contracts/crowdfund/CollectionBuyCrowdfund.sol
index a0517eb..28927a9 100644
--- a/contracts/crowdfund/CollectionBuyCrowdfund.sol
+++ b/contracts/crowdfund/CollectionBuyCrowdfund.sol
@@ -59,10 +59,12 @@ contract CollectionBuyCrowdfund is BuyCrowdfundBase {
 
     modifier onlyHost(address[] memory hosts) {
         bool isHost;
-        for (uint256 i; i < hosts.length; i++) {
-            if (hosts[i] == msg.sender) {
-                isHost = true;
-                break;
+        unchecked {
+            for (uint256 i; i < hosts.length; ++i) {
+                if (hosts[i] == msg.sender) {
+                    isHost = true;
+                    break;
+                }
             }
         }
 
diff --git a/contracts/crowdfund/Crowdfund.sol b/contracts/crowdfund/Crowdfund.sol
index 5c27346..8b91216 100644
--- a/contracts/crowdfund/Crowdfund.sol
+++ b/contracts/crowdfund/Crowdfund.sol
@@ -177,8 +177,11 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
     {
         Party party_ = party;
         CrowdfundLifecycle lc = getCrowdfundLifecycle();
-        for (uint256 i = 0; i < contributors.length; ++i) {
-            _burn(contributors[i], lc, party_);
+        // safe uncheck inc
+        unchecked {
+            for (uint256 i; i < contributors.length; ++i) {
+                _burn(contributors[i], lc, party_);
+            }
         }
     }
 
@@ -239,8 +242,12 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
         CrowdfundLifecycle lc = getCrowdfundLifecycle();
         Contribution[] storage contributions = _contributionsByContributor[contributor];
         uint256 numContributions = contributions.length;
-        for (uint256 i = 0; i < numContributions; ++i) {
-            ethContributed += contributions[i].amount;
+        // safe uncheck inc
+        unchecked {
+            for (uint256 i = 0; i < numContributions; ++i) {
+                // safe add impossible to overflow
+                ethContributed += contributions[i].amount;
+            }
         }
         if (lc == CrowdfundLifecycle.Won || lc == CrowdfundLifecycle.Lost) {
             (ethUsed, ethOwed, votingPower) = _getFinalContribution(contributor);
@@ -296,9 +303,12 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
                 preciousTokens,
                 preciousTokenIds
             );
-        // Transfer the acquired NFTs to the new party.
-        for (uint256 i = 0; i < preciousTokens.length; ++i) {
-            preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
+        // safe uncheck inc
+        unchecked {
+            // Transfer the acquired NFTs to the new party.
+            for (uint256 i; i < preciousTokens.length; ++i) {
+                preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
+            }
         }
     }

@@ -345,7 +355,7 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
         {
             Contribution[] storage contributions = _contributionsByContributor[contributor];
             uint256 numContributions = contributions.length;
-            for (uint256 i = 0; i < numContributions; ++i) {
+            for (uint256 i; i < numContributions;) {
                 Contribution memory c = contributions[i];
                 if (c.previousTotalContributions >= totalEthUsed) {
                     // This entire contribution was not used.
@@ -359,19 +369,26 @@ abstract contract Crowdfund is ERC721Receiver, CrowdfundNFT {
                     ethUsed += partialEthUsed;
                     ethOwed = c.amount - partialEthUsed;
                 }
+                // safe uncheck inc
+                unchecked { ++i; }
             }
         }
-        // one SLOAD with optimizer on
-        address splitRecipient_ = splitRecipient;
-        uint256 splitBps_ = splitBps;
-        if (splitRecipient_ == address(0)) {
-            splitBps_ = 0;
-        }
-        votingPower = ((1e4 - splitBps_) * ethUsed) / 1e4;
-        if (splitRecipient_ == contributor) {
-            // Split recipient is also the contributor so just add the split
-            // voting power.
-            votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up
+
+        unchecked {
+            // one SLOAD with optimizer on
+            address splitRecipient_ = splitRecipient;
+            uint256 splitBps_ = splitBps;
+            if (splitRecipient_ == address(0)) {
+                splitBps_ = 0;
+            }
+            // safe math, splitBps_ is always equal or lower to 1e4
+            votingPower = ((1e4 - splitBps_) * ethUsed) / 1e4;
+            if (splitRecipient_ == contributor) {
+                // Split recipient is also the contributor so just add the split
+                // voting power.
+                // safe math operation
+                votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up
+            }
         }
     }
 
diff --git a/contracts/distribution/TokenDistributor.sol b/contracts/distribution/TokenDistributor.sol
index 1ea71c1..3e869e6 100644
--- a/contracts/distribution/TokenDistributor.sol
+++ b/contracts/distribution/TokenDistributor.sol
@@ -227,8 +227,11 @@ contract TokenDistributor is ITokenDistributor {
         returns (uint128[] memory amountsClaimed)
     {
         amountsClaimed = new uint128[](infos.length);
-        for (uint256 i = 0; i < infos.length; ++i) {
-            amountsClaimed[i] = claim(infos[i], partyTokenIds[i]);
+        // safe uncheck inc
+        unchecked {
+            for (uint256 i; i < infos.length; ++i) {
+                amountsClaimed[i] = claim(infos[i], partyTokenIds[i]);
+            }
         }
     }
 
@@ -236,8 +239,11 @@ contract TokenDistributor is ITokenDistributor {
     function batchClaimFee(DistributionInfo[] calldata infos, address payable[] calldata recipients)
         external
     {
-        for (uint256 i = 0; i < infos.length; ++i) {
-            claimFee(infos[i], recipients[i]);
+        // safe uncheck inc
+        unchecked {
+            for (uint256 i = 0; i < infos.length; ++i) {
+                claimFee(infos[i], recipients[i]);
+            }
         }
     }
 
@@ -349,18 +355,23 @@ contract TokenDistributor is ITokenDistributor {
         }
 
         // Create a distribution.
-        uint128 fee = supply * args.feeBps / 1e4;
+        uint128 fee;
+        // safe math operation
+        unchecked { fee = supply * args.feeBps / 1e4; }
         uint128 memberSupply = supply - fee;
 
-        info = DistributionInfo({
-            tokenType: args.tokenType,
-            distributionId: ++lastDistributionIdPerParty[args.party],
-            token: args.token,
-            party: args.party,
-            memberSupply: memberSupply,
-            feeRecipient: args.feeRecipient,
-            fee: fee
-        });
+        unchecked {
+            info = DistributionInfo({
+                tokenType: args.tokenType,
+                // safe uncheck inc
+                distributionId: ++lastDistributionIdPerParty[args.party],
+                token: args.token,
+                party: args.party,
+                memberSupply: memberSupply,
+                feeRecipient: args.feeRecipient,
+                fee: fee
+            });
+        }
         (
             _distributionStates[args.party][info.distributionId].distributionHash15,
             _distributionStates[args.party][info.distributionId].remainingMemberSupply
diff --git a/contracts/gatekeepers/AllowListGateKeeper.sol b/contracts/gatekeepers/AllowListGateKeeper.sol
index c281fb5..63573b9 100644
--- a/contracts/gatekeepers/AllowListGateKeeper.sol
+++ b/contracts/gatekeepers/AllowListGateKeeper.sol
@@ -30,7 +30,9 @@ contract AllowListGateKeeper is IGateKeeper {
     /// @param merkleRoot The merkle root to use for the allowlist.
     /// @return id The ID of the new gate.
     function createGate(bytes32 merkleRoot) external returns (bytes12 id) {
-        uint96 id_ = ++_lastId;
+        uint96 id_;
+        // safe uncheck inc
+        unchecked { id_ = ++_lastId; }
         merkleRoots[id_] = merkleRoot;
         id = bytes12(id_);
     }
diff --git a/contracts/gatekeepers/TokenGateKeeper.sol b/contracts/gatekeepers/TokenGateKeeper.sol
index 341507e..a459fd2 100644
--- a/contracts/gatekeepers/TokenGateKeeper.sol
+++ b/contracts/gatekeepers/TokenGateKeeper.sol
@@ -45,7 +45,9 @@ contract TokenGateKeeper is IGateKeeper {
         external
         returns (bytes12 id)
     {
-        uint96 id_ = ++_lastId;
+        uint96 id_;
+        // safe uncheck inc
+        unchecked { id_= ++_lastId; }
         id = bytes12(id_);
         gateInfo[id_].token = token;
         gateInfo[id_].minimumBalance = minimumBalance;
diff --git a/contracts/party/PartyGovernance.sol b/contracts/party/PartyGovernance.sol
index 5dc6483..09e060b 100644
--- a/contracts/party/PartyGovernance.sol
+++ b/contracts/party/PartyGovernance.sol
@@ -303,8 +303,11 @@ abstract contract PartyGovernance is
         // Set the precious list.
         _setPreciousList(preciousTokens, preciousTokenIds);
         // Set the party hosts.
-        for (uint256 i=0; i < opts.hosts.length; ++i) {
-            isHost[opts.hosts[i]] = true;
+        // safe uncheck inc
+        unchecked {
+            for (uint256 i; i < opts.hosts.length; ++i) {
+                isHost[opts.hosts[i]] = true;
+            }
         }
         emit PartyInitialized(opts, preciousTokens, preciousTokenIds);
     }
@@ -529,7 +532,8 @@ abstract contract PartyGovernance is
         onlyDelegateCall
         returns (uint256 proposalId)
     {
-        proposalId = ++lastProposalId;
+        // safe uncheck inc
+        unchecked { proposalId = ++lastProposalId; }
         // Store the time the proposal was created and the proposal hash.
         (
             _proposalStateByProposalId[proposalId].values,
diff --git a/contracts/party/PartyGovernanceNFT.sol b/contracts/party/PartyGovernanceNFT.sol
index 33952ae..4edd925 100644
--- a/contracts/party/PartyGovernanceNFT.sol
+++ b/contracts/party/PartyGovernanceNFT.sol
@@ -126,7 +126,9 @@ contract PartyGovernanceNFT is
         onlyMinter
         onlyDelegateCall
     {
-        uint256 tokenId = ++tokenCount;
+        uint256 tokenId;
+        // safe uncheck inc
+        unchecked { tokenId = ++tokenCount; }
         votingPowerByTokenId[tokenId] = votingPower;
         _adjustVotingPower(owner, votingPower.safeCastUint256ToInt192(), delegate);
         _mint(owner, tokenId);
diff --git a/contracts/proposals/ArbitraryCallsProposal.sol b/contracts/proposals/ArbitraryCallsProposal.sol
index ec164bc..3961a5d 100644
--- a/contracts/proposals/ArbitraryCallsProposal.sol
+++ b/contracts/proposals/ArbitraryCallsProposal.sol
@@ -58,7 +58,7 @@ contract ArbitraryCallsProposal {
         }
         // Can only forward ETH attached to the call.
         uint256 ethAvailable = msg.value;
-        for (uint256 i = 0; i < calls.length; ++i) {
+        for (uint256 i; i < calls.length;) {
             // Execute an arbitrary call.
             _executeSingleArbitraryCall(
                 i,
@@ -71,11 +71,13 @@ contract ArbitraryCallsProposal {
             // Update the amount of ETH available for the subsequent calls.
             ethAvailable -= calls[i].value;
             emit ArbitraryCallExecuted(params.proposalId, i, calls.length);
+            // safe uncheck inc
+            unchecked { ++i; }
         }
         // If not a unanimous vote and we had a precious beforehand,
         // ensure that we still have it now.
         if (!isUnanimous) {
-            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
+            for (uint256 i; i < hadPreciouses.length;) {
                 if (hadPreciouses[i]) {
                     if (!_getHasPrecious(params.preciousTokens[i], params.preciousTokenIds[i])) {
                         revert PreciousLostError(
@@ -84,6 +86,8 @@ contract ArbitraryCallsProposal {
                         );
                     }
                 }
+                // safe uncheck inc
+                unchecked { ++i; }
             }
         }
         // No next step, so no progressData.
diff --git a/contracts/utils/PartyHelpers.sol b/contracts/utils/PartyHelpers.sol
index 03af63e..5eb5433 100644
--- a/contracts/utils/PartyHelpers.sol
+++ b/contracts/utils/PartyHelpers.sol
@@ -61,11 +61,13 @@ contract PartyHelpers {
     {
         Party p = Party(payable(party));
         membersAndDelegates = new MemberAndDelegate[](members.length);
-        for (uint256 i = 0; i < members.length; i++) {
+        for (uint256 i; i < members.length;) {
             membersAndDelegates[i] = MemberAndDelegate({
                 member: members[i],
                 delegate: p.delegationsByVoter(members[i])
             });
+            // safe uncheck inc
+            unchecked { ++i; }
         }
     }
 
@@ -82,11 +84,13 @@ contract PartyHelpers {
     {
         Party p = Party(payable(party));
         memberAndVotingPower = new MemberAndVotingPower[](voters.length);
-        for (uint256 i = 0; i < voters.length; i++) {
+        for (uint256 i; i < voters.length;) {
             memberAndVotingPower[i] = MemberAndVotingPower({
                 member: voters[i],
                 votingPower: p.getVotingPowerAt(voters[i], timestamp, indexes[i])
             });
+            // safe uncheck inc
+            unchecked { ++i; }
         }
     }
 
@@ -112,15 +116,19 @@ contract PartyHelpers {
 
         nftInfos = new NftInfo[](count);
 
-        for (uint256 i = 0; i < count; i++) {
-            uint256 currIndex = startTokenId + i;
-            address owner = p.ownerOf(currIndex);
-            uint256 intrinsicVotingPower = p.votingPowerByTokenId(currIndex);
-            nftInfos[i] = NftInfo({
-                intrinsicVotingPower: intrinsicVotingPower,
-                owner: owner,
-                tokenId: currIndex
-            });
+        unchecked {
+            // safe uncheck ++i
+            for (uint256 i; i < count; ++i) {
+                // safe unchecke add
+                uint256 currIndex = startTokenId + i;
+                address owner = p.ownerOf(currIndex);
+                uint256 intrinsicVotingPower = p.votingPowerByTokenId(currIndex);
+                nftInfos[i] = NftInfo({
+                    intrinsicVotingPower: intrinsicVotingPower,
+                    owner: owner,
+                    tokenId: currIndex
+                });
+            }
         }
     }
 }
```