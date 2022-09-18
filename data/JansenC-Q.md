## Different versions of solidity and Pragma floating
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributorParty.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/IGlobals.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/LibGlobals.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/IPartyFactory.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/IProposalExecutionEngine.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/LibProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalStorage.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC1155Receiver.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/ERC721Receiver.sol#2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC1155.sol#2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC20.sol#2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721.sol#2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/tokens/IERC721Receiver.sol#2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/EIP165.sol#2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Implementation.sol#2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibERC20Compat.sol#2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibRawResult.sol#2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibSafeCast.sol#2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#5
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC721.sol#5
References: https://swcregistry.io/docs/SWC-103, CWE-664: Improper Control of a Resource Through its Lifetime

## Lacks a zero-check
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L23
https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L27
