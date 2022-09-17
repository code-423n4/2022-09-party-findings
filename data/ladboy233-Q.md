## Increasing unit test and integration test coverage

The project requires the integration of Zora, Opensea and fractionalize protocol. We recommand project add unit test and integration test to increase the test coverage for the contract below


```
name                                                   test coverage
contracts/crowdfund/CrowdfundNFT.sol	                   44.00%
contracts/party/PartyGovernanceNFT.sol	                   57.69%
contracts/party/PartyFactory.sol 🌀	                   80.00%
contracts/proposals/ProposalStorage.sol 🖥 👥 🧮	   80.00%
contracts/proposals/ProposalExecutionEngine.sol 🖥 	   86.44%
```

## USE OF FLOATING PRAGMA

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

https://swcregistry.io/docs/SWC-103

all the codes are using 

```
// SPDX-License-Identifier: Beta Software
pragma solidity ^0.8;
```

we recommend locking the solidity version.

## USE A MORE RECENT VERSION OF SOLIDITY

 Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

all the codes are using 

```
// SPDX-License-Identifier: Beta Software
pragma solidity ^0.8;
```

we recommend using up to date solidity version

## _burn function in Crowdfund.sol missing zero address check for the receiver (contributor) address

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L444

```
     function _burn(address payable contributor, CrowdfundLifecycle lc, Party party_)
        private
    {
```

the function does not check if the contributor is a address(0)

burn(address(0)) alaways succeed.

## unhandled external call return value

we recommend the project handle the external call to make sure the external call do not fail silently. 

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L301

```
     preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L795

```
    (success, ) = targetAddress.call{value: amountEth}(targetCallData);
```

## Missing emit emission in important state change function

We recommend the project add emit emission when changing important state

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L327

```
    function disableEmergencyActions() onlyPartyDao external {
        emergencyActionsDisabled = true;
    }
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L800

```
     function disableEmergencyExecute() external onlyPartyDaoOrHost onlyDelegateCall {
        emergencyExecuteDisabled = true;
    }
```

