
## Summary

L-01 Unused receive() function will lock Ether in contract (2 instances)
L-02 require() should be used instead of assert() (9 instances)
L-03 Unsafe ERC20 Operations (3 instances)
L-04 Unspecific Compiler Version Pragma  (40 instances)
L-05 Unnecessary transferFrom() function (1 instance)
L-06 Duplicate safeTransferFrom() function (1 instance)

N-01 Adding a return statement when the function defines a named return variable, is redundant (1 instance)
N-02 public functions not called by the contract should be declared external instead (20 instances)
N-03 type(uint<n>).max should be used instead of uint<n>(-1) (3 instances)
N-04 constants should be defined rather than using magic numbers (8 intances)
N-05 Use a more recent version of solidity (46 instances) 
N-06 File is missing NatSpec (8 instances)
N-07 NatSpec is incomplete (2 instances)
N-08 Event is missing indexed fields (14 instances)
N-09 uint256 is assigned to zero by default, additional reassignment to zero is unnecessary (10 instances)

Total: 168 instance in 15 issues.

---

## L-01 Unused receive() function will lock Ether in contract

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert

2 instances in 2 files:

contracts/crowdfund/AuctionCrowdfund.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L144

contracts/party/Party.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L47


## L-02 require() should be used instead of assert()

9 instances in 7 files:

contracts/proposals/FractionalizeProposal.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L67

contracts/distribution/TokenDistributor.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L385
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L411

contracts/proposals/ProposalExecutionEngine.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L142

contracts/proposals/ListOnZoraProposal.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L120

contracts/party/PartyGovernanceNFT.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L179

contracts/party/PartyGovernance.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L504

contracts/proposals/ListOnOpenseaProposal.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L221
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L302


## L-03 Unsafe ERC20 Operation(s)

ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard. 
It is therefore recommended to always either use OpenZeppelin's SafeERC20 library or at least to wrap each operation in a require statement,
as seen in https://github.com/byterocket/c4-common-issues/blob/main/2-Low-Risk.md#l001---unsafe-erc20-operations

3 instances in 3 files:

contracts/utils/LibERC20Compat.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibERC20Compat.sol#L11-L31

contracts/distribution/TokenDistributor.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L386

contracts/party/PartyGovernance.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L505


## L-04 Unspecific Compiler Version Pragma

Avoid floating pragmas for non-library contracts, 
as seen in https://github.com/byterocket/c4-common-issues/blob/main/2-Low-Risk.md#l003---unspecific-compiler-version-pragma

40 instances (all files less Libraries's)

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/EIP165.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC1155Receiver.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC721Receiver.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Implementation.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributorParty.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaConduitController.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/gatekeepers/IGateKeeper.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721Receiver.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC20.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/market-wrapper/IMarketWrapper.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/IPartyFactory.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/IGlobals.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/IProposalExecutionEngine.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/FractionalV1.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/markets/IZoraAuctionHouse.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC1155.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaExchange.sol#L2

## L-05  Unnecessary transferFrom() function 

transferFrom() function is followed by safeTransferFrom() function in the contract PartyGovernanceNFT.sol
OpenZeppelin’s documentation discourages the use of transferFrom(), use safeTransferFrom() whenever possible
Given that any NFT can be used for the call option, there are a few NFTs that have logic in the onERC721Received() function, which is only triggered in the safeTransferFrom() function and not in transferFrom()

1 instance:

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L136

## L-06 Duplicate safeTransferFrom() function

safeTransferFrom() function is edited twice in the contract PartyGovernanceNFT.sol with exact same parameters, I believe an error with copy/paste.

Recommandation: Delete one version of this function.

1 instance (x2):
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L147-L155
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L158-L166

## N-01 Adding a return statement when the function defines a named return variable, is redundant

1 instance in 1 file:

contracts/party/PartyGovernance.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L876


## N-02 public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents’ functions and change the visibility from external to public. https://docs.soliditylang.org/en/latest/contracts.html#function-overriding

20 instances in 6 files:

contracts/distribution/TokenDistributor.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L139
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L191
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L250

contracts/crowdfund/AuctionCrowdfund.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L262

contracts/party/PartyGovernanceNFT.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L67
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L77
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L88
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L137
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L148
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L159

contracts/party/PartyGovernance.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L324
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L362
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L395
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L423
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L563

contracts/crowdfund/Crowdfund.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L168
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L192
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L213
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L251

contracts/crowdfund/BuyCrowdfundBase.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L150


## N-03 type(uint<n>).max should be used instead of uint<n>(-1)

Earlier versions of solidity can use uint<n>(-1) instead. Expressions not including the - 1 can often be re-written to accomodate the change (e.g. by using a > rather than a >=, which will also save some gas)

3 instances in 2 files:

contracts/distribution/TokenDistributor.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L261

contracts/party/PartyGovernance.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L190
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1033


## N-04 constants should be defined rather than using magic numbers

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L112

8 intances in 2 files:

contracts/party/PartyGovernance.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L280
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L283
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1062
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1078

contracts/crowdfund/Crowdfund.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L129
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L132
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L135
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L370
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L374


## N-05 Use a more recent version of solidity

Use a solidity version of at least 0.8.2 to get compiler automatic inlining. 
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads. 
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings. 
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value. 
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>). 
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions. 
Use a solidity version of at least 0.8.14 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>) 


46 instances (all files in scope):

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/EIP165.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC1155Receiver.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC721Receiver.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Implementation.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibAddress.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibRawResult.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibSafeERC721.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibERC20Compat.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibSafeCast.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributorParty.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaConduitController.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/gatekeepers/IGateKeeper.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721Receiver.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC20.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/market-wrapper/IMarketWrapper.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/IPartyFactory.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/IGlobals.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/IProposalExecutionEngine.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/FractionalV1.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/markets/IZoraAuctionHouse.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC1155.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaExchange.sol#L2



## N-06 File is missing NatSpec

8 instances:

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaConduitController.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC20.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/IGlobals.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/FractionalV1.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC1155.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/vendor/IOpenseaExchange.sol



## N-07 NatSpec is incomplete

2 instances

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/markets/IZoraAuctionHouse.sol


## N-08 Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields.

14 instances in 6 files:

contracts/proposals/FractionalizeProposal.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L22-L27

contracts/proposals/ArbitraryCallsProposal.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L35

contracts/proposals/ListOnZoraProposal.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L45-L51

contracts/party/PartyGovernance.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L151-L154
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L156-L159
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L162
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L165
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L167

contracts/crowdfund/Crowdfund.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L82
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L83

contracts/proposals/ListOnOpenseaProposal.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L63-L69
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L71-L75
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L77-L81
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L84-L96


## N-09 uint256 is assigned to zero by default, additional reassignment to zero is unnecessary 

https://docs.soliditylang.org/en/v0.8.13/control-structures.html#default-value

10 instances in 5 files:

contracts/proposals/LibProposal.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L14
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L32

contracts/distribution/TokenDistributor.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239

contracts/party/PartyGovernance.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L432

contracts/crowdfund/Crowdfund.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L242
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L348

contracts/proposals/ListOnOpenseaProposal.sol
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291


