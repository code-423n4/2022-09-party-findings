## FINDINGS
NB: *Some functions have been truncated where neccessary to just show affected parts of the code*

Throught the report some places might be denoted with audit tags to show the actual place affected.

## Cache storage values in memory to minimize SLOADs
The code can be optimized by minimizing the number of SLOADs.

SLOADs are expensive (100 gas after the 1st one) compared to MLOADs/MSTOREs (3 gas each). Storage values read multiple times should instead be cached in memory the first time (costing 1 SLOAD) and then read from this cache to avoid multiple SLOADs.


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L35-L40
### PartyGovernanceNFT.sol.onlyMinter(): mintAuthority should be cached(Saves 4 SLOADs in total )
```solidity
File: /contracts/party/PartyGovernanceNFT.sol
35:    modifier onlyMinter() {
36:        if (msg.sender != mintAuthority) { //@audit: mintAuthority SLOAD 1
37:            revert OnlyMintAuthorityError(msg.sender, mintAuthority); //@audit: mintAuthority SLOAD 2
38:        }
39:        _;
40:    }
```
As the above is a modifer , everytime the modifier is used in a function, we read the storage value **mintAuthority** two times inside the modifier only

The above modifer has been used two times in this contract , at [Line 126](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L126) and [Line 169](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L169), which means **4 SLOADS**

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L149-L188
### AuctionCrowdfund.sol.bid(): market should be cached(Saves 3 SLOADs)
```solidity
File: /contracts/crowdfund/AuctionCrowdfund.sol
149:    function bid() external onlyDelegateCall {

162:        if (market.isFinalized(auctionId_)) { //@audit: market SLOAD 1

166:        if (market.getCurrentHighestBidder(auctionId_) == address(this)) { //@audit: market SLOAD 2

169:        // Get the minimum necessary bid to be the highest bidder.
170:        uint96 bidAmount = market.getMinimumBid(auctionId_).safeCastUint256ToUint96(); //@audit: market SLOAD 3

178:        (bool s, bytes memory r) = address(market).delegatecall(abi.encodeCall( //@audit: market SLOAD 4
179:            IMarketWrapper.bid,
180:            (auctionId_, bidAmount)
181:        ));
		
188:    }
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L149-L188
### AuctionCrowdfund.sol.bid(): maximumBid should be cached(Saves 1 SLOAD)
```solidity
File: /contracts/crowdfund/AuctionCrowdfund.sol
149:    function bid() external onlyDelegateCall {

171:        // Make sure the bid is less than the maximum bid.
172:        if (bidAmount > maximumBid) { //@audit: maximumBid SLOAD 1
173:            revert ExceedsMaximumBidError(bidAmount, maximumBid); //@audit: maximumBid SLOAD 2
174:        }

188:    }
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L196-L259
### AuctionCrowdfund.sol.finalize(): market should be cached(Saves 1 SLOAD)
```solidity
File: /contracts/crowdfund/AuctionCrowdfund.sol
196:    function finalize(FixedGovernanceOpts memory governanceOpts)
            ...
215:            if (!market.isFinalized(auctionId_)) { //@audit: market SLOAD 1
216:                // Note that even if this crowdfund has expired but the auction is still
217:                // ongoing, this call can fail and block finalization until the auction ends.
218:                (bool s, bytes memory r) = address(market).call(abi.encodeCall( //@audit: market SLOAD 2
219:                    IMarketWrapper.finalize,
220:                    auctionId_
221:                ));
				        ...
259:    }
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L196-L259
### AuctionCrowdfund.sol.finalize(): nftTokenId and nftContract should be cached (Saves 1 SLOAD each - 2 SLOADS saved total)
```solidity
File: /contracts/crowdfund/AuctionCrowdfund.sol
196:    function finalize(FixedGovernanceOpts memory governanceOpts)

232:        // Are we now in possession of the NFT?
233:       if (nftContract.safeOwnerOf(nftTokenId) == address(this)) {//@audit: nftContract SLOAD 1,//@audit: nftTokenId SLOAD 1

243:            // Create a governance party around the NFT.
244:            party_ = _createParty(
245:                _getPartyFactory(),
246:                governanceOpts,
247:                nftContract, //@audit: nftContract SLOAD 2
248:                nftTokenId //@audit: nftTokenId SLOAD 2
249:            );
        
259:    }
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfundBase.sol#L90-L147
### BuyCrowdfundBase.sol.\_buy(): maximumPrice - second SLOAD should use cached value(Saves 1 SLOAD)
```solidity
File: /contracts/crowdfund/BuyCrowdfundBase.sol
90:    function _buy(

116:            uint96 maximumPrice_ = maximumPrice; //@audit: maximumPrice SLOAD 1
117:            if (maximumPrice_ != 0 && callValue > maximumPrice_) {
118:               revert MaximumPriceError(callValue, maximumPrice); //@audit: maximumPrice SLOAD 2(use cached value)
119:            }

147:    }
```
In the above function , as the value of `maximumPrice` is already cached as `maximumPrice_` , we should use the cached  value in the revert statement

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L262-L303
### Crowdfund.sol.\_createParty(): party should be cached (Saves 1 SLOAD)
```solidity
File: /contracts/crowdfund/Crowdfund.sol
262:    function _createParty(

271:        if (party != Party(payable(0))) { //@audit: party SLOAD 1
272:            revert PartyAlreadyExistsError(party);//@audit: party SLOAD 2
273:        }
		
303:    }
```
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L262-L303
### Crowdfund.sol.\_createParty(): governanceOptsHash should be cached (Saves 1 SLOAD)
```solidity
File: /contracts/crowdfund/Crowdfund.sol
275:            bytes16 governanceOptsHash_ = _hashFixedGovernanceOpts(governanceOpts);
276:            if (governanceOptsHash_ != governanceOptsHash) { //@audit: governanceOptsHash SLOAD 1
277:                revert InvalidGovernanceOptionsError(governanceOptsHash_, governanceOptsHash);//@audit:  SLOAD 2
278:            }
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L378-L442
### Crowdfund.sol.\_contribute(): gateKeeper and gateKeeperId should be cached(Saves 3 SLOADS in total, 2 for gateKeeper, 1 for gateKeeperId  )
```solidity
File: /contracts/crowdfund/Crowdfund.sol
378:    function _contribute(
	
392:        if (gateKeeper != IGateKeeper(address(0))) { //@audit: gateKeeper SLOAD 1
393:            if (!gateKeeper.isAllowed(contributor, gateKeeperId, gateData)) { //@audit: gateKeeper SLOAD 2 , //gateKeeperId SLOAD 1
394:                revert NotAllowedByGateKeeperError(
395:                    contributor,
396:                    gateKeeper, //@audit: gateKeeper SLOAD 3
397:                    gateKeeperId, //@audit: gateKeeperId SLOAD 2
398:                    gateData
399:                );
				
442:    }
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L444-L489
### Crowdfund.sol.\_burn(): splitRecipient should be cached(Saves 1 SLOAD)
```solidity
File: /contracts/crowdfund/Crowdfund.sol
444:    function _burn(address payable contributor, CrowdfundLifecycle lc, Party party_)
  
457:        if (contributor == splitRecipient) { //@audit: splitRecipient SLOAD 1

464:        if (splitRecipient != contributor || _doesTokenExistFor(contributor)) { //@audit: splitRecipient SLOAD 2

```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L562-L609
### PartyGovernance.sol.accept(): \_governanceValues should be cached(Saves 1 SLOAD)
```solidity
File: /contracts/party/PartyGovernance.sol
562:    function accept(uint256 proposalId, uint256 snapIndex)

600:        if (values.passedTime == 0 && _areVotesPassing(
601:            values.votes,
602:            _governanceValues.totalVotingPower, //@audit: _governanceValues SLOAD 1
603:            _governanceValues.passThresholdBps))  //@audit: _governanceValues SLOAD 2

609:    }
```

## Internal/Private functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

Affected code:
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L93-L102
```solidity
File: /contracts/proposals/ArbitraryCallsProposal.sol
93:    function _executeSingleArbitraryCall(
94:        uint256 idx,
95:        ArbitraryCall memory call,
96:        IERC721[] memory preciousTokens,
97:        uint256[] memory preciousTokenIds,
98:        bool isUnanimous,
99:        uint256 ethAvailable
100:    )
101:        private
102:    {
```
The above is only called once on [Line 63](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L63)


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L142-L151
```solidity
File: /contracts/proposals/ArbitraryCallsProposal.sol
142:    function _isCallAllowed(
143:        ArbitraryCall memory call,
144:        bool isUnanimous,
145:        IERC721[] memory preciousTokens,
146:        uint256[] memory preciousTokenIds
147:    )
148:        private
149:        view
150:        returns (bool isAllowed)
151:    {
```

The above is only called once on [Line 104](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L104)


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L203-L207
```solidity
203:    function _decodeApproveCallDataArgs(bytes memory callData)
204:       private
205:        pure
206:        returns (address operator, uint256 tokenId)
207:    {
```

The above is only called once on [Line 175](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L175)

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L221-L225
```solidity
File: /contracts/proposals/ArbitraryCallsProposal.sol
221:    function _decodeSetApprovalForAllCallDataArgs(bytes memory callData)
222:        private
223:        pure
224:        returns (address operator, bool isApproved)
225:    {
```

The above is only called once on [Line 187](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L187)

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L228-L232
```solidity
File: /contracts/proposals/ProposalExecutionEngine.sol
228:    function _extractProposalType(bytes memory proposalData)
229:        private
230:        pure
231:        returns (ProposalType proposalType, bytes memory offsetProposalData)
232:    {
```
The above is only called once on [Line 169](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L169)

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L251-L253
```solidity
File: /contracts/proposals/ProposalExecutionEngine.sol
251:    function _executeUpgradeProposalsImplementation(bytes memory proposalData)
252:        private
253:    {
```

The above is only called once on [Line 219](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L219)

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L804-L815
```solidity
File: /contracts/party/PartyGovernance.sol
804:    function _executeProposal(
805:        uint256 proposalId,
806:        Proposal memory proposal,
807:        IERC721[] memory preciousTokens,
808:        uint256[] memory preciousTokenIds,
809:        uint256 flags,
810:        bytes memory progressData,
811:        bytes memory extraData
812:    )
813:        private
814:        returns (bool completed)
815:    {
```
The above is only called once on [Line 697](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L697)

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L916
```solidity
File: /contracts/party/PartyGovernance.sol
916:    function _getTotalVotingPower() internal view returns (uint256) {
```


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L922-L930
```solidity
File: /contracts/party/PartyGovernance.sol
922:    function _rebalanceDelegates(
923:        address voter,
924:        address oldDelegate,
925:        address newDelegate,
926:        VotingPowerSnapshot memory oldSnap,
927:        VotingPowerSnapshot memory newSnap
928:    )
929:        private
930:    {
```
The above is only called once on [Line 913](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L913)

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L1001-L1010
```solidity
File: /contracts/party/PartyGovernance.sol
1000:    function _getProposalFlags(ProposalStateValues memory pv)
1001:        private
1002:        view
1003:        returns (uint256)
1004:    {
```
The above is only called once on [Line 702](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L702)

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L1069-L1080
```solidity
File: /contracts/party/PartyGovernance.sol
1069:    function _areVotesPassing(
1070:        uint96 voteCount,
1071:        uint96 totalVotingPower,
1072:        uint16 passThresholdBps
1073:    )
1074:        private
```
The above is only called once on [Line 600](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L600)

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L1082-L1092
```solidity
File: /contracts/party/PartyGovernance.sol
1082:    function _setPreciousList(
1083:        IERC721[] memory preciousTokens,
1084:        uint256[] memory preciousTokenIds
1085:    )
1086:        private
1087:    {
```

The above is only called once on [Line 304](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L304)

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L1094-L1103
```solidity
File: /contracts/party/PartyGovernance.sol
1094:    function _isPreciousListCorrect(
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L37-L91
```solidity
File: /contracts/proposals/ArbitraryCallsProposal.sol
37:    function _executeArbitraryCalls(
38:        IProposalExecutionEngine.ExecuteProposalParams memory params
39:    )
40:        internal
41:        returns (bytes memory nextProgressData)
42:    {
```

The above is called once on a child contract on [Line 217](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L217)

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L848-L877
```solidity
File: /contracts/party/PartyGovernance.sol
848:    function _getVotingPowerSnapshotAt(address voter, uint40 timestamp, uint256 hintIndex)
849:        internal
850:        view
851:        returns (VotingPowerSnapshot memory snap)
852:    {
```

The above is only called once on [Line 366](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L366)

## Multiple accesses of a mapping/array should use a local variable cache


Caching a mapping's value in a local storage or calldata variable when the value is accessed multiple times saves ~42 gas per access due to not having to perform the same offset calculation every time.
### Help the Optimizer by saving a storage variable's reference instead of repeatedly fetching it

To help the optimizer,declare a storage type variable and use it instead of repeatedly fetching the reference in a map or an array. 
As an example, instead of repeatedly calling ```someMap[someIndex]```, save its reference like this: ```SomeStruct storage someStruct = someMap[someIndex]``` and use it.

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundNFT.sol#L141-L148
### CrowdfundNFT.sol.\_mint():\_owners[tokenId] should be cached
```solidity
File: /contracts/crowdfund/CrowdfundNFT.sol
141:    function _mint(address owner) internal returns (uint256 tokenId)
142:    {
143:        tokenId = uint256(uint160(owner));
144:        if (_owners[tokenId] != owner) { //@audit: Cache _owners[tokenId] in local storage
145:            _owners[tokenId] = owner; /@audit: _owners[tokenId] should use the cached value
146:            emit Transfer(address(0), owner, tokenId);
147:        }
148:    }
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundNFT.sol#L150-L153
### CrowdfundNFT.sol.\_burn():\_owners[tokenId] should be cached
```solidity
File: /contracts/crowdfund/CrowdfundNFT.sol
150:    function _burn(address owner) internal {

152:        if (_owners[tokenId] == owner) { //@audit: _owners[tokenId] should be cached in local storage
153:            _owners[tokenId] = address(0); //@audit: _owners[tokenId] should use the cached variable to update the value
```


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L331-L369
###  TokenDistributor.sol.\_createDistribution(): \_storedBalances[balanceId] should be cached
```solidity
File: /contracts/distribution/TokenDistributor.sol
331:    function _createDistribution(CreateDistributionArgs memory args)

341:            supply = (args.currentTokenBalance - _storedBalances[balanceId]) //@audit: Initial access
342:                .safeCastUint256ToUint128();

347:            // Update stored balance.
348:            _storedBalances[balanceId] = args.currentTokenBalance; //@audit: Second access
349:        }
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L526-L549
```solidity
File: /contracts/party/PartyGovernance.sol
526:    function propose(Proposal memory proposal, uint256 latestSnapIndex)

535:            _proposalStateByProposalId[proposalId].values, //@audit: _proposalStateByProposalId[proposalId]
536:            _proposalStateByProposalId[proposalId].hash //@audit: _proposalStateByProposalId[proposalId]

549:    }
```


## Use calldata instead of memory for function parameters
When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution. Note that even if an interface defines a function as having memory arguments, it's still valid for implementation contracs to use calldata arguments instead.

If the array is passed to an internal function which passes the array to another internal function where the array is modified and therefore memory is used in the external call, it's still more gass-efficient to use calldata when the external function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one

Note that I've also flagged instances where the function is public but can be marked as external since it's not called by the contract, and cases where a constructor is involved

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L18-L24
```solidity
File: /contracts/utils/ReadOnlyDelegateCall.sol

//@audit: bytes memory callData should be declared as bytes calldata callData
18: function delegateCallAndRevert(address impl, bytes memory callData) external {
19:        // Attempt to gate to only `_readOnlyDelegateCall()` invocations.
20:        require(msg.sender == address(this));
21:        (bool s, bytes memory r) = impl.delegatecall(callData);
22:        // Revert with success status and return data.
23:        abi.encode(s, r).rawRevert();
24:    }
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyFactory.sol#L26-L34
```solidity
File: /contracts/party/PartyFactory.sol

//@audit: use calldata on opts,preciousTokens and preciousTokenIds as they are readonly in this function
26:    function createParty(
27:        address authority,
28:        Party.PartyOptions memory opts,
29:        IERC721[] memory preciousTokens,
30:        uint256[] memory preciousTokenIds
31:    )
32:        external
33:        returns (Party party)
34:    {
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L110
```solidity
File: /contracts/crowdfund/AuctionCrowdfund.sol

//@audit: opts should use calldata as it's readonly in this function
110:    function initialize(AuctionCrowdfundOptions memory opts)
111:        external
112:        payable
113:        onlyConstructor
114:    {
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L196-L200
```solidity
File: /contracts/crowdfund/AuctionCrowdfund.sol

//@audit: use calldata on governanceOpts
196:    function finalize(FixedGovernanceOpts memory governanceOpts)
197:        external
198:        onlyDelegateCall
199:        returns (Party party_)
200:    {
```


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L526-L531
```solidity
File: /contracts/party/PartyGovernance.sol

//@audit: use calldata on proposal as it's readonly in this function
526:    function propose(Proposal memory proposal, uint256 latestSnapIndex)
527:        external
528:        onlyActiveMember
529:        onlyDelegateCall
530:       returns (uint256 proposalId)
531:    {
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L653-L661
```solidity
File: /contracts/party/PartyGovernance.sol

//@audit: use calldata on proposal,preciousTokens and preciousTokenIds
653:    function execute(
654:        uint256 proposalId,
655:        Proposal memory proposal,
656:        IERC721[] memory preciousTokens,
657:        uint256[] memory preciousTokenIds,
658:        bytes calldata progressData,
659:        bytes calldata extraData
660:    )
661:        external
```
### Avoid contract existence checks by using solidity version 0.8.10 or later

Prior to 0.8.10 the compiler inserted extra code, including EXTCODESIZE (100 gas), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L744-L745
```solidity
File: /contracts/party/PartyGovernance.sol
240:            address partyDao = _GLOBALS.getAddress(LibGlobals.GLOBAL_DAO_WALLET);

250:        address partyDao = _GLOBALS.getAddress(LibGlobals.GLOBAL_DAO_WALLET);

744:            uint256 globalMaxCancelDelay =
745:                _GLOBALS.getUint256(LibGlobals.GLOBAL_PROPOSAL_MAX_CANCEL_DURATION);

```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L202-L203
```solidity
File: /contracts/proposals/ListOnOpenseaProposal.sol
202:                uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MIN_ORDER_DURATION));
203:                uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MAX_ORDER_DURATION));

254:        bytes32 conduitKey = _GLOBALS.getBytes32(LibGlobals.GLOBAL_OPENSEA_CONDUIT_KEY);
255:        (address conduit,) = CONDUIT_CONTROLLER.getConduit(conduitKey);


265:        orderParams.zone = _GLOBALS.getAddress(LibGlobals.GLOBAL_OPENSEA_ZONE);

346:        orderHash = SEAPORT.getOrderHash(orderComps);

359:        (,, uint256 totalFilled,) = SEAPORT.getOrderStatus(orderHash);
```
The above are just some of the instances that would benefit from this. 

### Use Shift Right/Left instead of Division/Multiplication

While the DIV / MUL opcode uses 5 gas, the SHR / SHL opcode only uses 3 gas. Furthermore, beware that Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting. Eventually, overflow checks are never performed for shift operations as they are done for arithmetic operations. Instead, the result is always truncated, so the calculation can be unchecked in Solidity version 0.8+
```
    Use >> 1 instead of / 2
```

[relevant source](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md/#g008---use-shift-rightleft-instead-of-divisionmultiplication-if-possible)

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L434
```solidity
File: /contracts/party/PartyGovernance.sol
434:            uint256 mid = (low + high) / 2;
```

### x += y costs more gas than x = x + y for state variables

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L411
```solidity
File: /contracts/crowdfund/Crowdfund.sol
411:            totalContributions += amount;
```

The above should be modified to 
```solidity
		totalContributions = totalContributions + amount;
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L381
```solidity
File: /contracts/distribution/TokenDistributor.sol
381:        _storedBalances[balanceId] -= amount;
```
### Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Use a larger size then downcast where needed


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfund.sol#L98-L103
```solidity
File: /contracts/crowdfund/BuyCrowdfund.sol

//@audit: uint96 callValue
98:function buy(
99:        address payable callTarget,
100:        uint96 callValue,
101:        bytes calldata callData,
102:        FixedGovernanceOpts memory governanceOpts
103:    )
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L113-L119
```solidity
File: /contracts/crowdfund/CollectionBuyCrowdfund.sol

//@audit: uint96 callValue
113:    function buy(
114:        uint256 tokenId,
115:        address payable callTarget,
116:        uint96 callValue,
117:        bytes calldata callData,
118:        FixedGovernanceOpts memory governanceOpts
119:    )
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L94-L95
```solidity
File: /contracts/proposals/ListOnZoraProposal.sol
94:                uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MIN_AUCTION_DURATION));
95:                uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_DURATION));

102:                uint40 maxTimeout = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_ZORA_MAX_AUCTION_TIMEOUT));
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L129-L138
```solidity
File: /contracts/proposals/ListOnZoraProposal.sol

//@audit: uint40 timeout,uint40 duration
129:    function _createZoraAuction(
130:        // The minimum bid.
131:        uint256 listPrice,
132:        // How long the auction must wait for the first bid.
133:        uint40 timeout,
134:        // How long the auction will run for once a bid has been placed.
135:        uint40 duration,
136:        IERC721 token,
137:        uint256 tokenId
138:    )
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L164-L169
```solidity
File: /contracts/proposals/ListOnZoraProposal.sol

//@audit: uint40 minExpiry
164:    function _settleZoraAuction(
165:        uint256 auctionId,
166:        uint40 minExpiry,
167:        IERC721 token,
168:        uint256 tokenId
169:    )
```
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L151-L153
```solidity
File: /contracts/proposals/ListOnOpenseaProposal.sol
151:                uint40 zoraTimeout =
152:                    uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_ZORA_AUCTION_TIMEOUT));
153:                uint40 zoraDuration =

202:                uint40 minDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MIN_ORDER_DURATION));
203:                uint40 maxDuration = uint40(_GLOBALS.getUint256(LibGlobals.GLOBAL_OS_MAX_ORDER_DURATION));
```
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L98-L102
```solidity
File: /contracts/distribution/TokenDistributor.sol

//@audit: uint16 feeBps
98:    function createNativeDistribution(
99:        ITokenDistributorParty party,
100:        address payable feeRecipient,
101:        uint16 feeBps
102:    )
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L118-L123
```solidity
File: /contracts/distribution/TokenDistributor.sol

//@audit: uint16 feeBps
118:    function createErc20Distribution(
119:        IERC20 token,
120:        ITokenDistributorParty party,
121:        address payable feeRecipient,
122:        uint16 feeBps
123:    )


338:         uint128 supply;

352:        uint128 fee = supply * args.feeBps / 1e4;
353:        uint128 memberSupply = supply - fee;
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfundBase.sol#L90-L97
```solidity
File: /contracts/crowdfund/BuyCrowdfundBase.sol

//@audit: uint96 callValue
90:    function _buy(
91:        IERC721 token,
92:        uint256 tokenId,
93:        address payable callTarget,
94:        uint96 callValue,
95:        bytes calldata callData,
96:        FixedGovernanceOpts memory governanceOpts
97:    )


114:        uint96 settledPrice_;

116:        uint96 maximumPrice_ = maximumPrice;
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L378-L384
```solidity
File: /contracts/crowdfund/Crowdfund.sol

//@audit: uint96 amount,uint96 previousTotalContributions
378:    function _contribute(
379:        address contributor,
380:        uint96 amount,
381:        address delegate,
382:        uint96 previousTotalContributions,
383:        bytes memory gateData
384:    )
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L347
```solidity
File: /contracts/party/PartyGovernance.sol

//@audit: uint40 timestamp
347:     function getVotingPowerAt(address voter, uint40 timestamp)

//@audit: uint40 timestamp
361:    function getVotingPowerAt(address voter, uint40 timestamp, uint256 snapIndex)

//@audit: uint40 timestamp
422:    function findVotingPowerSnapshotIndex(address voter, uint40 timestamp)

//@audit: uint40 timestamp
848:    function _getVotingPowerSnapshotAt(address voter, uint40 timestamp, uint256 hintIndex)

//@audit: uint96 totalVotes, uint96 totalVotingPower
1057:    function _isUnanimousVotes(uint96 totalVotes, uint96 totalVotingPower)

```


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L1069-L1080
```solidity
File: contracts/party/PartyGovernance.sol

//@audit: uint96 voteCount,uint96 totalVotingPower,uint16 passThresholdBps
1069:    function _areVotesPassing(
1070:        uint96 voteCount,
1071:        uint96 totalVotingPower,
1072:        uint16 passThresholdBps
1073:    )
```

### Using unchecked blocks to save gas
Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block
[see resource](https://github.com/ethereum/solidity/issues/10695)
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L170
```solidity
File: /contracts/distribution/TokenDistributor.sol
170:        state.remainingMemberSupply = remainingMemberSupply - amountClaimed;
```

The above operation cannot underflow due to the check on [Line 167](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L167-L169) that ensures that `remainingMemberSupply` is greater than `amountClaimed`

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L424
```solidity
File: /contracts/crowdfund/Crowdfund.sol
424:                Contribution memory lastContribution = contributions[numContributions - 1];

428:                    contributions[numContributions - 1] = lastContribution;
```

The operation `numContributions - 1` cannot underflow due to the check on [Line 423](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L423) that ensures that `numContributions` is greater or equal to `1`


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L445
```solidity
File: /contracts/party/PartyGovernance.sol
445:        return high == 0 ? type(uint256).max : high - 1;
```
The above cannot underflow due to the check on `high == 0?` which is just a shorthand that will ensure that `high - 1` would only be evaluated if `high > 0`


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L862
```solidity
File: /contracts/party/PartyGovernance.sol
862:                (hintIndex == snapsLength - 1 || snaps[hintIndex+1].timestamp > timestamp)
```

The operation `snapsLength - 1` cannot underflow due to the check on [Line 855](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L855) that ensures that `snapsLength` is greater or equal to 1

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L980-L982
```solidity
File: /contracts/party/PartyGovernance.sol
979:         if (n != 0) {
980:            VotingPowerSnapshot memory lastSnap = voterSnaps[n - 1];
981:            if (lastSnap.timestamp == snap.timestamp) {
982:                voterSnaps[n - 1] = snap;
```

The operation `n - 1` cannot unerflow due to the check on [Line 979](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L979) that ensures that `n` is greater than or equal to 1 before perfoming the subtraction arithmetic

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L996-L998
```solidity
File: /contracts/party/PartyGovernance.sol
996:        if (n != 0) {
997:            snap = voterSnaps[n - 1];
998:        }
```
The operation `n - 1` cannot unerflow due to the check on [Line 996](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L996) that ensures that `n` is greater than or equal to 1 before perfoming the subtraction arithmetic

### Using unchecked blocks to save gas - Increments in for loop can be unchecked  ( save 30-40 gas per loop iteration)
The majority of Solidity for loops increment a uint256 variable that starts at 0. These increment operations never need to be checked for over/underflow because the variable will never reach the max number of uint256 (will run out of gas long before that happens). The default over/underflow check wastes gas in every iteration of virtually every for loop 


**Affected code**

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62-L67
```solidity
File: /contracts/crowdfund/CollectionBuyCrowdfund.sol
62:        for (uint256 i; i < hosts.length; i++) {
63:            if (hosts[i] == msg.sender) {
64:                isHost = true;
65:                break;
66:            }
67:        }
```

The above should be modified to:
```solidity
        for (uint256 i; i < hosts.length;) {
            if (hosts[i] == msg.sender) {
                isHost = true;
                break;
            }
		unchecked {
			++i;
		}
        }
```
**Other Instances to modify**
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291
```solidity
File: /contracts/proposals/ListOnOpenseaProposal.sol
291:            for (uint256 i = 0; i < fees.length; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14
```solidity
File: /contracts/proposals/LibProposal.sol
14:        for (uint256 i = 0; i < preciousTokens.length; ++i) {

32:        for (uint256 i = 0; i < preciousTokens.length; ++i) {

```
[see resource](https://github.com/ethereum/solidity/issues/10695)


### Cache the length of arrays in loops (saves ~6 gas per iteration)
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.

The solidity compiler will always read the length of the array during each iteration. That is,

   1.if it is a storage array, this is an extra sload operation (100 additional extra gas (EIP-2929 2) for each iteration except for the first),
   2.if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first),
   3.if it is a calldata array, this is an extra calldataload operation (3 additional gas for each iteration except for the first)

This extra costs can be avoided by caching the array length (in stack):
 When reading the length of an array,  **sload** or **mload** or **calldataload** operation is only called once and subsequently replaced by a cheap **dupN** instruction. Even though mload , calldataload and dupN have the same gas cost, mload and calldataload needs an additional dupN to put the offset in the stack, i.e., an extra 3 gas. which brings this to 6 gas
 
Here, I suggest storing the array’s length in a variable before the for-loop, and use it instead:

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62
```solidity
File: /contracts/crowdfund/CollectionBuyCrowdfund.sol

62: for (uint256 i; i < hosts.length; i++) {

```
 
 **The above should be modified to**

```solidity
 uint256 length = hosts.length;
 for (uint256 i; i < length; i++) {
```

**Other instances to modify**
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L50-L52
```solidity
File: /contracts/proposals/ArbitraryCallsProposal.sol
50:        bool[] memory hadPreciouses = new bool[](params.preciousTokenIds.length);
51:        if (!isUnanimous) {
52:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61
```solidity
File: /contracts/proposals/ArbitraryCallsProposal.sol
61:        for (uint256 i = 0; i < calls.length; ++i) {

78:            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L230
```solidity
File: /contracts/distribution/TokenDistributor.sol
230:        for (uint256 i = 0; i < infos.length; ++i) {

239:        for (uint256 i = 0; i < infos.length; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L180
```solidity
File: /contracts/crowdfund/Crowdfund.sol
180:        for (uint256 i = 0; i < contributors.length; ++i) {

300:        for (uint256 i = 0; i < preciousTokens.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306
```solidity
File: /contracts/party/PartyGovernance.sol
306:        for (uint256 i=0; i < opts.hosts.length; ++i) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291
```solidity
File: /contracts/proposals/ListOnOpenseaProposal.sol
291:            for (uint256 i = 0; i < fees.length; ++i) {
````

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14
```solidity
File: /contracts/proposals/LibProposal.sol
14:        for (uint256 i = 0; i < preciousTokens.length; ++i) {

32:        for (uint256 i = 0; i < preciousTokens.length; ++i) {

```
### ++i costs less gas compared to i++ or i += 1  (~5 gas per iteration)

++i costs less gas compared to i++ or i += 1 for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.

i++ increments i and returns the initial value of i. Which means:

```solidity
uint i = 1;  
i++; // == 1 but i == 2  
```

But ++i returns the actual incremented value:

```solidity
uint i = 1;  
++i; // == 2 and i == 2 too, so no need for a temporary variable  
```

In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2

Instances include:

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62
```solidity
File: /contracts/crowdfund/CollectionBuyCrowdfund.sol

62: for (uint256 i; i < hosts.length; i++) {

```

### Using bools for storage incurs overhead
```
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
```

See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27) 
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

**Instances affected include**
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L12
```solidity
File: /contracts/globals/Globals.sol
12:    mapping(uint256 => mapping(bytes32 => bool)) private _includedWordValues;
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L62
```solidity
File: /contracts/distribution/TokenDistributor.sol
62:    bool public emergencyActionsDisabled;
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L106
```solidity
File: /contracts/crowdfund/Crowdfund.sol
106:    bool private _splitRecipientHasBurned;
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L197
```solidity
File: /contracts/party/PartyGovernance.sol
197:    bool public emergencyExecuteDisabled;

207:    mapping(address => bool) public isHost;
```
### A modifier used only once and not being inherited should be inlined to save gas
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L238-L246
```solidity
File: /contracts/party/PartyGovernance.sol
238:    modifier onlyPartyDao() {
239:        {
240:            address partyDao = _GLOBALS.getAddress(LibGlobals.GLOBAL_DAO_WALLET);
241:            if (msg.sender != partyDao) {
242:                revert OnlyPartyDaoError(msg.sender, partyDao);
243:            }
244:        }
245:        _;
246:    }
```
The above is only used on [Line 790](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L790)

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L249-L255
```solidity
File: /contracts/party/PartyGovernance.sol
249:    modifier onlyPartyDaoOrHost() {
250:        address partyDao = _GLOBALS.getAddress(LibGlobals.GLOBAL_DAO_WALLET);
251:        if (msg.sender != partyDao && !isHost[msg.sender]) {
252:            revert OnlyPartyDaoOrHostError(msg.sender, partyDao);
253:        }
254:        _;
255:    }
```

The above modifier is only used on [Line 800](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L800)

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L258-L263
```solidity
File: /contracts/party/PartyGovernance.sol
258:    modifier onlyWhenEmergencyExecuteAllowed() {
259:        if (emergencyExecuteDisabled) {
260:            revert OnlyWhenEmergencyActionsAllowedError();
261:        }
262:        _;
263:    }
```

The above is only used on [Line 791](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L791)

### abi.encode() is less efficient than abi.encodePacked()
Changing abi.encode function to abi.encodePacked can save gas since the abi.encode function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, abi.encodePacked is more gas-efficient [see Solidity-Encode-Gas-Comparison](https://github.com/ConnorBlockchain/Solidity-Encode-Gas-Comparison).

Consider using abi.encodePacked() here:

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L23
```solidity
File: /contracts/utils/ReadOnlyDelegateCall.sol
23:        abi.encode(s, r).rawRevert();
```

### Use a more recent version of solidity

Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

The entire codebase seems to be using  the following
```solidity
  pragma solidity ^0.8;
```
