

## Missing fee parameter validation


Some fee parameters of functions are not checked for invalid values. Validate the parameters:
        
        
### Code instances:

        ListOnOpenseaProposal._listOnOpensea (fees)
        TokenDistributor.createNativeDistribution (feeBps)
        TokenDistributor.createErc20Distribution (feeBps)
        ListOnOpenseaProposal._listOnOpensea (feeRecipients)
        TokenDistributor.createNativeDistribution (feeRecipient)
        TokenDistributor.createErc20Distribution (feeRecipient)



## safeApprove of openZeppelin is deprecated


You use safeApprove of openZeppelin although it's deprecated.
(see https://github.com/OpenZeppelin/openzeppelin-contracts/blob/566a774222707e424896c0c390a84dc3c13bdcb2/contracts/token/ERC20/utils/SafeERC20.sol#L38)
You should change it to increase/decrease Allowance as OpenZeppilin says.
        
### Code instances:

        Deprecated safeApprove in ArbitraryCallsProposal.sol line 172: if (selector == IERC721.approve.selector) {
        Deprecated safeApprove in ListOnOpenseaProposal.sol line 255: token.approve(conduit, tokenId);
        Deprecated safeApprove in ListOnOpenseaProposal.sol line 366: token.approve(address(0), tokenId);
        Deprecated safeApprove in ListOnZoraProposal.sol line 142: token.approve(address(ZORA), tokenId);
        Deprecated safeApprove in FractionalizeProposal.sol line 52: data.token.approve(address(VAULT_FACTORY), data.tokenId);



## Require with empty message

The following requires are with empty messages. 
This is very important to add a message for any require. So the user has enough information to know the reason of failure.
### Code instances:

        Solidity file: ReadOnlyDelegateCall.sol, In line 20 with Empty Require message.
        Solidity file: ProposalExecutionEngine.sol, In line 246 with Empty Require message.
        Solidity file: ProposalExecutionEngine.sol, In line 247 with Empty Require message.



## Not verified input


external / public functions parameters should be validated to make sure the address is not 0.
Otherwise if not given the right input it can mistakenly lead to loss of user funds.    

### Code instances:

        ERC20.sol.transfer to
        TokenDistributor.sol.createNativeDistribution feeRecipient
        Globals.sol.setAddress value
        TokenDistributor.sol.createErc20Distribution feeRecipient
        ProposalExecutionEngine.sol.initialize oldImpl
        PartyGovernanceNFT.sol.transferFrom to
        PartyGovernanceNFT.sol.safeTransferFrom to
        ReadOnlyDelegateCall.sol.delegateCallAndRevert impl
        Globals.sol.setIncludesAddress value
        BuyCrowdfund.sol.buy callTarget
        CollectionBuyCrowdfund.sol.buy callTarget
        ERC20.sol.transferFrom to
        ERC20.sol.transferFrom from
        TokenDistributor.sol.emergencyWithdraw token
        TokenDistributor.sol.claimFee recipient
        TokenDistributor.sol.emergencyWithdraw recipient
        ERC20.sol.approve spender
        PartyGovernanceNFT.sol.mint delegate
        Globals.sol.transferMultiSig newMultiSig
        ERC20.sol.permit spender



## Solidity compiler versions mismatch


The project is compiled with different versions of solidity, which is not recommended because it can lead to undefined behaviors.
        
### Code instance:

        



## Not verified owner


        owner param should be validated to make sure the owner address is not address(0).
        Otherwise if not given the right input all only owner accessible functions will be unaccessible.
        
        
### Code instances:

        ERC20.sol.permit owner
        PartyGovernanceNFT.sol.transferFrom owner
        PartyGovernanceNFT.sol.mint owner
        PartyGovernanceNFT.sol.safeTransferFrom owner



## Named return issue

Users can mistakenly think that the return value is the named return, but it is actually the actualreturn statement that comes after. To know that the user needs to read the code and is confusing.
Furthermore, removing either the actual return or the named return will save gas. 

### Code instances:

        LibSafeERC721.sol, safeOwnerOf
        ListOnZoraProposal.sol, _executeListOnZora
        ArbitraryCallsProposal.sol, _isCallAllowed
        ArbitraryCallsProposal.sol, _executeArbitraryCalls
        FractionalizeProposal.sol, _executeFractionalize
        Crowdfund.sol, _createParty
        CollectionBuyCrowdfund.sol, buy
        CrowdfundFactory.sol, _prepareGate
        CrowdfundNFT.sol, balanceOf
        AuctionCrowdfund.sol, _getFinalPrice
        TokenDistributor.sol, _getBalanceId
        ProposalExecutionEngine.sol, getCurrentInProgressProposalId
        BuyCrowdfund.sol, buy
        PartyGovernanceNFT.sol, ownerOf
        ListOnOpenseaProposal.sol, _executeListOnOpensea
        ProposalStorage.sol, _getProposalExecutionEngine
        ListOnZoraProposal.sol, _settleZoraAuction



## Assert instead require to validate user inputs


        From solidity docs: Properly functioning code should never reach a failing assert statement; if this happens there is a bug in your contract which you should fix.
        With assert the user pays the gas and with require it doesn't. The ETH network gas isn't cheap and users can see it as a scam.
        
### Code instances:

        ListOnOpenseaProposal.sol : reachable assert in line 220
        CrowdfundNFT.sol : reachable assert in line 165
        ListOnZoraProposal.sol : reachable assert in line 119
        PartyGovernanceNFT.sol : reachable assert in line 178
        TokenDistributor.sol : reachable assert in line 384
        ListOnOpenseaProposal.sol : reachable assert in line 301
        ProposalExecutionEngine.sol : reachable assert in line 141
        ReadOnlyDelegateCall.sol : reachable assert in line 29
        FractionalizeProposal.sol : reachable assert in line 66
        TokenDistributor.sol : reachable assert in line 410



## Never used parameters

Those are functions and parameters pairs that the function doesn't use the parameter. In case those functions are external/public this is even worst since the user is required to put value that never used and can misslead him and waste its time. 

### Code instances:

        BuyCrowdfundBase.sol: function _buy parameter callData isn't used. (_buy is internal)
        ProposalExecutionEngine.sol: function initialize parameter initializeData isn't used. (initialize is external)
        ProposalExecutionEngine.sol: function initialize parameter oldImpl isn't used. (initialize is external)
        LibAddress.sol: function transferEth parameter amount isn't used. (transferEth is internal)



## Open TODOs

Open TODOs can hint at programming or architectural errors that still need to be fixed. 
These files has open TODOs:

### Code instances:

Open TODO in PartyGovernanceNFTRenderer.sol line 55 :             // TODO: Parameterize

Open TODO in PartyGovernanceNFTRenderer.sol line 86 :         // TODO: Write decimal string library

Open TODO in CrowdfundNFTRenderer.sol line 33 :             // TODO: Parameterize

Open TODO in DummyERC721Renderer.sol line 7 :         // TODO: make this human readable




## Check transfer receiver is not 0 to avoid burned money


Transferring tokens to the zero address is usually prohibited to accidentally avoid "burning" tokens by sending them to an unrecoverable zero address.


### Code instances:

        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/distribution/TokenDistributor.sol#L208
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/distribution/TokenDistributor.sol#L172
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/crowdfund/CrowdfundNFT.sol#L154
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/distribution/TokenDistributor.sol#L383
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/crowdfund/CrowdfundNFT.sol#L146
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/party/PartyGovernanceNFT.sol#L163
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/vendor/solmate/ERC20.sol#L84
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/party/PartyGovernanceNFT.sol#L141
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/vendor/solmate/ERC20.sol#L106
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/vendor/solmate/ERC20.sol#L191
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/distribution/TokenDistributor.sol#L323
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/vendor/solmate/ERC20.sol#L203



## Missing commenting


        The following functions are missing commenting as describe below:
        
### Code instances:

        AuctionCrowdfund.sol, _getFinalPrice (internal), @return is missing
        AuctionCrowdfund.sol, getCrowdfundLifecycle (public), @return is missing



## Add a timelock

To give more trust to users: functions that set key/critical variables should be put behind a timelock.

### Code instances:

        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/globals/Globals.sol#L71
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/globals/Globals.sol#L67
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/globals/Globals.sol#L59
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/globals/Globals.sol#L75
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/globals/Globals.sol#L79
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/globals/Globals.sol#L63
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/crowdfund/CrowdfundNFT.sol#L77



-------- med ---------
## Must approve 0 first


Some tokens (like USDT) do not work when changing the allowance from an existing non-zero allowance value.
They must first be approved by zero and then the actual allowance must be approved.

### Code instances:

approve without approving 0 first FractionalizeProposal.sol, 52,         data.token.approve(address(VAULT_FACTORY), data.tokenId);

approve without approving 0 first ListOnZoraProposal.sol, 142,         token.approve(address(ZORA), tokenId);

approve without approving 0 first ListOnOpenseaProposal.sol, 255,         token.approve(conduit, tokenId);




## Unsafe Cast

use openzeppilin's safeCast in:
 
### Code instances:

        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/utils/LibSafeCast.sol#L19 : unsafe cast uint128(v)
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/utils/LibSafeCast.sol#L44 : unsafe cast uint128(x)
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/utils/LibSafeCast.sol#L12 : unsafe cast uint96(v)
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/renderers/CrowdfundNFTRenderer.sol#L107 : unsafe cast uint160(tokenId)
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/utils/LibSafeCast.sol#L26 : unsafe cast uint192(v)
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/utils/LibSafeCast.sol#L55 : unsafe cast uint40(x)



## Two arrays length mismatch


The functions below fail to perform input validation on arrays to verify the lengths match.
A mismatch could lead to an exception or undefined behavior.
Consider making this a medium risk please.
    
    
### Code instances

        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/proposals/LibProposal.sol#L22 isTokenIdPrecious ['preciousTokens', 'preciousTokenIds']
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/utils/PartyHelpers.sol#L73 getVotingPowersAt ['voters', 'indexes']
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/distribution/TokenDistributor.sol#L225 batchClaim ['infos', 'partyTokenIds']
        https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/distribution/TokenDistributor.sol#L236 batchClaimFee ['infos', 'recipients']



## Override function but with different argument location


    
    
### Code instances:

Crowdfund.sol._initialize inherent CrowdfundNFT.sol._initialize but the parameters does not match
 https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/crowdfund/CrowdfundNFT.sol#L39
BuyCrowdfundBase.sol._initialize inherent CrowdfundNFT.sol._initialize but the parameters does not match
 https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/crowdfund/CrowdfundNFT.sol#L39
ProposalExecutionEngine.sol.initialize inherent IProposalExecutionEngine.sol.initialize but the parameters does not match
 https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/proposals/IProposalExecutionEngine.sol#L18
BuyCrowdfundBase.sol._initialize inherent Crowdfund.sol._initialize but the parameters does not match
 https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/crowdfund/Crowdfund.sol#L124
CrowdfundNFT.sol.safeTransferFrom inherent IERC721.sol.safeTransferFrom but the parameters does not match
 https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/tokens/IERC721.sol#L11
CrowdfundNFT.sol.safeTransferFrom inherent IERC721.sol.safeTransferFrom but the parameters does not match
 https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/tokens/IERC721.sol#L12
Crowdfund.sol._burn inherent CrowdfundNFT.sol._burn but the parameters does not match
 https://github.com/code-423n4/2022-09-party/tree/main/party-contracts-c4-main/contracts/crowdfund/CrowdfundNFT.sol#L150



-------- high ---------
## approve return value is ignored

Some tokens don't correctly implement the EIP20 standard and their approve function returns void instead of a success boolean. 
Calling these functions with the correct EIP20 function signatures will always revert.
Tokens that don't correctly implement the latest EIP20 spec, like USDT, will be unusable in the mentioned contracts as they revert the transaction because of the missing return value.
We recommend using OpenZeppelin’s SafeERC20 versions with the safeApprove function that handle the return value check as well as non-standard-compliant tokens.
The list of occurrences in format (solidity file, line number, actual line)
### Code instances:

ListOnOpenseaProposal.sol, 255,         token.approve(conduit, tokenId);

ListOnOpenseaProposal.sol, 366,             token.approve(address(0), tokenId);

ListOnZoraProposal.sol, 142,         token.approve(address(ZORA), tokenId);




## transfer return value of a general ERC20 is ignored

Need to use safeTransfer instead of transfer. As there are popular tokens, such as USDT that transfer/trasnferFrom method doesn’t return anything. The transfer return value has to be checked (as there are some other tokens that returns false instead revert), that means you must 
 1. Check the transfer return value
Another popular possibility is to add a whiteList.
Those are the appearances (solidity file, line number, actual line):

### Code instance:

        PartyGovernanceNFT.sol, 143 (transferFrom), super.transferFrom(owner, to, tokenId);

