# Disclaimer

- Dear, judges and sponsors! The gas optimization report consists of the general optimization patterns and unique optimization cases. I higly suggest you to pay more attention on unique optimization cases, since it requires more time to highlight and the value is higher. I'll attach every single occurance of unique cases, buf for general optimization pattern - only random one.

  - The following G-0x corresponds to unique cases:
    - **[G-05](G-05)**
    - **[G-06](G-06)**
    - **[G-07](G-07)**
    - **[G-09](G-09)**
    - **[G-10](G-10)**
    - **[G-11](G-11)**
    - **[G-12](G-12)**

# Table of contents

- **[[0x0] Disclaimer](#0x0)**
- **[[G-01] Try ++i instead of i++](G-01)**
- **[[G-02] Try `unchecked{++i}` instead of `i++` in loops](G-02)**
- **[[G-03] Consider `a = a + b` instead of `a += b`](G-03)**
- **[[G-04] Consider marking onlyOwner functions as payable](G-04)**
- **[[G-05] Use binary shifting instead of `a / 2^x, x > 0`](G-05)**
- **[[G-06] Cache state variables, `MLOAD` << `SLOAD`](G-06)**
- **[[G-07] Add `require()/reverts` before some computations](G-07)**
- **[[G-09] Single `struct` can be splited into multiple parts](G-09)**
- **[[G-10] `Internal` functions can be inlined](G-10)**
- **[[G-11] Cache the array.length into a variable](G-11)**
- **[[G-12] Unused the named return variables](G-12)**

## **[G-01] Try ++i instead of i++**<a name="G-01"></a>

### ***Description:***

  - In case of `i++`, the compiler has to to create a temp variable to return `i` (if there's a need) and then `i` gets incremented.  
  - In case of `++i`, the compiler just simply returns already incremented value.

### ***All occurances:***

  - Contracts:
  
    ```Solidity
      file: party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol 
      .........................................................................
      
        // Lines: [60-74]
            modifier onlyHost(address[] memory hosts) {
                bool isHost;
                for (uint256 i; i < hosts.length; i++) {
                    if (hosts[i] == msg.sender) {
                        isHost = true;
                        break;
                    }
                }

                if (!isHost) {
                    revert OnlyPartyHostError();
                }

                _;
            }
    ```

## **[G-02] Try `unchecked{++i};` instead of `i++;`/`++i;` in loops**<a name="G-02"></a>

### ***Description:***

  - Since [Solidity 0.8.0](https://github.com/ethereum/solidity/releases/tag/v0.8.0), all arithmetic operations revert on over- and underflow by default Therefore, `unchecked` box can be used to prevent all unnecessary checks, if it's no a way to get a reversion.
  
  - There are ~1e80 atoms in the universe, so 2^256 is closed to that number, therefore it's no a way to be overflowed, because of the gas limit as well. 

### ***All occurances:***

- Contracts:

    ```Solidity
      file: party-contracts-c4/contracts/crowdfund/Crowdfund.sol 
      ...............................
      
        // Lines: [348-362]
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
    ```

## **[G-03] Consider `a = a + b` instead of `a += b`**<a name="G-03"></a>

### ***Description:***

- It has an impact on the deployment cost and the cost for distinct transaction as well.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: party-contracts-c4/contracts/crowdfund/Crowdfund.sol 
      ...............................

        // Lines: [348-362]
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
    
    ```

## **[G-04] Consider marking onlyOwner functions as payable**<a name="G-04"></a>

### ***Description:***

- This one is a bit questionable, but you can try that out. So, the compiler adds some extra conditions in case of non-payable, but we know that `onlyOwner` modifier will be reverted, if the user invoke following methods.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol 
      ...............................
      
        // Lines: [113-123]
          function buy(
              uint256 tokenId,
              address payable callTarget,
              uint96 callValue,
              bytes calldata callData,
              FixedGovernanceOpts memory governanceOpts
          )
              external
              onlyHost(governanceOpts.hosts)
              returns (Party party_)
          {}
        ```

## **[G-05] Use binary shifting instead of `a / 2^x, x > 0` or `a * 2^x, x > 0`**<a name="G-05"></a>

### ***Description:***

- It's also pretty impactful one, especially in loops.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: party-contracts-c4/contracts/party/PartyGovernance.sol 
      ...............................
      
        // Lines: [433-442]
          while (low < high) {
              uint256 mid = (low + high) / 2;
              if (snaps[mid].timestamp > timestamp) {
                  // Entry is too recent.
                  high = mid;
              } else {
                  // Entry is older. This is our best guess for now.
                  low = mid + 1;
              }
          }
        ```

## **[G-06] Cache state variables, `MLOAD` << `SLOAD`**<a name="G-06"></a>

### ***Description:***

- `MLOAD` costs only 3 units of gas, `SLOAD`(warm access) is about 100 units. Therefore, cache, when it's possible.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol 
      ..................................................................

        // Lines: [116-119]
          // Comment: use cached maximumPrice_  in revert

              uint96 maximumPrice_ = maximumPrice;
              if (maximumPrice_ != 0 && callValue > maximumPrice_) {
                  revert MaximumPriceError(callValue, maximumPrice);
              }

      file: party-contracts-c4/contracts/crowdfund/Crowdfund.sol 
      ..........................................................

        // Lines: [240-244]
          // Comment: use memory instead of storage since you're only performing read operations

          Contribution[] storage contributions = _contributionsByContributor[contributor];
          uint256 numContributions = contributions.length;

          for (uint256 i = 0; i < numContributions; ++i) {
              ethContributed += contributions[i].amount;
          } 

        // Lines: [271-274]
          // Comment: party could be hashed 
              if (party != Party(payable(0))) {
                revert PartyAlreadyExistsError(party);
              }

        // Lines: [346-362]
          // Comment: use memory for contributions, since no write operations are performed 

            Contribution[] storage contributions = _contributionsByContributor[contributor];
            uint256 numContributions = contributions.length;
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

        // Lines: [391-401]
          // Comment: cache gateKeeper, gateKeeperId 
          if (gateKeeper != IGateKeeper(address(0))) {
              if (!gateKeeper.isAllowed(contributor, gateKeeperId, gateData)) {
                  revert NotAllowedByGateKeeperError(
                      contributor,
                      gateKeeper,
                      gateKeeperId,
                      gateData
                  );
              }
          }


      file: party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol 
      ..................................................................

        // Lines: [422-446]
          // Comments: there is no reason to get a ref for snaps, allocate into the memory instead
            function findVotingPowerSnapshotIndex(address voter, uint40 timestamp)
                public
                view
                returns (uint256 index)
            {
                VotingPowerSnapshot[] storage snaps = _votingPowerSnapshotsByVoter[voter];

                // Derived from Open Zeppelin binary search
                // ref: https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/Checkpoints.sol#L39
                uint256 high = snaps.length;
                uint256 low = 0;
                while (low < high) {
                    uint256 mid = (low + high) / 2;
                    if (snaps[mid].timestamp > timestamp) {
                        // Entry is too recent.
                        high = mid;
                    } else {
                        // Entry is older. This is our best guess for now.
                        low = mid + 1;
                    }
                }

                // Return `type(uint256).max` if no valid voting snapshots found.
                return high == 0 ? type(uint256).max : high - 1;
            }

        // Lines: [499-514]
          // Comments: feeRecipient/feeBps could be cached 

            if (tokenType == ITokenDistributor.TokenType.Native) {
                return distributor.createNativeDistribution { value: address(this).balance }(this, feeRecipient, feeBps);
            }
            // Otherwise must be an ERC20 token distribution.
            assert(tokenType == ITokenDistributor.TokenType.Erc20);
            IERC20(token).compatTransfer(
                address(distributor),
                IERC20(token).balanceOf(address(this))
            );
            return distributor.createErc20Distribution(
                IERC20(token),
                this,
                feeRecipient,
                feeBps
            );

        // Lines: [568-568]
          // Comments: it's possible to allocate info into the memory in order to perform reading 

            ProposalState storage info = _proposalStateByProposalId[proposalId];

        // Lines: [853-853]
          // Comments: consider allocating into the memory 

            VotingPowerSnapshot[] storage snaps = _votingPowerSnapshotsByVoter[voter];

        // Lines: [980-980]
          // Comment: Redundant memory allocation
              VotingPowerSnapshot memory lastSnap = voterSnaps[n - 1];

        // Lines: [994-998]
          // Comment: allocate into the memory 
          VotingPowerSnapshot[] storage voterSnaps = _votingPowerSnapshotsByVoter[voter];
          uint256 n = voterSnaps.length;
          if (n != 0) {
              snap = voterSnaps[n - 1];
          }


      file: party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol 
      ..................................................................

        // Lines: [149-188]
          // Comment: allocate into the memory `market` and `maximumBid` 

          function bid() external onlyDelegateCall {
              // Check that the auction is still active.
              {
                  CrowdfundLifecycle lc = getCrowdfundLifecycle();
                  if (lc != CrowdfundLifecycle.Active) {
                      revert WrongLifecycleError(lc);
                  }
              }
              // Mark as busy to prevent `burn()`, `bid()`, and `contribute()`
              // getting called because this will result in a `CrowdfundLifecycle.Busy`.
              _bidStatus = AuctionCrowdfundStatus.Busy;
              // Make sure the auction is not finalized.
              uint256 auctionId_ = auctionId;
              if (market.isFinalized(auctionId_)) {
                  revert AuctionFinalizedError(auctionId_);
              }
              // Only bid if we are not already the highest bidder.
              if (market.getCurrentHighestBidder(auctionId_) == address(this)) {
                  revert AlreadyHighestBidderError();
              }
              // Get the minimum necessary bid to be the highest bidder.
              uint96 bidAmount = market.getMinimumBid(auctionId_).safeCastUint256ToUint96();
              // Make sure the bid is less than the maximum bid.
              if (bidAmount > maximumBid) {
                  revert ExceedsMaximumBidError(bidAmount, maximumBid);
              }
              lastBid = bidAmount;
              // No need to check that we have `bidAmount` since this will attempt to
              // transfer `bidAmount` ETH to the auction platform.
              (bool s, bytes memory r) = address(market).delegatecall(abi.encodeCall(
                  IMarketWrapper.bid,
                  (auctionId_, bidAmount)
              ));
              if (!s) {
                  r.rawRevert();
              }
              emit Bid(bidAmount);

              _bidStatus = AuctionCrowdfundStatus.Active;
          }

        // Lines: [196-259]
          // Comment: `market`, `nftTokenId`, `nftContract` could be allocated into the memory

              function finalize(FixedGovernanceOpts memory governanceOpts)
                  external
                  onlyDelegateCall
                  returns (Party party_)
              {
                  // Check that the auction is still active and has not passed the `expiry` time.
                  CrowdfundLifecycle lc = getCrowdfundLifecycle();
                  if (lc != CrowdfundLifecycle.Active && lc != CrowdfundLifecycle.Expired) {
                      revert WrongLifecycleError(lc);
                  }
                  // Mark as busy to prevent burn(), bid(), and contribute()
                  // getting called because this will result in a `CrowdfundLifecycle.Busy`.
                  _bidStatus = AuctionCrowdfundStatus.Busy;

                  uint96 lastBid_ = lastBid;
                  // Only finalize on the market if we placed a bid.
                  if (lastBid_ != 0) {
                      uint256 auctionId_ = auctionId;
                      // Finalize the auction if it isn't finalized.
                      if (!market.isFinalized(auctionId_)) {
                          // Note that even if this crowdfund has expired but the auction is still
                          // ongoing, this call can fail and block finalization until the auction ends.
                          (bool s, bytes memory r) = address(market).call(abi.encodeCall(
                              IMarketWrapper.finalize,
                              auctionId_
                          ));
                          if (!s) {
                              r.rawRevert();
                          }
                      }
                  } else {
                      // If we never placed a bid, the auction must have expired.
                      if (lc != CrowdfundLifecycle.Expired) {
                          revert AuctionNotExpiredError();
                      }
                  }
                  // Are we now in possession of the NFT?
                  if (nftContract.safeOwnerOf(nftTokenId) == address(this)) {
                      if (lastBid_ == 0) {
                          // The NFT was gifted to us. Everyone who contributed wins.
                          lastBid_ = totalContributions;
                          if (lastBid_ == 0) {
                              // Nobody ever contributed. The NFT is effectively burned.
                              revert NoContributionsError();
                          }
                          lastBid = lastBid_;
                      }
                      // Create a governance party around the NFT.
                      party_ = _createParty(
                          _getPartyFactory(),
                          governanceOpts,
                          nftContract,
                          nftTokenId
                      );
                      emit Won(lastBid_, party_);
                  } else {
                      // Clear `lastBid` so `_getFinalPrice()` is 0 and people can redeem their
                      // full contributions when they burn their participation NFTs.
                      lastBid = 0;
                      emit Lost();
                  }

                  _bidStatus = AuctionCrowdfundStatus.Finalized;
              }

      file: party-contracts-c4/contracts/proposals/ProposalStorage.sol 
      .................................................................

        // Lines: [50-57]
            // Comment: use memory to return the value, calldata is also possible 
                function _getSharedProposalStorage()
                    private
                    pure
                    returns (SharedProposalStorage storage stor)
                {
                    uint256 s = SHARED_STORAGE_SLOT;
                    assembly { stor.slot := s }
                }
        ```

## **[G-07] Add `require()/reverts before some computations**<a name="G-07"></a>


### ***Description:***

- Everyting above `require()` takes some gas for execution, therefore if the statement reverts gas will not be retrieved.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: party-contracts-c4/contracts/party/PartyGovernance.sol 
      .................................................................
      
        // Lines: [674-676]
            // Comment: could be moved up(before sloads)
                if (status != ProposalStatus.Ready && status != ProposalStatus.InProgress) {
                    revert BadProposalStatusError(status);
                }

        // Lines: [690-692]
            // Comment: could be moved up
                if (!_isPreciousListCorrect(preciousTokens, preciousTokenIds)) {
                    revert BadPreciousListError();
                }

        // Lines: [734-736]
            // Comment: could be moved up
                ProposalStatus status = _getProposalStatus(values);
                if (status != ProposalStatus.InProgress) {
                    revert BadProposalStatusError(status);
                }
    ```


## **[G-10] `Internal` functions can be inlined**<a name="G-10"></a>

### ***Description:***

- It takes some extra `JUMP`s in order to fetch this data. Also, after sorting functions by their bytes4(selectors) the functions under the current will cost more to invoke. In loops it will save significant amount of gas.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol 
      .................................................................
      
        // Lines: [283-290]
            function _getFinalPrice()
                internal
                override
                view
                returns (uint256 price)
            {
                return lastBid;
            }

    ```

## **[G-12] Unused the named return variables**<a name="G-12"></a>

### ***Description:***

- Unnecessary gas usage.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol 
      .................................................................
      
        // Lines: [283-290]
            // Comment: price is unused
            function _getFinalPrice()
                internal
                override
                view
                returns (uint256 price)
            {
                return lastBid;
            }

      file: party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol 
      .................................................................
        // Lines: [98-115]
            // Comment: party_ is unused 
                function buy(
                    address payable callTarget,
                    uint96 callValue,
                    bytes calldata callData,
                    FixedGovernanceOpts memory governanceOpts
                )
                    external
                    returns (Party party_)
                {
                    return _buy(
                        nftContract,
                        nftTokenId,
                        callTarget,
                        callValue,
                        callData,
                        governanceOpts
                    );
                }

      file: party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol 
      .......................................................................
        // Lines: [113-132]
            // Comment: party_ is unused 
                function buy(
                    uint256 tokenId,
                    address payable callTarget,
                    uint96 callValue,
                    bytes calldata callData,
                    FixedGovernanceOpts memory governanceOpts
                )
                    external
                    onlyHost(governanceOpts.hosts)
                    returns (Party party_)
                {
                    return _buy(
                        nftContract,
                        tokenId,
                        callTarget,
                        callValue,
                        callData,
                        governanceOpts
                    );
                }

      file: party-contracts-c4/contracts/crowdfund/Crowdfund.sol 
      .......................................................................
        // Lines: [113-132]
            // Comment: party_ is unused 
                function _createParty(
                    IPartyFactory partyFactory,
                    FixedGovernanceOpts memory governanceOpts,
                    IERC721 preciousToken,
                    uint256 preciousTokenId
                )
                    internal
                    returns (Party party_)
                {
                    IERC721[] memory tokens = new IERC721[](1);
                    tokens[0] = preciousToken;
                    uint256[] memory tokenIds = new uint256[](1);
                    tokenIds[0] = preciousTokenId;
                    return _createParty(partyFactory, governanceOpts, tokens, tokenIds);
                }

      file: party-contracts-c4/contracts/crowdfund/CrowdfundFactory.sol 
      .......................................................................
        // Lines: [107-130]
            // Comment: newGateKeeperId is unused 
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

      file: party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol 
      .......................................................................
        // Lines: [133-135]
            // Comment: numTokens is unused 
                function balanceOf(address owner) external view returns (uint256 numTokens) {
                    return _doesTokenExistFor(owner) ? 1 : 0;
                }

      file: party-contracts-c4/contracts/distribution/TokenDistributor.sol 
      .......................................................................
        // Lines: [403-413]
            // Comment: balanceId is unused 
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

      file: party-contracts-c4/contracts/party/PartyGovernance.sol 
      .......................................................................
        // Lines: [347-353]
            // Comment: votingPower is unused 
                function getVotingPowerAt(address voter, uint40 timestamp)
                    external
                    view
                    returns (uint96 votingPower)
                {
                    return getVotingPowerAt(voter, timestamp, type(uint256).max);
                }

        // Lines: [361-368]
            // Comment: votingPower is unused 
                function getVotingPowerAt(address voter, uint40 timestamp, uint256 snapIndex)
                    public
                    view
                    returns (uint96 votingPower)
                {
                    VotingPowerSnapshot memory snap = _getVotingPowerSnapshotAt(voter, timestamp, snapIndex);
                    return (snap.isDelegated ? 0 : snap.intrinsicVotingPower) + snap.delegatedVotingPower;
                }

        // Lines: [385-387]
            // Comment: gv is unused 
               function getVotingPowerAt(address voter, uint40 timestamp, uint256 snapIndex)
                    public
                    view
                    returns (uint96 votingPower)
                {
                    VotingPowerSnapshot memory snap = _getVotingPowerSnapshotAt(voter, timestamp, snapIndex);
                    return (snap.isDelegated ? 0 : snap.intrinsicVotingPower) + snap.delegatedVotingPower;
                } 

        // Lines: [422-446]
            // Comment: index is unused 
                function findVotingPowerSnapshotIndex(address voter, uint40 timestamp)
                    public
                    view
                    returns (uint256 index)
                {
                    VotingPowerSnapshot[] storage snaps = _votingPowerSnapshotsByVoter[voter];

                    // Derived from Open Zeppelin binary search
                    // ref: https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/Checkpoints.sol#L39
                    uint256 high = snaps.length;
                    uint256 low = 0;
                    while (low < high) {
                        uint256 mid = (low + high) / 2;
                        if (snaps[mid].timestamp > timestamp) {
                            // Entry is too recent.
                            high = mid;
                        } else {
                            // Entry is older. This is our best guess for now.
                            low = mid + 1;
                        }
                    }

                    // Return `type(uint256).max` if no valid voting snapshots found.
                    return high == 0 ? type(uint256).max : high - 1;
                }

        // Lines: [483-513]
            // Comment: distInfo is unused 
                function distribute(
                    ITokenDistributor.TokenType tokenType,
                    address token,
                    uint256 tokenId
                )
                    external
                    onlyActiveMember
                    onlyDelegateCall
                    returns (ITokenDistributor.DistributionInfo memory distInfo)
                {
                    // Get the address of the token distributor.
                    ITokenDistributor distributor = ITokenDistributor(
                        _GLOBALS.getAddress(LibGlobals.GLOBAL_TOKEN_DISTRIBUTOR)
                    );
                    emit DistributionCreated(tokenType, token, tokenId);
                    // Create a native token distribution.
                    if (tokenType == ITokenDistributor.TokenType.Native) {
                        return distributor.createNativeDistribution
                            { value: address(this).balance }(this, feeRecipient, feeBps);
                    }
                    // Otherwise must be an ERC20 token distribution.
                    assert(tokenType == ITokenDistributor.TokenType.Erc20);
                    IERC20(token).compatTransfer(
                        address(distributor),
                        IERC20(token).balanceOf(address(this))
                    );
                    return distributor.createErc20Distribution(
                        IERC20(token),
                        this,
                        feeRecipient,
                        feeBps
                    );
                }

        // Lines: [562-609]
            // Comment: totalVotes is unused 
                function accept(uint256 proposalId, uint256 snapIndex)
                    public
                    onlyDelegateCall
                    returns (uint256 totalVotes)
                {
                    // Get the information about the proposal.
                    ProposalState storage info = _proposalStateByProposalId[proposalId];
                    ProposalStateValues memory values = info.values;

                    // Can only vote in certain proposal statuses.
                    {
                        ProposalStatus status = _getProposalStatus(values);
                        // Allow voting even if the proposal is passed/ready so it can
                        // potentially reach 100% consensus, which unlocks special
                        // behaviors for certain proposal types.
                        if (
                            status != ProposalStatus.Voting &&
                            status != ProposalStatus.Passed &&
                            status != ProposalStatus.Ready
                        ) {
                            revert BadProposalStatusError(status);
                        }
                    }

                    // Cannot vote twice.
                    if (info.hasVoted[msg.sender]) {
                        revert AlreadyVotedError(msg.sender);
                    }
                    // Mark the caller as having voted.
                    info.hasVoted[msg.sender] = true;

                    // Increase the total votes that have been cast on this proposal.
                    uint96 votingPower = getVotingPowerAt(msg.sender, values.proposedTime, snapIndex);
                    values.votes += votingPower;
                    info.values = values;
                    emit ProposalAccepted(proposalId, msg.sender, votingPower);

                    // Update the proposal status if it has reached the pass threshold.
                    if (values.passedTime == 0 && _areVotesPassing(
                        values.votes,
                        _governanceValues.totalVotingPower,
                        _governanceValues.passThresholdBps))
                    {
                        info.values.passedTime = uint40(block.timestamp);
                        emit ProposalPassed(proposalId);
                    }
                    return values.votes;
                }

        // Lines: [804-845]
            // Comment: completed is unused 
                function _executeProposal(
                    uint256 proposalId,
                    Proposal memory proposal,
                    IERC721[] memory preciousTokens,
                    uint256[] memory preciousTokenIds,
                    uint256 flags,
                    bytes memory progressData,
                    bytes memory extraData
                )
                    private
                    returns (bool completed)
                {
                    // Setup the arguments for the proposal execution engine.
                    IProposalExecutionEngine.ExecuteProposalParams memory executeParams =
                        IProposalExecutionEngine.ExecuteProposalParams({
                            proposalId: proposalId,
                            proposalData: proposal.proposalData,
                            progressData: progressData,
                            extraData: extraData,
                            preciousTokens: preciousTokens,
                            preciousTokenIds: preciousTokenIds,
                            flags: flags
                        });
                    // Get the progress data returned after the proposal is executed.
                    bytes memory nextProgressData;
                    {
                        // Execute the proposal.
                        (bool success, bytes memory resultData) =
                            address(_getProposalExecutionEngine()).delegatecall(abi.encodeCall(
                                IProposalExecutionEngine.executeProposal,
                                (executeParams)
                            ));
                        if (!success) {
                            resultData.rawRevert();
                        }
                        nextProgressData = abi.decode(resultData, (bytes));
                    }
                    emit ProposalExecuted(proposalId, msg.sender, nextProgressData);
                    // If the returned progress data is empty, then the proposal completed
                    // and it should not be executed again.
                    return nextProgressData.length == 0;
                }

        // Lines: [876-876]
            // Comment: redundant return 
                return snap;

        // Lines: [1012-1055]
            // Comment: status is unused 
                function _getProposalStatus(ProposalStateValues memory pv)
                    private
                    view
                    returns (ProposalStatus status)
                {
                    // Never proposed.
                    if (pv.proposedTime == 0) {
                        return ProposalStatus.Invalid;
                    }
                    // Executed at least once.
                    if (pv.executedTime != 0) {
                        if (pv.completedTime == 0) {
                            return ProposalStatus.InProgress;
                        }
                        // completedTime high bit will be set if cancelled.
                        if (pv.completedTime & UINT40_HIGH_BIT == UINT40_HIGH_BIT) {
                            return ProposalStatus.Cancelled;
                        }
                        return ProposalStatus.Complete;
                    }
                    // Vetoed.
                    if (pv.votes == uint96(int96(-1))) {
                        return ProposalStatus.Defeated;
                    }
                    uint40 t = uint40(block.timestamp);
                    GovernanceValues memory gv = _governanceValues;
                    if (pv.passedTime != 0) {
                        // Ready.
                        if (pv.passedTime + gv.executionDelay <= t) {
                            return ProposalStatus.Ready;
                        }
                        // If unanimous, we skip the execution delay.
                        if (_isUnanimousVotes(pv.votes, gv.totalVotingPower)) {
                            return ProposalStatus.Ready;
                        }
                        // Passed.
                        return ProposalStatus.Passed;
                    }
                    // Voting window expired.
                    if (pv.proposedTime + gv.voteDuration <= t) {
                        return ProposalStatus.Defeated;
                    }
                    return ProposalStatus.Voting;
                }

      file: party-contracts-c4/contracts/party/PartyGovernanceNFT.sol 
      .................................................................

        // Lines: [66-73]
            // Comment: owner is unused 
                function ownerOf(uint256 tokenId)
                    public
                    view
                    override(ERC721, ITokenDistributorParty)
                    returns (address owner)
                {
                    return ERC721.ownerOf(tokenId);
                }

      file:  party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol
      .................................................................

        // Lines: [37-42]
            // Comment: nextProgressData is unused 
                function _executeArbitraryCalls(
                    IProposalExecutionEngine.ExecuteProposalParams memory params
                )
                    internal
                    returns (bytes memory nextProgressData)
                {}

        // Lines: [142-151]
            // Comment: isAllowed is unused 
                function _isCallAllowed(
                    ArbitraryCall memory call,
                    bool isUnanimous,
                    IERC721[] memory preciousTokens,
                    uint256[] memory preciousTokenIds
                )
                    private
                    view
                    returns (bool isAllowed)
                {

      file:  party-contracts-c4/contracts/proposals/FractionalizeProposal.sol
      .................................................................

        // Lines: [39-44]
            // Comment: nextProgressData is unused 
                function _executeFractionalize(
                    IProposalExecutionEngine.ExecuteProposalParams memory params
                )
                    internal
                    returns (bytes memory nextProgressData)
                {

      file:  party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol 
      .................................................................

        // Lines: [123-128]
            // Comment: nextProgressData is unused 
                function _executeListOnOpensea(
                    IProposalExecutionEngine.ExecuteProposalParams memory params
                )
                    internal
                    returns (bytes memory nextProgressData)
                {

      file: party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol 
      .................................................................

        // Lines: [78-83]
            // Comment: nextProgressData is unused 
                function _executeListOnZora(
                    IProposalExecutionEngine.ExecuteProposalParams memory params
                )
                    internal
                    returns (bytes memory nextProgressData)
                {

        // Lines: [164-173]
            // Comment: sold is unused 
                function _settleZoraAuction(
                    uint256 auctionId,
                    uint40 minExpiry,
                    IERC721 token,
                    uint256 tokenId
                )
                    internal
                    override
                    returns (bool sold)
                {

      file: party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol 
      .................................................................

        // Lines: [107-111]
            // Comment: id is unused 
                function getCurrentInProgressProposalId()
                    external
                    view
                    returns (uint256 id)
                {

      file: party-contracts-c4/contracts/proposals/ProposalStorage.sol 
      .................................................................

        // Lines: [21-25]
            // Comment: impl is unused 
                function _getProposalExecutionEngine()
                    internal
                    view
                    returns (IProposalExecutionEngine impl)
                {

      file: party-contracts-c4/contracts/utils/LibSafeERC721.sol 
      .................................................................

        // Lines: [16-31]
            // Comment: owner is unused 
                function safeOwnerOf(IERC721 token, uint256 tokenId)
                    internal
                    view
                    returns (address owner)
                {
    ```


## Kudos for the quality of the code! It's pretty easy to explore
