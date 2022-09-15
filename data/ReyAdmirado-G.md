
| | issue |
| ----------- | ----------- |
| 1 | [using `calldata` instead of `memory` for read-only arguments in external functions saves gas](#1-using-calldata-instead-of-memory-for-read-only-arguments-in-external-functions-saves-gas) |
| 2 | [using `bool` for storage incurs overhead](#2-using-bool-for-storage-incurs-overhead) |
| 3 | [abi.encode() is less efficient than abi.encodepacked()](#3-abiencode-is-less-efficient-than-abiencodepacked) |
| 4 | [expressions for constant values such as a call to keccak256(), should use immutable rather than constant](#4-expressions-for-constant-values-such-as-a-call-to-keccak256-should-use-immutable-rather-than-constant) |
| 5 | [not using the named return variables when a function returns, wastes deployment gas](#5-not-using-the-named-return-variables-when-a-function-returns-wastes-deployment-gas) |
| 6 | [`<array>.length` should not be looked up in every loop of a for-loop](#6-arraylength-should-not-be-looked-up-in-every-loop-of-a-for-loop) |
| 7 | [`++i` costs less gas than `i++`, especially when it’s used in for-loops (--i/i-- too)](#7-i-costs-less-gas-than-i-especially-when-its-used-in-for-loops---ii---too) |
| 8 | [` ++i/i++` should be `unchecked{++i}/unchecked{i++}` when it is not possible for them to overflow, as is the case when used in for-loop and while-loops](#8--ii-should-be-uncheckediuncheckedi-when-it-is-not-possible-for-them-to-overflow-as-is-the-case-when-used-in-for-loop-and-while-loops) |
| 9 | [internal function never used should be deleted to save gas](#9-internal-function-never-used-should-be-deleted-to-save-gas) |
| 10 | [usage of uint/int smaller than 32 bytes (256 bits) incurs overhead](#10-usage-of-uintint-smaller-than-32-bytes-256-bits-incurs-overhead) |
| 11 | [bytes constants are more efficient than string constants](#11-bytes-constants-are-more-efficient-than-string-constants) |
| 12 | [it costs more gas to initialize non-constant/non-immutable variables to zero than to let the default of zero be applied](#12-it-costs-more-gas-to-initialize-non-constantnon-immutable-variables-to-zero-than-to-let-the-default-of-zero-be-applied) |
| 13 | [state variables can be packed into fewer storage slots](#13-state-variables-can-be-packed-into-fewer-storage-slots) |
| 14 | [use custom errors rather than require() strings to save deployment gas](#14-use-custom-errors-rather-than-require-strings-to-save-deployment-gas) |
| 15 | [internal functions only called once can be inlined to save gas](#15-internal-functions-only-called-once-can-be-inlined-to-save-gas) |
| 16 | [can make the variable outside the loop to save gas](#16-can-make-the-variable-outside-the-loop-to-save-gas) |
| 17 | [`<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables](#17-x--y-costs-more-gas-than-x--x--y-for-state-variables) |


## 1. using `calldata` instead of `memory` for read-only arguments in external functions saves gas

- [PartyFactory.sol#L26](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L26)

- [Party.sol#L33](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L33)

- [ReadOnlyDelegateCall.sol#L8](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L8)
- [ReadOnlyDelegateCall.sol#L18](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L18)

- [BuyCrowdfund.sol#L68](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L68)
- [BuyCrowdfund.sol#L98](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L98)

- [CollectionBuyCrowdfund.sol#L83](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L83)
- [CollectionBuyCrowdfund.sol#L113](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L113)

- [AuctionCrowdfund.sol#L110](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L110)
- [AuctionCrowdfund.sol#L196](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L196)

- [ProposalExecutionEngine.sol#L116](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L116)


## 2. using `bool` for storage incurs overhead

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

- [Proxy.sol#L18](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol#L18)

- [ReadOnlyDelegateCall.sol#L21](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L21)
- [ReadOnlyDelegateCall.sol#L33](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L33)

- [ProposalStorage.sol#L39](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L39)

- [CollectionBuyCrowdfund.sol#L61](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L61)

- [CrowdfundFactory.sol#L124](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L124)

- [ArbitraryCallsProposal.sol#L46](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L46)
- [ArbitraryCallsProposal.sol#L50](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L50)
- [ArbitraryCallsProposal.sol#L98](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L98)
- [ArbitraryCallsProposal.sol#L112](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L112)
- [ArbitraryCallsProposal.sol#L144](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L144)
- [ArbitraryCallsProposal.sol#L187](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L187)

- [AuctionCrowdfund.sol#L178](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L178)
- [AuctionCrowdfund.sol#L218](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L218)


## 3. abi.encode() is less efficient than abi.encodepacked()

- [ReadOnlyDelegateCall.sol#L23](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L23)

- [ListOnZoraProposal.sol#L115](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L115)

- [ListOnOpenseaProposal.sol#L164](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L164)
- [ListOnOpenseaProposal.sol#L219](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L219)


## 4. expressions for constant values such as a call to keccak256(), should use immutable rather than constant

- [ProposalStorage.sol#L19](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L19)

- [ProposalExecutionEngine.sol#L80](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L80)


## 5. not using the named return variables when a function returns, wastes deployment gas

- [ProposalStorage.sol#L24](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L24)

- [BuyCrowdfund.sol#L105](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L105)

- [CollectionBuyCrowdfund.sol#L122](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L122)

- [CrowdfundFactory.sol#L113](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L113)

- [ArbitraryCallsProposal.sol#L150](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L150)

- [AuctionCrowdfund.sol#L287](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L287)

- [ListOnZoraProposal.sol#L82](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L82)
- [ListOnZoraProposal.sol#L172](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L172)

- [PartyGovernanceNFT.sol#L70](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L70)

- [CrowdfundNFT.sol#L133](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L133)

- [TokenDistributor.sol#L406](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L406)

- [ProposalExecutionEngine.sol#L110](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L110)

- [ListOnOpenseaProposal.sol#L127](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L127)


## 6. `<array>.length` should not be looked up in every loop of a for-loop

This reduce gas cost as show here https://forum.openzeppelin.com/t/a-collection-of-gas-optimisation-tricks/19966/5

- [CollectionBuyCrowdfund.sol#L62](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62)

- [ArbitraryCallsProposal.sol#L52](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52)
- [ArbitraryCallsProposal.sol#L61](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61)
- [ArbitraryCallsProposal.sol#L78](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78)

- [TokenDistributor.sol#L230](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230)
- [TokenDistributor.sol#L239](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239)

- [ListOnOpenseaProposal.sol#L291](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291)

- [PartyGovernance.sol#L306](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L306)

- [Crowdfund.sol#L180](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180)
- [Crowdfund.sol#L242](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L242)
- [Crowdfund.sol#L300](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300)
- [Crowdfund.sol#L348](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L348)


## 7. `++i` costs less gas than `i++`, especially when it’s used in for-loops (--i/i-- too)

Saves 6 gas per loop

- [CollectionBuyCrowdfund.sol#L62](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62)


## 8. ` ++i/i++` should be `unchecked{++i}/unchecked{i++}` when it is not possible for them to overflow, as is the case when used in for-loop and while-loops

In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.

- [CollectionBuyCrowdfund.sol#L62](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62)

- [ArbitraryCallsProposal.sol#L52](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52)
- [ArbitraryCallsProposal.sol#L61](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61)
- [ArbitraryCallsProposal.sol#L78](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78)

- [TokenDistributor.sol#L230](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230)
- [TokenDistributor.sol#L239](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239)

- [ListOnOpenseaProposal.sol#L291](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291)

- [PartyGovernance.sol#L306](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L306)

- [Crowdfund.sol#L180](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180)
- [Crowdfund.sol#L242](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L242)
- [Crowdfund.sol#L300](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300)
- [Crowdfund.sol#L348](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L348)


## 9. internal function never used should be deleted to save gas

- [ProposalStorage.sol#L29](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L29)


## 10. usage of uint/int smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Use a larger size then downcast where needed

- [BuyCrowdfund.sol#L100](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L100)

- [CollectionBuyCrowdfund.sol#L116](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L116)

- [AuctionCrowdfund.sol#L99](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L99)
- [AuctionCrowdfund.sol#L94](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L94)
- [AuctionCrowdfund.sol#L96](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L96)
- [AuctionCrowdfund.sol#L170](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L170)
- [AuctionCrowdfund.sol#L210](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L210)

- [ListOnZoraProposal.sol#L94](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L94)
- [ListOnZoraProposal.sol#L95](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L95)
- [ListOnZoraProposal.sol#L102](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L102)
- [ListOnZoraProposal.sol#L133](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L133)
- [ListOnZoraProposal.sol#L135](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L135)
- [ListOnZoraProposal.sol#L166](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L166)

- [TokenDistributor.sol#L101](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L101)
- [TokenDistributor.sol#L122](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L122)
- [TokenDistributor.sol#L166](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L166)
- [TokenDistributor.sol#L338](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L338)
- [TokenDistributor.sol#L352](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L352)
- [TokenDistributor.sol#L353](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L353)

- [ListOnOpenseaProposal.sol#L151](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L151)
- [ListOnOpenseaProposal.sol#L153](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L153)
- [ListOnOpenseaProposal.sol#L202](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L202)
- [ListOnOpenseaProposal.sol#L203](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L203)

- [BuyCrowdfundBase.sol#L60](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L60)
- [BuyCrowdfundBase.sol#L62](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L62)
- [BuyCrowdfundBase.sol#L64](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L64)
- [BuyCrowdfundBase.sol#L94](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L94)
- [BuyCrowdfundBase.sol#L114](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L114)


## 11. bytes constants are more efficient than string constants
If data can fit into 32 bytes, then you should use bytes32 datatype rather than bytes or strings as it is cheaper in solidity.

- [BuyCrowdfund.sol#L22](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L22)
- [BuyCrowdfund.sol#L24](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L24)

- [CollectionBuyCrowdfund.sol#L25](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L25)
- [CollectionBuyCrowdfund.sol#L27](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L27)

- [AuctionCrowdfund.sol#L36](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L36)
- [AuctionCrowdfund.sol#L38](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L38)

- [PartyGovernanceNFT.sol#L50](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L50)
- [PartyGovernanceNFT.sol#L51](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L51)

- [CrowdfundNFT.sol#L21](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L21)
- [CrowdfundNFT.sol#L24](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L24)
- [CrowdfundNFT.sol#L39](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L39)

- [BuyCrowdfundBase.sol#L23](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L23)
- [BuyCrowdfundBase.sol#L25](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L25)


## 12. it costs more gas to initialize non-constant/non-immutable variables to zero than to let the default of zero be applied

- [ArbitraryCallsProposal.sol#L52](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52)
- [ArbitraryCallsProposal.sol#L61](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61)
- [ArbitraryCallsProposal.sol#L78](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78)

- [TokenDistributor.sol#L230](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230)
- [TokenDistributor.sol#L239](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239)

- [ListOnOpenseaProposal.sol#L291](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L291)

- [PartyGovernance.sol#L306](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L306)

- [Crowdfund.sol#L180](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180)
- [Crowdfund.sol#L242](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L242)
- [Crowdfund.sol#L300](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300)
- [Crowdfund.sol#L348](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L348)


## 13. state variables can be packed into fewer storage slots

If variables occupying the same slot are both written the same function or by the constructor, avoids a separate Gsset (20000 gas). Reads of the variables are also cheaper.

bring line 62 after 55:
- [TokenDistributor.sol#L55-62](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L55)


## 14. use custom errors rather than require() strings to save deployment gas

https://blog.soliditylang.org/2021/04/21/custom-errors/

- [Proxy.sol#L32](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol#L32)

- [ReadOnlyDelegateCall.sol#L20](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L20)

- [ProposalExecutionEngine.sol#L246](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L246)
- [ProposalExecutionEngine.sol#L247](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L247)


## 15. internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

- [ProposalExecutionEngine.sol#L205](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L205)

- [ListOnOpenseaProposal.sol#L123](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L123)


## 16. can make the variable outside the loop to save gas

- [Crowdfund.sol#L349](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L349)


## 17. `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

- [Crowdfund.sol#L243](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L243)
- [Crowdfund.sol#L352](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L352)
- [Crowdfund.sol#L355](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L355)
- [Crowdfund.sol#L359](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L359)
- [Crowdfund.sol#L374](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L374)
- [Crowdfund.sol#L411](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L411)
- [Crowdfund.sol#L427](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L427)

- [PartyGovernance.sol#L595](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L595)
- [PartyGovernance.sol#L959](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L959)
