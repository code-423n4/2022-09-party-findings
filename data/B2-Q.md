## Public function that could be declared external
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L35-L39](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L35-L39)
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L61-L65](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L61-L65)
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L87L-91](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L87L-91)

## Missing zero address validation
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol#L12-L22](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol#L12-L22)
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L18-L23](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L18-L23)
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L22-L27](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L22-L27)

## Unlocked Pragma
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/EIP165.sol#L2](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/EIP165.sol#L2)
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC1155Receiver.sol#L2](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC1155Receiver.sol#L2)
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC721Receiver.sol#L2](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC721Receiver.sol#L2)
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol#L](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Proxy.sol#L2)
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L2](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L2)
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L2](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L2)

## MULTIPLE UINT256 MAPPINGS CAN BE COMBINED INTO A SINGLE MAPPING OF AN UNIT256 TO A STRUCT, WHERE APPROPRIATE
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L10-L12](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L10-L12)
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L64-L71](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L64-L71)
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L207-L215](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L207-L215)

## Dependence on block.timestamp is risky as it can be manipulated by miners
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L166](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L166)
- [https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L210]-(https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L210)
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L118
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L276
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L117

## APPROVE should be replaced with SAFEAPPROVE or SAFEINCREASEALLOWANCE()/SAFEDECREASEALLOWANCE()
`approve` is subject to a known front-runnning attack. Consider using safeApprove instead.
Instances are below:
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L143

## Multiple return statements used
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L164-L204
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L107-L130
- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L283-L290


