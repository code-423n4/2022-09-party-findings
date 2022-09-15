## low risks
### L01: the assert() function when false, uses up all the remaining gas and reverts all the changes made.
#### problem
assert(false) compiles to 0xfe, which is an invalid opcode, using up all remaining gas, and reverting all changes.
require(false) compiles to 0xfd which is the REVERT opcode, meaning it will refund the remaining gas. The opcode can also return a value (useful for debugging), but I don't believe that is supported in Solidity as of this moment. (2017-11-21)
#### prof
crowdfund/CrowdfundNFT.sol, 166, b'        assert(false); // Will not be reached.'
proposals/FractionalizeProposal.sol, 67, b'        assert(vault.balanceOf(address(this)) == supply);'
proposals/ListOnOpenseaProposal.sol, 221, b'        assert(step == ListOnOpenseaStep.ListedOnOpenSea);'
proposals/ListOnOpenseaProposal.sol, 302, b'        assert(SEAPORT.validate(orders));'
proposals/ListOnZoraProposal.sol, 120, b'        assert(step == ZoraStep.ListedOnZora);'
party/PartyGovernance.sol, 504, b'        assert(tokenType == ITokenDistributor.TokenType.Erc20);'
party/PartyGovernanceNFT.sol, 179, b'        assert(false); // Will not be reached.'
proposals/ProposalExecutionEngine.sol, 142, b'                assert(currentInProgressProposalId == 0);'


### L02: UNUSED/EMPTY RECEIVE()/FALLBACK() FUNCTION
#### problem
If the intention is for the Ether to be used, the function should call another function, otherwise it should revert
#### prof
crowdfund/AuctionCrowdfund.sol, 144, b'    receive() external payable {}'
party/Party.sol, 47, b'    receive() external payable {}'

### L03:_SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE
#### problem
_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both OpenZeppelin and solmate have versions of this function
#### prof
crowdfund/Crowdfund.sol, 439, b'                _mint(contributor);'
party/PartyGovernanceNFT.sol, 132, b'        _mint(owner, tokenId);'


### L04: RETURN VALUES OF approve() NOT CHECKED
#### prof
approve() NOT CHECKED
Not all IERC20 implementations revert() when there’s a failure in approve(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment
#### problem
proposals/ArbitraryCallsProposal.sol, 173, b'                if (selector == IERC721.approve.selector) {'
proposals/FractionalizeProposal.sol, 53, b'        data.token.approve(address(VAULT_FACTORY), data.tokenId);'
proposals/ListOnOpenseaProposal.sol, 256, b'        token.approve(conduit, tokenId);'
proposals/ListOnOpenseaProposal.sol, 367, b'            token.approve(address(0), tokenId);'
proposals/ListOnZoraProposal.sol, 143, b'        token.approve(address(ZORA), tokenId);'

### L05:INPUT ARRAY LENGTHS MAY DIFFER
#### problem
If the caller makes a copy-paste error, the lengths may be mismatchd and an operation believed to have been completed may not in fact have been completed
#### prof
party/PartyFactory.sol, 53, b'    function createParty(\n        address authority,\n        Party.PartyOptions memory opts,\n        IERC721[] memory preciousTokens,\n        uint256[] memory preciousTokenIds\n    )\n        external\n        returns (Party party)\n    '
party/PartyGovernance.sol, 710, b'    function execute(\n        uint256 proposalId,\n        Proposal memory proposal,\n        IERC721[] memory preciousTokens,\n        uint256[] memory preciousTokenIds,\n        bytes calldata progressData,\n        bytes calldata extraData\n    )\n        external\n        payable\n        onlyActiveMember\n        onlyDelegateCall\n    '

### L06: address variable should check if it is zero MISSING CHECKS FOR ADDRESS(0X0) WHEN ASSIGNING VALUES TO ADDRESS STATE VARIABLES
#### prof
crowdfund/AuctionCrowdfund.sol, 115, b'        nftContract = opts.nftContract;'
crowdfund/AuctionCrowdfund.sol, 117, b'        market = opts.market;'
crowdfund/BuyCrowdfund.sol, 87, b'        nftContract = opts.nftContract;'
crowdfund/CollectionBuyCrowdfund.sol, 101, b'        nftContract = opts.nftContract;'
crowdfund/Crowdfund.sol, 119, b'        _GLOBALS = globals;'
crowdfund/Crowdfund.sol, 139, b'        splitRecipient = opts.splitRecipient;'
crowdfund/Crowdfund.sol, 150, b'        gateKeeper = opts.gateKeeper;'
crowdfund/Crowdfund.sol, 298, b'        party = party_ = partyFactory\n            .createParty(\n                address(this),\n                Party.PartyOptions({\n                    name: name,\n                    symbol: symbol,\n                    governance: PartyGovernance.GovernanceOpts({\n                        hosts: governanceOpts.hosts,\n                        voteDuration: governanceOpts.voteDuration,\n                        executionDelay: governanceOpts.executionDelay,\n                        passThresholdBps: governanceOpts.passThresholdBps,\n                        totalVotingPower: _getFinalPrice().safeCastUint256ToUint96(),\n                        feeBps: governanceOpts.feeBps,\n                        feeRecipient: governanceOpts.feeRecipient\n                    })\n                }),\n                preciousTokens,\n                preciousTokenIds\n            );'
crowdfund/CrowdfundFactory.sol, 26, b'        _GLOBALS = globals;'
crowdfund/CrowdfundNFT.sol, 35, b'        _GLOBALS = globals;'
proposals/FractionalizeProposal.sol, 35, b'        VAULT_FACTORY = vaultFactory;'
globals/Globals.sol, 24, b'        multiSig = multiSig_;'
globals/Globals.sol, 28, b'        multiSig = newMultiSig;'
utils/Implementation.sol, 11, b'    constructor() { IMPL = address(this); }'
proposals/ListOnOpenseaProposal.sol, 114, b'        SEAPORT = seaport;'
proposals/ListOnOpenseaProposal.sol, 115, b'        CONDUIT_CONTROLLER = conduitController;'
proposals/ListOnOpenseaProposal.sol, 116, b'        _GLOBALS = globals;'
proposals/ListOnZoraProposal.sol, 71, b'        ZORA = zoraAuctionHouse;'
proposals/ListOnZoraProposal.sol, 72, b'        _GLOBALS = globals;'
party/PartyFactory.sol, 22, b'        GLOBALS = globals;'
party/PartyGovernance.sol, 267, b'        _GLOBALS = globals;'
party/PartyGovernance.sol, 302, b'        feeRecipient = opts.feeRecipient;'
party/PartyGovernanceNFT.sol, 45, b'        _GLOBALS = globals;'
party/PartyGovernanceNFT.sol, 62, b'        mintAuthority = mintAuthority_;'
proposals/ProposalExecutionEngine.sol, 94, b'        _GLOBALS = globals;'
utils/Proxy.sol, 17, b'        IMPL = impl;'

### N01: EVENT IS MISSING INDEXED FIELDS
proposals/ArbitraryCallsProposal.sol, 35, b'    event ArbitraryCallExecuted(uint256 proposalId, uint256 idx, uint256 count);'
crowdfund/AuctionCrowdfund.sol, 73, b'    event Bid(uint256 bidAmount);'
crowdfund/AuctionCrowdfund.sol, 74, b'    event Won(uint256 bid, Party party);'
crowdfund/AuctionCrowdfund.sol, 75, b'    event Lost();'
crowdfund/BuyCrowdfundBase.sol, 52, b'    event Won(Party party, IERC721 token, uint256 tokenId, uint256 settledPrice);'
crowdfund/Crowdfund.sol, 82, b'    event Burned(address contributor, uint256 ethUsed, uint256 ethOwed, uint256 votingPower);'
crowdfund/Crowdfund.sol, 83, b'    event Contributed(address contributor, uint256 amount, address delegate, uint256 previousTotalContributions);'
crowdfund/CrowdfundFactory.sol, 16, b'    event BuyCrowdfundCreated(BuyCrowdfund crowdfund, BuyCrowdfund.BuyCrowdfundOptions opts);'
crowdfund/CrowdfundFactory.sol, 17, b'    event AuctionCrowdfundCreated(AuctionCrowdfund crowdfund, AuctionCrowdfund.AuctionCrowdfundOptions opts);'
crowdfund/CrowdfundFactory.sol, 18, b'    event CollectionBuyCrowdfundCreated(CollectionBuyCrowdfund crowdfund, CollectionBuyCrowdfund.CollectionBuyCrowdfundOptions opts);'
proposals/ListOnOpenseaProposal.sol, 70, b'    event OpenseaOrderListed(\n        IOpenseaExchange.OrderParameters orderParams,\n        bytes32 orderHash,\n        IERC721 token,\n        uint256 tokenId,\n        uint256 listPrice,\n        uint256 expiry\n    );'
proposals/ListOnOpenseaProposal.sol, 76, b'    event OpenseaOrderSold(\n        bytes32 orderHash,\n        IERC721 token,\n        uint256 tokenId,\n        uint256 listPrice\n    );'
proposals/ListOnOpenseaProposal.sol, 82, b'    event OpenseaOrderExpired(\n        bytes32 orderHash,\n        IERC721 token,\n        uint256 tokenId,\n        uint256 expiry\n    );'
proposals/ListOnZoraProposal.sol, 52, b'    event ZoraAuctionCreated(\n        uint256 auctionId,\n        IERC721 token,\n        uint256 tokenId,\n        uint256 startingPrice,\n        uint40 duration,\n        uint40 timeoutTime\n    );'
proposals/ListOnZoraProposal.sol, 53, b'    event ZoraAuctionExpired(uint256 auctionId, uint256 expiry);'
proposals/ListOnZoraProposal.sol, 54, b'    event ZoraAuctionSold(uint256 auctionId);'
proposals/ListOnZoraProposal.sol, 55, b'    event ZoraAuctionFailed(uint256 auctionId);'
party/PartyGovernance.sol, 155, b'    event Proposed(\n        uint256 proposalId,\n        address proposer,\n        Proposal proposal\n    );'
party/PartyGovernance.sol, 160, b'    event ProposalAccepted(\n        uint256 proposalId,\n        address voter,\n        uint256 weight\n    );'
party/PartyGovernance.sol, 162, b'    event PartyInitialized(GovernanceOpts opts, IERC721[] preciousTokens, uint256[] preciousTokenIds);'
party/PartyGovernance.sol, 167, b'    event DistributionCreated(ITokenDistributor.TokenType tokenType, address token, uint256 tokenId);'
party/PartyGovernance.sol, 169, b'    event HostStatusTransferred(address oldHost, address newHost);'
proposals/ProposalExecutionEngine.sol, 66, b'    event ProposalEngineImplementationUpgraded(address oldImpl, address newImpl);'


### N02: NON-LIBRARY/INTERFACE FILES SHOULD USE FIXED COMPILER VERSIONS, NOT FLOATING ONES
proposals/ArbitraryCallsProposal.sol, 2, b'pragma solidity ^0.8;'
crowdfund/AuctionCrowdfund.sol, 2, b'pragma solidity ^0.8;'
crowdfund/BuyCrowdfund.sol, 2, b'pragma solidity ^0.8;'
crowdfund/BuyCrowdfundBase.sol, 2, b'pragma solidity ^0.8;'
crowdfund/CollectionBuyCrowdfund.sol, 2, b'pragma solidity ^0.8;'