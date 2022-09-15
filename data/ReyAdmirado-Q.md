| | issue |
| ----------- | ----------- |
| 1 | [use of floating pragma](#1-use-of-floating-pragma) |
| 2 | [require()/revert() statements should have descriptive reason strings or custom error](#2-requirerevert-statements-should-have-descriptive-reason-strings-or-custom-error) |
| 3 | [Outdated compiler version](#3-outdated-compiler-version) |
| 4 | [inconsistent use of named return variables](#4-inconsistent-use-of-named-return-variables) |
| 5 | [require() should be used instead of assert()](#5-require-should-be-used-instead-of-assert) |
| 6 | [internal function has never been used should be deleted](#6-internal-function-has-never-been-used-should-be-deleted) |
| 7 | [Scoped contracts are missing proper NatSpec comments](#7-scoped-contracts-are-missing-proper-natspec-comments) |
| 8 | [event is missing indexed fields](#8-event-is-missing-indexed-fields) |
| 9 | [inconsistency in comments](#9-inconsistency-in-comments) |
| 10 | [constants should be defined rather than using magic numbers](#10-constants-should-be-defined-rather-than-using-magic-numbers) |
| 11 | [typo in comments](#11-typo-in-comments) |


## 1. use of floating pragma

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

- [EIP165.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/EIP165.sol)
- [ERC1155Receiver.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC1155Receiver.sol)
- [ERC721Receiver.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC721Receiver.sol)
- [Proxy.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol)
- [ReadOnlyDelegateCall.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol)
- [Party.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol)
- [PartyFactory.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol)
- [ProposalStorage.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol)
- [FractionalizeProposal.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol)
- [Globals.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol)
- [BuyCrowdfund.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol)
- [CollectionBuyCrowdfund.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol)
- [CrowdfundFactory.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol)
- [CrowdfundNFT.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol)
- [PartyGovernanceNFT.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol)
- [ListOnZoraProposal.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol)
- [AuctionCrowdfund.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol)
- [ArbitraryCallsProposal.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol)
- [CrowdfundNFT.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol)
- [PartyGovernanceNFT.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol)
- [ListOnZoraProposal.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol)
- [AuctionCrowdfund.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol)
- [ArbitraryCallsProposal.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol)
- [ProposalExecutionEngine.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol)
- [TokenDistributor.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol)
- [Implementation.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Implementation.sol)
- [BuyCrowdfundBase.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol)
- [ListOnOpenseaProposal.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol)
- [Crowdfund.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol)
- [PartyGovernance.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol)


## 2. require()/revert() statements should have descriptive reason strings or custom error

- [Proxy.sol#L32](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol#L32)

- [ReadOnlyDelegateCall.sol#L20](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L20)

- [ProposalExecutionEngine.sol#L246](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L246)
- [ProposalExecutionEngine.sol#L247](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L247)


## 3. Outdated compiler version

Using very old versions of Solidity prevents benefits of bug fixes and newer security checks. Using the latest versions might make contracts susceptible to undiscovered compiler bugs


## 4. inconsistent use of named return variables

there is an inconsistent use of named return variables in the contract some functions return named variables, others return explicit values. consider adopting a consistent approach.
this would improve both the explicitness and readability of the code, and it may also help reduce regressions during future code refactors.

- [PartyFactory.sol#L33](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L33)


## 5. require() should be used instead of assert()

Prior to solidity version 0.8.0, hitting an assert consumes the remainder of the transaction’s available gas rather than returning it, as require()/revert() do. assert() should be avoided even past solidity version 0.8.0 as its documentation states that “The assert function creates an error of type Panic(uint256). … Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix”.

- [ReadOnlyDelegateCall.sol#L30](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L30)

- [FractionalizeProposal.sol#L67](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L67)

- [ListOnZoraProposal.sol#L120](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L45)

- [PartyGovernanceNFT.sol#L179](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L179)

- [CrowdfundNFT.sol#L166](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L166)

- [TokenDistributor.sol#L385](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L385)
- [TokenDistributor.sol#L411](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L411)

- [ProposalExecutionEngine.sol#L142](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L142)

- [ListOnOpenseaProposal.sol#L221](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L221)
- [ListOnOpenseaProposal.sol#L302](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L302)



## 6. internal function has never been used should be deleted

- [ProposalStorage.sol#L29](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L29)


## 7. Scoped contracts are missing proper NatSpec comments

The scoped contracts are missing proper NatSpec comments such as @notice @dev @param on many places. It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI)

- [ProposalStorage.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol)
- [FractionalizeProposal.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol)
- [Globals.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol)
- [ArbitraryCallsProposal.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol)
- [ListOnZoraProposal.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol)


## 8. event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

- [FractionalizeProposal.sol#L22](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L22)

- [ArbitraryCallsProposal.sol#L35](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L35)

- [ListOnZoraProposal.sol#L45](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L45)

- [ListOnOpenseaProposal.sol#L63](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L63)
- [ListOnOpenseaProposal.sol#L71](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L71)
- [ListOnOpenseaProposal.sol#L77](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L77)
- [ListOnOpenseaProposal.sol#L84](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L84)

- [BuyCrowdfundBase.sol#L52](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L52)

- [PartyGovernance.sol#L151](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L151)
- [PartyGovernance.sol#L156](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L156)
- [PartyGovernance.sol#L162](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L162)
- [PartyGovernance.sol#L165](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L165)
- [PartyGovernance.sol#L168](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L168)

- [Crowdfund.sol#L82](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L82)
- [Crowdfund.sol#L83](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L83)


## 9. inconsistency in comments

should have 3 /:

- [PartyGovernanceNFT.sol#L100](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L100)



## 10. constants should be defined rather than using magic numbers 
Even assembly can benefit from using readable constants instead of hex/numeric literals

- [TokenDistributor.sol#L352](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L352)

- [Crowdfund.sol#L129](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L129)
- [Crowdfund.sol#L132](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L132)
- [Crowdfund.sol#L135](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L135)


## 11. typo in comments

receieves --> receives

- [BuyCrowdfundBase.sol#L31](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L31)

recipeint --> recipient

- [Crowdfund.sol#L46](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L46)
