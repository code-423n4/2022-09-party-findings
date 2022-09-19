L-1 _SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE
`_mint()` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements IERC721Receiver

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L132

N-1  Event is missing indexed fields
Each event should use three indexed fields if there are three or more fields

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L151

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L156

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L162

N-2 Avoid using Floating Pragma:
Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L2
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L2

N-3 Use more recent version solidity
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(,) Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions
```
party-contracts-c4/contracts/utils/Proxy.sol:: 2 => pragma solidity ^0.8;
party-contracts-c4/contracts/utils/ReadOnlyDelegateCall.sol:: 2 => pragma solidity ^0.8;
party-contracts-c4/contracts/party/Party.sol:: 2 => pragma solidity ^0.8; 
party-contracts-c4/contracts/party/PartyFactory.sol:: 2 => pragma solidity ^0.8; 
party-contracts-c4/contracts/proposals/ProposalStorage.sol:: 2 => pragma solidity ^0.8; 
party-contracts-c4/contracts/proposals/FractionalizeProposal.sol:: 2 => pragma solidity ^0.8;	
party-contracts-c4/contracts/globals/Globals.sol:: 2 => pragma solidity ^0.8;	
party-contracts-c4/contracts/crowdfund/BuyCrowdfund.sol:: 2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CollectionBuyCrowdfund.sol:: 2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CrowdfundFactory:: 2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/CrowdfundNFT.sol:: 2 => pragma solidity ^0.8;	
party-contracts-c4/contracts/party/PartyGovernanceNFT.sol:: 2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnZoraProposal.sol:: 2 => pragma solidity ^0.8; 
party-contracts-c4/contracts/crowdfund/AuctionCrowdfund.sol:: 2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ArbitraryCallsProposal.sol:: 2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ProposalExecutionEngine.sol:: 2 => pragma solidity ^0.8; 
party-contracts-c4/contracts/distribution/TokenDistributor.sol:: 2 => pragma solidity ^0.8; 
party-contracts-c4/contracts/utils/Impparty-contracts-c4/lementation.sol:: 2 => pragma solidity ^0.8; 
party-contracts-c4/contracts/crowdfund/BuyCrowdfundBase.sol:: 2 => pragma solidity ^0.8;
party-contracts-c4/contracts/proposals/ListOnOpenseaProposal.sol:: 2 => pragma solidity ^0.8;
party-contracts-c4/contracts/crowdfund/Crowdfund.sol:: 2 => pragma solidity ^0.8; 
party-contracts-c4/contracts/party/PartyGovernance.sol:: 2 => pragma solidity ^0.8; 
```
