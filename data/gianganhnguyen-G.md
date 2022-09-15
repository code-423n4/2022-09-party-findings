# 1. [G-1]  ++i  cost less gas compare i++ in for loops

File CollectionBuyCrowdfund.sol, line 62:

    for (uint256 i; i < hosts.length; i++) {
       ...
    }

Becomes:

    for (uint256 i; i < hosts.length; ++i) {
        ...
    }

##### Instances include:

File PartyHelpers.sol, line 64, 85, 115

# 2. [G-2] Cache the length of arrays in the loops

File CollectionBuyCrowdfund.sol, line 62:

    for (uint256 i; i < hosts.length; i++) {
       ...
    }

Becomes:

    uint256 lengthHosts = hosts.length;
    for (uint256 i; i < lengthHosts; ++i) {
       ...
    }

##### Instances include:

File Crowdfund.sol, line 180, 300
File TokenDistributor.sol, line 230, 239
File PartyGovernance.sol, line 306
File ArbitraryCallsProposal.sol, line 52, 61, 78
File LibProposal.sol, line 14, 32
File ListOnOpenseaProposal.sol, line 291
File PartyHelpers.sol, line 64, 85
File ERC1155.sol, line 80, 118

# 3. [G-3] Using uncheck blocks to save gas - increments in for loop can be uncheck

These increment operations never need to be checked for over/underflow because the variable will never reach the max number of uint256.

 File CollectionBuyCrowdfund.sol, line 62:

    for (uint256 i; i < hosts.length; i++) {
       ...
    }

Becomes:

    uint256 lengthHosts = hosts.length;
    for (uint256 i; i < lengthHosts;) {
          ...
          unchecked { ++i; }
      }

##### Instances include:

File Crowdfund.sol, line 180, 242, 300, 348
File TokenDistributor.sol, line 230, 239
File PartyGovernance.sol, line 306
File ArbitraryCallsProposal.sol, line 52, 61, 78
File LibProposal.sol, line 14, 32
File ListOnOpenseaProposal.sol, line 291
File PartyHelpers.sol, line 64, 85, 115
File Strings.sol, line 57
File ERC1155.sol, line 118

# 4. [G-4] Initialize uint256 i; is cheaper than uint256 i = 0

##### Instances include:

File Crowdfund.sol, line 180, 242, 300, 348
File TokenDistributor.sol, line 230, 239
File PartyGovernance.sol, line 306
File ArbitraryCallsProposal.sol, line 52, 61, 78
File LibProposal.sol, line 14, 32
File ListOnOpenseaProposal.sol, line 291
File PartyHelpers.sol, line 64, 85, 115
File ERC1155.sol, line 80, 118, 168,  198

# 5. [G-5] Using uncheck blocks to save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block

 File Crowdfund.sol, line 242:

    for (uint256 i = 0; i < numContributions; ++i) {
       ethContributed += contributions[i].amount;
    }

Becomes:

    unchecked {
      for (uint256 i = 0; i < numContributions;) {
         ethContributed += contributions[i].amount;
          ++i;
      }
    }

 File Crowdfund.sol, line 348:

    for (uint256 i = 0; i < numContributions; ++i) {
       Contribution memory c = contributions[i];
       if (c.previousTotalContributions >= totalEthUsed) {
         // This entire contribution was not used.
         ethOwed += c.amount;
       } else if (c.previousTotalContributions + c.amount <= totalEthUsed) {
         // This entire contribution was used.
         ethUsed += c.amount;
       } else {
         // This contribution was partially used.
         uint256 partialEthUsed = totalEthUsed - c.previousTotalContributions;
         ethUsed += partialEthUsed;
         ethOwed = c.amount - partialEthUsed;
         }
    }

Becomes:

    unchecked {
      for (uint256 i = 0; i < numContributions; ++i) {
         Contribution memory c = contributions[i];
         if (c.previousTotalContributions >= totalEthUsed) {
           // This entire contribution was not used.
           ethOwed += c.amount;
         } else if (c.previousTotalContributions + c.amount <= totalEthUsed) {
           // This entire contribution was used.
           ethUsed += c.amount;
         } else {
           // This contribution was partially used.
           uint256 partialEthUsed = totalEthUsed - c.previousTotalContributions;
           ethUsed += partialEthUsed;
           ethOwed = c.amount - partialEthUsed;
           }
      }
    }

 File Crowdfund.sol, line 370:

    votingPower = ((1e4 - splitBps_) * ethUsed) / 1e4;

Becomes:

    unchecked {
      votingPower = ((1e4 - splitBps_) * ethUsed) / 1e4;
    }

 File Crowdfund.sol, line 374:

    votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4;

Becomes:

    unchecked {
      votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4;
    }

 File Crowdfund.sol, line 411:

    totalContributions += amount;

Becomes:

    unchecked {
      totalContributions += amount;
    }

 File Crowdfund.sol, line 427:

    lastContribution.amount += amount;

Becomes:

    unchecked {
      lastContribution.amount += amount;
    }


 File TokenDistributor.sol, line 352:

    uint128 fee = supply * args.feeBps / 1e4;
    uint128 memberSupply = supply - fee;

Becomes:

    uint128 fee;
    uint128 memberSupply;
    unchecked {
       fee = supply * args.feeBps / 1e4;
       memberSupply = supply - fee;
    }

 File TokenDistributor.sol, line 381:

    _storedBalances[balanceId] -= amount;

Becomes:

    unchecked {
       _storedBalances[balanceId] -= amount;
    }

 File AllowListGateKeeper.sol, line 33:

    uint96 id_ = ++_lastId;

Becomes:

    uint96 id_;
    unchecked {
      id_ = ++_lastId;
    }

 File AllowListGateKeeper.sol, line 170:

    state.remainingMemberSupply = remainingMemberSupply - amountClaimed;

Becomes:

    unchecked {
      state.remainingMemberSupply = remainingMemberSupply - amountClaimed;
    }

 File TokenGateKeeper.sol, line 48:

    uint96 id_ = ++_lastId;

Becomes:

    uint96 id_;
    unchecked {
      id_ = ++_lastId;
    }

 File KoansMarketWrapper.sol, line 79:

    return amount + ((amount * minBidIncrementPercentage) / 100);

Becomes:

    uint265 result;
    unchecked {
      result = amount + ((amount * minBidIncrementPercentage) / 100);
    }
    return result;

 File KoansMarketWrapper.sol, line 123:

    bool settledNormally = auctionId != currentAuctionId;
    bool settledWhenPaused = auctionId == currentAuctionId && settled;

Becomes:

    bool settledNormally;
    bool settledWhenPaused;
    unchecked {
      settledNormally = auctionId != currentAuctionId;
      settledWhenPaused = auctionId == currentAuctionId && settled;
    }

 File NounsMarketWrapper.sol, line 39:

    return auctionId == currentAuctionId && block.timestamp < endTime;

Becomes:

    bool tmp;
    unchecked {
      tmp = currentAuctionId && block.timestamp < endTime;
    }
    return auctionId == tmp;

 File NounsMarketWrapper.sol, line 52:

    return auctionId == tokenId && auctionExists(auctionId);

Becomes:

    bool tmp;
    unchecked {
      tmp = tokenId && auctionExists(auctionId);
    }
    return auctionId == tmp;

 File NounsMarketWrapper.sol, line 77:

    return amount + ((amount * minBidIncrementPercentage) / 100);

Becomes:

    bool result;
    unchecked {
      result= amount + ((amount * minBidIncrementPercentage) / 100);
    }
    return result;

 File NounsMarketWrapper.sol, line 121:

    bool settledNormally = auctionId != currentAuctionId;
    bool settledWhenPaused = auctionId == currentAuctionId && settled;

Becomes:

    bool settledNormally;
    bool settledWhenPaused;
    unchecked {
      settledNormally = auctionId != currentAuctionId;
      settledWhenPaused = auctionId == currentAuctionId && settled;
    }

 File ZoraMarketWrapper.sol, line 57:

    return
        _auction.tokenId == tokenId &&
        _auction.tokenContract == IERC721(nftContract) &&
        _auction.auctionCurrency == IERC20(address(0));

Becomes:

    bool result;
    unchecked {
      result= 
        _auction.tokenId == tokenId &&
        _auction.tokenContract == IERC721(nftContract) &&
        _auction.auctionCurrency == IERC20(address(0));
    }
    return result;

 File ZoraMarketWrapper.sol, line 80:

    return _auction.amount + (_auction.amount * minBidIncrementPercentage / 100);

Becomes:

    bool result;
    unchecked {
      result= _auction.amount + (_auction.amount * minBidIncrementPercentage / 100);
    }
    return result;

 File PartyGovernance.sol, line 433:

    while (low < high) {
       ...
    }

Becomes:

    unchecked {
      while (low < high) {
         ...
      }
    }

 File PartyGovernance.sol, line 445:

    return high == 0 ? type(uint256).max : high - 1;

Becomes:

    uint256 index;
    unchecked {
      index = high == 0 ? type(uint256).max : high - 1;
    }
    return index;

 File PartyGovernance.sol, line 532:

    proposalId = ++lastProposalId;

Becomes:

    unchecked {
      proposalId = ++lastProposalId;
    }

 File ArbitraryCallsProposal.sol, line 72:

    ethAvailable -= calls[i].value;

Becomes:

    unchecked {
      ethAvailable -= calls[i].value;
    }

 File PartyHelpers.sol, line 116:

    uint256 currIndex = startTokenId + i;

Becomes:

    uint256 currIndex;
    unchecked {
      uint256 currIndex = startTokenId + i;
    }

 File ERC1155.sol, line 84:

    balanceOf[from][id] -= amount;
    balanceOf[to][id] += amount;

Becomes:

    unchecked {
      balanceOf[from][id] -= amount;
      balanceOf[to][id] += amount;
    }

 File ERC1155.sol, line 168:

    for (uint256 i = 0; i < idsLength; ) {
      balanceOf[to][ids[i]] += amounts[i];
      unchecked {
        ++i;
      }
    }

Becomes:

    unchecked {
      for (uint256 i = 0; i < idsLength; ) {
        balanceOf[to][ids[i]] += amounts[i];
        ++i;
      }
    }

 File ERC1155.sol, line 198:

    for (uint256 i = 0; i < idsLength; ) {
      balanceOf[from][ids[i]] -= amounts[i];
      unchecked {
        ++i;
      }
    }

Becomes:

    unchecked {
      for (uint256 i = 0; i < idsLength; ) {
        balanceOf[from][ids[i]] -= amounts[i];
        ++i;
      }
    }

# 6. [G-6]  Using memory instead of storage variable

File Crowdfund.sol, line 240:

    Contribution[] storage contributions = _contributionsByContributor[contributor];

Becomes:

    Contribution[] memory contributions = _contributionsByContributor[contributor];

##### Instances include:

File Crowdfund.sol, line 346

# 6. [G-6]  <x> += y cost more gas than <x> = <x> + <y>

##### Instances include:

File Crowdfund.sol, line 243, 352, 355, 359, 374, 411, 427
File TokenDistributor.sol, line 381, 
File PartyGovernance.sol, line 595, 959
File ArbitraryCallsProposal.sol, line 72
File ERC20.sol, line 76, 81, 98, 103, 183, 188, 195, 200
File ERC1155.sol, line 51, 52, 84, 85, 145, 169, 199, 216
