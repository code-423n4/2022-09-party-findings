## QA

### Missing checks for address(0x0) when assigning values to address state variables
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L24
```solidity
File: /contracts/globals/Globals.sol
24:        multiSig = multiSig_;

28:        multiSig = newMultiSig;

```

### Low-level call return value not checked
The call() functions return a boolean indicating if the call succeeded or failed. Thus these functions have a simple caveat, in that the transaction that executes these functions will not revert if the external call (initialised by call() or send()) fails, rather the call() or send() will simply return false. 

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L783-L796
```solidity
File: /contracts/party/PartyGovernance.sol
783:    function emergencyExecute(
784:        address targetAddress,
785:        bytes calldata targetCallData,
786:        uint256 amountEth
787:    )
788:        external
789:        payable
790:        onlyPartyDao
791:        onlyWhenEmergencyExecuteAllowed
792:        onlyDelegateCall
793:        returns (bool success)
794:    {
795:        (success, ) = targetAddress.call{value: amountEth}(targetCallData);
796:    }
```

In the above, a return value is defined but the status are not checked.

### Recommendation
Always check the return value of low  level calls 

### Using transferFrom on ERC721 tokens
The cases where ERC-721 tokens are transferred are always using transferFrom and not safeTransferFrom. This means users need to be aware their tokens may get locked up in an unprepared contract recipient.

Should a ERC-721 compatible token be transferred to an unprepared contract, it would end up being locked up there. 

From [openzeppelin.](https://docs.openzeppelin.com/contracts/4.x/api/token/erc721#IERC721-transferFrom-address-address-uint256-)
 	>> Note that the caller is responsible to confirm that the recipient is capable of receiving ERC721 or else they may be permanently lost. Usage of safeTransferFrom prevents loss, though the caller must understand this adds an external call which potentially creates a reentrancy vulnerability.

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L301
```solidity
File: /contracts/crowdfund/Crowdfund.sol
301:            preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
```

### Recommendation
Recommend consider changing transferFrom to safeTransferFrom . 

### constants should be defined rather than using magic numbers
There are several occurrences of literal values with unexplained meaning .Literal values in the codebase without an explained meaning make the code harder to read, understand and maintain, thus hindering the experience of developers, auditors and external contributors alike.

Developers should define a constant variable for every magic value used , giving it a clear and self-explanatory name. 

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L112
```solidity
File: /contracts/party/PartyGovernanceNFT.sol

//@audit: 1e18
112:        return votingPowerByTokenId[tokenId] * 1e18 / _getTotalVotingPower();
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L208
```solidity
File: /contracts/proposals/ArbitraryCallsProposal.sol

//@audit: 68
208:        if (callData.length < 68) {

226:        if (callData.length < 68) {
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L261
```solidity
File: /contracts/distribution/TokenDistributor.sol

//@audit: 1e18
261:                + (1e18 - 1)

//@audit: 1e18
263:            / 1e18

//@audit: 1e4
335:        if (args.feeBps > 1e4) {

//@audit: 1e4
352:        uint128 fee = supply * args.feeBps / 1e4;
```
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L129
```solidity
File: /contracts/crowdfund/Crowdfund.sol

//@audit: 1e4
129:        if (opts.governanceOpts.feeBps > 1e4) {

//@audit: 1e4
132:        if (opts.governanceOpts.passThresholdBps > 1e4) {

//@audit: 1e4
135:        if (opts.splitBps > 1e4) {

//@audit: 1e4
370:        votingPower = ((1e4 - splitBps_) * ethUsed) / 1e4;

//@audit: 1e4
374:            votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up
```
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L280
```solidity
File: /contracts/party/PartyGovernance.sol

//@audit:  1e4
280:        if (opts.feeBps > 1e4) {

//@audit:  1e4
283:        if (opts.passThresholdBps > 1e4) {

//@audit:  1e4
1062:        uint256 acceptanceRatio = (totalVotes * 1e4) / totalVotingPower;

//@audit: 0.9999e4
1066:        return acceptanceRatio >= 0.9999e4;

//@audit:  1e4
1078:          return uint256(voteCount) * 1e4
```


###  \_safeMint() should be used rather than \_mint() wherever possible

\_mint() is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of \_safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both open [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/Rari-Capital/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function so that NFTs aren't lost if they're minted to contracts that cannot transfer them back out.

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L132
```solidity
File: /contracts/party/PartyGovernanceNFT.sol
132:        _mint(owner, tokenId);
```

### Non assembly method exists
 ```assembly { size := extcodesize() }``` => ```uint256 size = address().code.length```
We can rewrite the code without the need for assembly here . 

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/Implementation.sol#L22-L29
```solidity
File: /contracts/utils/Implementation.sol
22:    modifier onlyConstructor() {
23:        uint256 codeSize;
24:        assembly { codeSize := extcodesize(address()) }
25:        if (codeSize != 0) {
26:            revert OnlyConstructorError();
27:        }
28:        _;
29:    }
```

### public functions not called by the contract should be declared external instead
Contracts are allowed to override their parents' functions and change the visibility from external to public.

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundFactory.sol#L35-L53
```solidity
File: /contracts/crowdfund/CrowdfundFactory.sol
35:    function createBuyCrowdfund(
36:        BuyCrowdfund.BuyCrowdfundOptions memory opts,
37:        bytes memory createGateCallData
38:    )
39:        public
40:        payable
41:        returns (BuyCrowdfund inst)
42:    {
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundFactory.sol#L61-L79
```solidity
File: /contracts/crowdfund/CrowdfundFactory.sol
61:    function createAuctionCrowdfund(
62:        AuctionCrowdfund.AuctionCrowdfundOptions memory opts,
63:        bytes memory createGateCallData
64:    )
65:        public

```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundFactory.sol#L87-L105
```solidity
File: /contracts/crowdfund/CrowdfundFactory.sol
87:    function createCollectionBuyCrowdfund(
88:        CollectionBuyCrowdfund.CollectionBuyCrowdfundOptions memory opts,
89:        bytes memory createGateCallData
90:    )
91:        public
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L167-L169
```solidity
File: /contracts/crowdfund/Crowdfund.sol
167:    function burn(address payable contributor)
168:        public
169:    {


191:    function contribute(address delegate, bytes memory gateData)
192:        public
```
### Lock pragmas to specific compiler version
Contracts should be deployed with the same compiler version and flags that they have been tested the most with. Locking the pragma helps ensure that contracts do not accidentally get deployed using, for example, the latest compiler which may have higher risks of undiscovered bugs. Contracts may also be deployed by others and the pragma indicates the compiler version intended by the original authors.

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/EIP165.sol#L2
```solidity
File: /contracts/utils/EIP165.sol
2:		pragma solidity ^0.8;
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/tokens/ERC1155Receiver.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/tokens/ERC721Receiver.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/Proxy.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/Party.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyFactory.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalStorage.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/FractionalizeProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfund.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundFactory.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundNFT.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/Implementation.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfundBase.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L2


### Missing/Incomplete Natspec
Solidity contracts can use a special form of comments to provide rich documentation for functions, return variables and more.It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). [see official docs](https://docs.soliditylang.org/en/v0.8.13/style-guide.html#natspec)


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L18
```solidity
File: /contracts/utils/ReadOnlyDelegateCall.sol
o
//@audit: Missing @param impl, @param callData
18:    function delegateCallAndRevert(address impl, bytes memory callData) external {
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyFactory.sol#L26-L34
```solidity
File: /contracts/party/PartyFactory.sol

//@audit: Missing @param authority,@param opts,@param preciousTokens,@param preciousTokenIds,@return party
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

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundFactory.sol#L29-L41
```solidity
File: /contracts/crowdfund/CrowdfundFactory.sol

//@audit: Missing @return inst
35:    function createBuyCrowdfund(
36:        BuyCrowdfund.BuyCrowdfundOptions memory opts,
37:        bytes memory createGateCallData
38:    )
39:        public
40:        payable
41:        returns (BuyCrowdfund inst)
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundFactory.sol#L61-L67
```solidity
File: /contracts/crowdfund/CrowdfundFactory.sol
//@audit: Missing @return inst
61:    function createAuctionCrowdfund(
62:        AuctionCrowdfund.AuctionCrowdfundOptions memory opts,
63:        bytes memory createGateCallData
64:    )
65:        public
66:        payable
67:        returns (AuctionCrowdfund inst)


//@audit: Missing @return inst
87:     function createCollectionBuyCrowdfund(
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L101-L105
```solidity
File: /contracts/party/PartyGovernanceNFT.sol

//@audit: Missing @params, @returns
101:    function royaltyInfo(uint256, uint256)
102:        external
103:        view
104:        returns (address, uint256)
    {
	
//@audit: Missing @param tokenId,@return	
111:    function getDistributionShareOf(uint256 tokenId) external view returns (uint256) {	
```


### File does not contain any Natspec
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L1-L82

### Missing Friendly Revert strings
 require()/revert() statements should have descriptive reason strings

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L20
```solidity
File: /contracts/utils/ReadOnlyDelegateCall.sol
20:        require(msg.sender == address(this));
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L246-L247
```solidity
File: /contracts/proposals/ProposalExecutionEngine.sol
246:        require(proposalType != ProposalType.Invalid);
247:        require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));
```

### Named return not used anywhere
Using both named returns and a return statement isn’t necessary in  a function.To  improve code quality, consider using only one of those.

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/FractionalizeProposal.sol#L39-L43
```solidity
File: /contracts/proposals/FractionalizeProposal.sol

//@audit: nextProgressData has not been used anywhere in this function
39:    function _executeFractionalize(
40:        IProposalExecutionEngine.ExecuteProposalParams memory params
41:    )
42:        internal
43:        returns (bytes memory nextProgressData
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundFactory.sol#L107-L113
```solidity
File: /contracts/crowdfund/CrowdfundFactory.sol

//@audit: newGateKeeperId has not been used anywhere in this function
107:    function _prepareGate(
108:        IGateKeeper gateKeeper,
109:        bytes12 gateKeeperId,
110:        bytes memory createGateCallData
111:    )
112:        private
113:        returns (bytes12 newGateKeeperId)
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundNFT.sol#L133-L135
```solidity
File: /contracts/crowdfund/CrowdfundNFT.sol

//@audit: numTokens has not been used anywhere in this function
133:    function balanceOf(address owner) external view returns (uint256 numTokens) {
134:        return _doesTokenExistFor(owner) ? 1 : 0;
135:    }
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L164-L172
```solidity
File: /contracts/proposals/ListOnZoraProposal.sol

//@audit: sold not used anywhere in this function
164:    function _settleZoraAuction(
        ....
169:    )
170:        internal
171:        override
172:        returns (bool sold)
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L37-L42
```solidity
File: /contracts/proposals/ArbitraryCallsProposal.sol

//@audit: nextProgressData not used in this function
37:    function _executeArbitraryCalls(
38:        IProposalExecutionEngine.ExecuteProposalParams memory params
39:    )
40:        internal
41:        returns (bytes memory nextProgressData)
42:    {
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L347-L353
```solidity
File: /contracts/party/PartyGovernance.sol

//@audit: votingPower 
347:    function getVotingPowerAt(address voter, uint40 timestamp)
348:        external
349:        view
350:        returns (uint96 votingPower)
351:    {
352:        return getVotingPowerAt(voter, timestamp, type(uint256).max);
353:    }
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L361-L368
```solidity
File: /contracts/party/PartyGovernance.sol

//@audit: votingPower not used 
361:    function getVotingPowerAt(address voter, uint40 timestamp, uint256 snapIndex)
362:        public
363:        view
364:        returns (uint96 votingPower)
365:    {
366:        VotingPowerSnapshot memory snap = _getVotingPowerSnapshotAt(voter, timestamp, snapIndex);
367:        return (snap.isDelegated ? 0 : snap.intrinsicVotingPower) + snap.delegatedVotingPower;
368:    }
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L385-L387
```solidity
File: /contracts/party/PartyGovernance.sol

//@audit: named gv returned _governanceValues
385:    function getGovernanceValues() external view returns (GovernanceValues memory gv) {
386:        return _governanceValues;
387:    }
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L562-L565
```solidity
File: /contracts/party/PartyGovernance.sol

//@audit: named totalVotes returned values.votes
562:    function accept(uint256 proposalId, uint256 snapIndex)
563:        public
564:       onlyDelegateCall
565:        returns (uint256 totalVotes)
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L804-L845
```solidity
File: /contracts/party/PartyGovernance.sol

//@audit: named completed, returned nextProgressData.length == 0;
804:    function _executeProposal(

812:    )
813:        private
814:        returns (bool completed)
815:    {

844:        return nextProgressData.length == 0;
845:    }
```

### Typos/Grammer
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L60
```solidity
File: /contracts/proposals/ListOnZoraProposal.sol
//@audit: exit instead of exist
60:    // keccak256(abi.encodeWithSignature('Error(string)', "Auction doesn't exit"))
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundFactory.sol#L29
```solidity
File: /contracts/crowdfund/CrowdfundFactory.sol
//@audit: purchases instead of purchase
29:    /// @notice Create a new crowdfund to purchases a specific NFT (i.e., with a

//@audit: purchases instead of purchase
81:    /// @notice Create a new crowdfund to purchases any NFT from a collection
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L13
```solidity
File: /contracts/utils/ReadOnlyDelegateCall.sol

//@audit: performs instead of perform
13:   // Inherited by contracts to performs read-only delegate calls.
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L896-L897
```solidity
File: /contracts/party/PartyGovernance.sol

//@audit: set the it to themself ? set it to themself?, maybe
896:        // If `oldDelegate` is zero, `voter` never delegated, set the it to
897:        // themself.
```


### Lack of event emission after critical initialize() functions
Events help non-contract tools to track changes, and events prevent users from being surprised by changes

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L124-L152
```solidity
File: /contracts/crowdfund/Crowdfund.sol
124:    function _initialize(CrowdfundOptions memory opts)
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfundBase.sol#L70-L86
```solidity
File: /contracts/crowdfund/BuyCrowdfundBase.sol
70:    function _initialize(BuyCrowdfundBaseOptions memory opts)
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L110-L141
```solidity
File: /contracts/crowdfund/AuctionCrowdfund.sol
110:    function initialize(AuctionCrowdfundOptions memory opts)
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L49-L63
```solidity
File: /contracts/party/PartyGovernanceNFT.sol
49:    function _initialize(
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/Party.sol#L33-L45
```solidity
File: /contracts/party/Party.sol
33:    function initialize(PartyInitData memory initData)
```
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfund.sol#L68-L88
```solidity
File: /contracts/crowdfund/BuyCrowdfund.sol
68:    function initialize(BuyCrowdfundOptions memory opts)
```
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L83-L102
```solidity
File: /contracts/crowdfund/CollectionBuyCrowdfund.sol
83:    function initialize(CollectionBuyCrowdfundOptions memory opts)
```
### Events are missing indexed values
Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L82-L83
```solidity
File: /contracts/crowdfund/Crowdfund.sol
82:    event Burned(address contributor, uint256 ethUsed, uint256 ethOwed, uint256 votingPower);
83:    event Contributed(address contributor, uint256 amount, address delegate, uint256 previousTotalContributions);
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundFactory.sol#L16-L18
```solidity
File: /contracts/crowdfund/CrowdfundFactory.sol
16:    event BuyCrowdfundCreated(BuyCrowdfund crowdfund, BuyCrowdfund.BuyCrowdfundOptions opts);
17:    event AuctionCrowdfundCreated(AuctionCrowdfund crowdfund, AuctionCrowdfund.AuctionCrowdfundOptions opts);
18:    event CollectionBuyCrowdfundCreated(CollectionBuyCrowdfund crowdfund,CollectionBuyCrowdfund.CollectionBuyCrowdfundOptions opts);
```


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L45-L55
```solidity
File: /contracts/proposals/ListOnZoraProposal.sol
45:    event ZoraAuctionCreated(
46:        uint256 auctionId,
47:        IERC721 token,
48:        uint256 tokenId,
49:        uint256 startingPrice,
50:        uint40 duration,
51:        uint40 timeoutTime
52:    );
53:    event ZoraAuctionExpired(uint256 auctionId, uint256 expiry);
54:    event ZoraAuctionSold(uint256 auctionId);
55:    event ZoraAuctionFailed(uint256 auctionId);
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L35
```solidity
File: /contracts/proposals/ArbitraryCallsProposal.sol
35:    event ArbitraryCallExecuted(uint256 proposalId, uint256 idx, uint256 count);
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L66
```solidity
File: /contracts/proposals/ProposalExecutionEngine.sol
66:    event ProposalEngineImplementationUpgraded(address oldImpl, address newImpl);
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/BuyCrowdfundBase.sol#L52
```solidity
File: /contracts/crowdfund/BuyCrowdfundBase.sol
52:    event Won(Party party, IERC721 token, uint256 tokenId, uint256 settledPrice);
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L63-L97
```solidity
File: /contracts/proposals/ListOnOpenseaProposal.sol
    event OpenseaOrderListed(
        IOpenseaExchange.OrderParameters orderParams,
        bytes32 orderHash,
        IERC721 token,
        uint256 tokenId,
        uint256 listPrice,
        uint256 expiry
    );
    event OpenseaOrderSold(
        bytes32 orderHash,
        IERC721 token,
        uint256 tokenId,
        uint256 listPrice
    );
    event OpenseaOrderExpired(
        bytes32 orderHash,
        IERC721 token,
        uint256 tokenId,
        uint256 expiry
    );
    // Coordinated event w/OS team to track on-chain orders.
    event OrderValidated(
        bytes32 orderHash,
        address indexed offerer,
        address indexed zone,
        IOpenseaExchange.OfferItem[] offer,
        IOpenseaExchange.ConsiderationItem[] consideration,
        IOpenseaExchange.OrderType orderType,
        uint256 startTime,
        uint256 endTime,
        bytes32 zoneHash,
        uint256 salt,
        bytes32 conduitKey,
        uint256 counter
    );
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L151-L160
```solidity
File: /contracts/party/PartyGovernance.sol
151:    event Proposed(
152:        uint256 proposalId,
153:        address proposer,
154:        Proposal proposal
155:    );
156:    event ProposalAccepted(
157:        uint256 proposalId,
158:        address voter,
159:        uint256 weight
160:    );
```
### Use a more recent version of solidity

Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

The entire codebase seems to be using  the following
```solidity
  pragma solidity ^0.8;
```

### A modifier used only once and not being inherited should be inlined 
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
