# LOW

## 1. `_safeMint()` should be used rather than `_mint()` wherever possible


`_mint()` is discouraged (https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`. Both OpenZeppelin and solmate have versions of this function

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol
```
439			_mint(contributor);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol
```
132			_mint(owner, tokenId);
```

# NON-CRITICAL

## 1. Floading Pragma version


In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

Proof of Concept
https://swcregistry.io/docs/SWC-103

```
All contracts
```

## 2. Use a more recent version of Solidity


- Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions
- Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>) Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)

```
All contracts
```

## 3. NatSpec is incomplete


All contracts are missing @author, @title messages. Some contracts do not have any NatSpec messages at all.

## 4. Event is missing `indexed`fields


Each `event` should use three `indexed` fields if there are three or more fields.

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol
```
22	event FractionalV1VaultCreated(
23		IERC721 indexed token
24		uint256 indexed tokenId,
25		uint256 vaultId,
26		IERC20 vault,
27		uint256 listPrice
28		);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol
```
45		event ZoraAuctionCreated(
46			uint256 auctionId,
47			IERC721 token,
48			uint256 tokenId,
49			uint256 startingPrice,
50			uint40 duration,
51			uint40 timeoutTime
52		);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol
```
35		event ArbitraryCallExecuted(uint256  proposalId, uint256  idx, uint256  count);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol
```
52		event Won(Party party, IERC721  token, uint256  tokenId, uint256  settledPrice);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol
```
63		event OpenseaOrderListed(
64			IOpenseaExchange.OrderParameters orderParams,
65			bytes32 orderHash,
66			IERC721 token,
67			uint256 tokenId,
68			uint256 listPrice,
69			uint256 expiry
70		);
71		event OpenseaOrderSold(
72			bytes32 orderHash,
73			IERC721 token,
74			uint256 tokenId,
75			uint256 listPrice
76		);
77		event OpenseaOrderExpired(
78			bytes32 orderHash,
79			IERC721 token,
80			uint256 tokenId,
81			uint256 expiry
82		);
83		// Coordinated event w/OS team to track on-chain orders.
84		event OrderValidated(
85			bytes32 orderHash,
86			address indexed offerer,
87			address indexed zone,
88			IOpenseaExchange.OfferItem[] offer,
89			IOpenseaExchange.ConsiderationItem[] consideration,
90			IOpenseaExchange.OrderType orderType,
91			uint256 startTime,
92			uint256 endTime,
93			bytes32 zoneHash,
94			uint256 salt,
95			bytes32 conduitKey,
96			uint256 counter
97		);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol
```
82		event Burned(address contributor, uint256 ethUsed, uint256 ethOwed, uint256 votingPower);
83		event Contributed(address contributor, uint256 amount, address delegate, uint256 previousTotalContributions);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol
```
151		event Proposed(
152			uint256 proposalId,
153			address proposer,
154			Proposal proposal
155		);
156		event ProposalAccepted(
157			uint256 proposalId,
158			address voter,
159			uint256 weight
160		);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC1155.sol
```
20		event ApprovalForAll(address  indexed  owner, address  indexed  operator, bool  approved);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol
```
37		event DistributionFeeClaimed(
38			ITokenDistributorParty indexed party,
39			address indexed feeRecipient,
40			TokenType tokenType,
41			address token,
42			uint256 amount
43		);
```

## 5. Use safeTransfer/safeTransferFrom consistently instead of transfer/transferFrom


It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

Reference: This similar medium-severity finding from Consensys Diligence Audit of Fei Protocol: https://consensys.net/diligence/audits/2021/01/fei-protocol/#unchecked-return-value-for-iweth-transfer-call

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol
```
143		super.transferFrom(owner, to, tokenId);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol
```
301		preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
```

## 6. Expressions for constant values such as a call to `keccak256()`, should use immutable rather than constant


https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol
```
19		uint256  private constant SHARED_STORAGE_SLOT =  uint256(keccak256("ProposalStorage.SharedProposalStorage"));
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol
```
80		uint256  private constant _STORAGE_SLOT =  uint256(keccak256('ProposalExecutionEngine.Storage'));
```