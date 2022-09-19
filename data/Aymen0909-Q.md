# QA Report

## Summary

|               | Issue         | Risk     |Instances     |
| :-------------: |:-------------|:-------------:|:-------------:|
| 1      | `constructor` and `transferMultiSig` function missing zero address checks inside `Globals` contract | Low | 2 |
| 2      | `require` should be used instead of `assert` | Low | 5  |
| 3      | Non-library/Non-interface files should use fixed compiler versions, not floating ones     | NC | 21  |
| 4      | Non-assembly method available | NC | 1 |
| 5      | Named return variables not used anywhere in the function | NC | 18 |
| 6      | `constant` should be defined rather than using magic numbers  | NC | 13 |


## Findings

### 1-  `constructor` and `transferMultiSig` function missing zero address checks inside `Globals` contract :

the `constructor` and `transferMultiSig` function inside the `Globals` contract are missing non zero address checks when setting the `multiSig` address which can cause a set/transfer of `multiSig` to `address(0)`.

#### Impact - Low Risk

#### Proof of Concept
Instances include:

File: contracts/globals/Globals.sol

[multiSig = multiSig_;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L24)

[multiSig = newMultiSig;](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L28)

#### Mitigation
Add non-zero address checks in the instances aforementioned.

### 2- `require` should be used instead of `assert` :

Prior to solidity version 0.8.0, hitting an assert consumes the remainder of the transaction’s available gas rather than returning it, as `require()/revert()` do. `assert()` should be avoided even past solidity version 0.8.0 as its [documentation](https://docs.soliditylang.org/en/v0.8.14/control-structures.html#panic-via-assert-and-error-via-require) states that “The assert function creates an error of type Panic(uint256). … Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix”.

#### Impact - Low Risk

#### Proof of Concept
Instances include:

File: contracts/distribution/TokenDistributor.sol

[assert(tokenType == TokenType.Erc20);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L385)

[assert(tokenType == TokenType.Erc20);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L411)

File: contracts/party/PartyGovernance.sol

[assert(tokenType == ITokenDistributor.TokenType.Erc20);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L504)

File: contracts/proposals/FractionalizeProposal.sol

[assert(vault.balanceOf(address(this)) == supply);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L67)

File: contracts/proposals/ProposalExecutionEngine.sol

[assert(currentInProgressProposalId == 0);](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L142)

#### Mitigation
Replace the `assert` checks aforementioned with `require` statements.

### 3- Non-library/interface files should use fixed compiler versions, not floating ones :

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

check this source : https://swcregistry.io/docs/SWC-103

#### Impact - NON CRITICAL

#### Proof of Concept
All contracts use a floating solidity version : 

```
pragma solidity ^0.8;
```
#### Mitigation
Lock the pragma version to the same version as used in the other contracts and also consider known bugs (https://github.com/ethereum/solidity/releases) for the compiler version that is chosen.

Pragma statements can be allowed to float when a contract is intended for consumption by other developers, as in the case with contracts in a library or EthPM package. Otherwise, the developer would need to manually update the pragma in order to compile it locally.

### 4- Non-assembly method available :

`<address>.code.length` can be used in Solidity `>= 0.8.0` to access an account’s code size and check if it is a contract without inline assembly.

#### Impact - NON CRITICAL

#### Proof of Concept

There is 1 instance of this issue:

File: contracts/utils/Implementation.sol

[assembly { codeSize := extcodesize(address()) }](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Implementation.sol#L24)

#### Mitigation
Assembly method can be replaced it with : 

```
codeSize = address().code.length;
```

### 5- Named return variables not used anywhere in the function :

When Named return variable are declared they should be used inside the function instead of the return statement or if not they should be removed to avoid confusion.

#### Impact - NON CRITICAL

#### Proof of Concept
Instances include:

File: contracts/crowdfund/CrowdfundFactory.sol

[returns (bytes12 newGateKeeperId)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L113)

File: contracts/distribution/TokenDistributor.sol

[returns (bytes32 balanceId)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L406)

File: contracts/party/PartyGovernance.sol

[returns (uint96 votingPower)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L350)

[returns (uint96 votingPower)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L364)

[returns (uint256 index)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L425)

[returns (ITokenDistributor.DistributionInfo memory distInfo)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L491)

[returns (uint256 totalVotes)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L565)

[returns (bool completed)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L814)

[returns (VotingPowerSnapshot memory snap)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L851)

[returns (ProposalStatus status)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1015)

File: contracts/party/PartyGovernanceNFT.sol

[returns (address owner)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L70)

File: contracts/proposals/ArbitraryCallsProposal.sol

[returns (bytes memory nextProgressData)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L41)

[returns (bool isAllowed)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L150)

File: contracts/proposals/FractionalizeProposal.sol

[returns (bytes memory nextProgressData)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L43)

File: contracts/proposals/ListOnZoraProposal.sol

[returns (bytes memory nextProgressData)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L82)

[returns (bool sold)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L172)

File: contracts/proposals/ProposalExecutionEngine.sol

[returns (uint256 id)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L110)

[returns (bytes memory nextProgressData)](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L119)

#### Mitigation

Either use the named return variables inplace of the return statement or remove them.

### 6- `constant` should be defined rather than using magic numbers :

It is best practice to use constant variables rather than hex/numeric literal values to make the code easier to understand and maintain. 

#### Impact - NON CRITICAL

#### Proof of Concept
Instances include:

File: contracts/crowdfund/Crowdfund.sol

[1e4](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L129-L135)

[1e4](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L370-L374)

File: contracts/party/PartyGovernance.sol

[1e4](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L280-L283)

[1e4](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1062)

[0.9999e4](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1066)

[1e4](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1078)

File: contracts/proposals/ArbitraryCallsProposal.sol

[32](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L163)

[68](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L208)

[36](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L213)

[68](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L216)

[68](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L226)

[36](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L231)

[68](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L234)

#### Mitigation
Replace the hex/numeric literals aforementioned with `constants`