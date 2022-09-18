#1 Natspec incomplete

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/Proxy.sol#L16

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L18

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/ReadOnlyDelegateCall.sol#L27

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L228

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L251
Natspec comment incomplete. I suggest to complete the natspec comment.

#2 Floating Pragma

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/Proxy.sol#L2

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/LibAddress.sol#L2

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/Implementation.sol#L2

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/utils/LibERC20Compat.sol#L2

and etc.

In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
We suggest to recheck your file and update the pragma 

#3 Unnecessary import file

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyFactory.sol#L6

The import is not used in any way. Remove it to improve readability of the code

#4 File missing natspec

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalStorage.sol#L1-L59

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L23-L82

the file  has a natspec comment  to explain utility about function or parameter. So add natspec comment to increase readability

#5 Missing indexed field

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/FractionalizeProposal.sol#L21

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L66

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L152

event is missing indexed fields. Add indexed at important field for increase creadibility.

#5  Use safeapprove or safeIncraseAllowance/safeDecreaseAllowance.

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L256

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L367

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/FractionalizeProposal.sol#L53

the approve() function returns a boolean indicating whether it was successful or not. Best practice is to either check the return value or use safeApprove() / safeIncreaseAllowance() which will revert if the operation was unsuccessful. we suggest to use safeApprove() / safeIncreaseAllowance() instead of approve()

#6 Lack zero address in constructor

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/globals/Globals.sol#L23

Checking addresses against zero-address during initialization in constructor is a security best-practice. However, such checks are missing in multiple constructors. Allowing zero-addresses will lead to contract reverts and force redeployments if there are no setters for such address variables. So i suggest to Add zero-address checks in all the constructors

#7 Useless function

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CrowdfundNFT.sol#L47-L99

remove or use the unused function in contract before deploy the contract to increase readibility and saving gas fee.

#8 MintAuthority missing check address

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernanceNFT.sol#L55

add simple check for requirement _to mint can be zero address. 

#9 Unused receive() function

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/AuctionCrowdfund.sol#L144

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert. Remove these functions, or include a call to other function in receive(), so that a user that mistakenly sends ETH to retrieves it immediately.

#10 Missing message requirement

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ProposalExecutionEngine.sol#L246-L247

in requirement statement need message. when the running process revert require will show revert message, so i suggest to add message in require.

#11 Use safemint

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L439

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L479

_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA. In OpenZeppelin have versions of this function so that NFTs aren’t lost if they’re minted to contracts that cannot transfer them back out. so i suggest to use safemint() instead min().

#12 Prevent divided by 0

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L1062

Division by 0 can lead to accidentally revert. so add check value can't be zero