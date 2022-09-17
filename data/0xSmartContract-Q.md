### Non-Critical Issues List
| Number |Issues Details|
|:--:|:-------|
|[N-01]|Use a more recent version of solidity|
|[N-02]|Unused/Empty ```receive()``` / ```fallback()``` function|
|[N-03]|Function writing that does not comply with the 'Solidity Style Guide’|
|[N-04]|Use ```require``` instead of ```assert```|
|[N-05]|Include return parameters in NatSpec comments|
|[N-06]|```0``` address check|
|[N-07]|Long lines are not suitable for the 'Solidity Style Guide'|
|[N-08]|Undefined dependencies|
|[N-09]|For modern and more readable code; Update import usages|

Total 9 issues


### Low Risk Issues List
| Number |Issues Details|
|:--:|:-------|
|[L-01]|Floating pragma|
|[L-02]|Missing Event for critical parameters change|
|[L-03]|Add to blacklist function|
|[L-04]|No check if OnERC721Received is implemented|
|[L-05]|Missing Event for critical parameters change|
|[L-06]|Return value of transfer not checked|

Total 6 issues


## [N-01] Use a more recent version of solidity

**Context:**
All Contracts

**Description:**
Old version of Solidity is used (```^0.8.0```), newer version should be used.


## [N-02] Unused/Empty ```receive()``` / ```fallback()``` function

**Context:**
[AuctionCrowdfund.sol#L144](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L144)
[Party.sol#L47](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L47)


**Description:**
If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth)))


## [N-03] Function writing that does not comply with the 'Solidity Style Guide’

**Context:**
[Party.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol)
[TokenDistributor.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol)
[PartyGovernanceNFT.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol)
[CrowdfundNFT.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol)
[Globals.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol)

**Description:**
Order of Functions; ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this.

https://docs.soliditylang.org/en/v0.8.15/style-guide.html

Functions should be grouped according to their visibility and ordered:

- constructor
- receive function (if exists)
- fallback function (if exists)
- external
- public
- internal
- private
- within a grouping, place the view and pure functions last


## [N-04] Use ```require``` instead of ```assert```

**Context:**
[ReadOnlyDelegateCall.sol#L30](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L30)
[ProposalExecutionEngine.sol#L142](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L142)
[ListOnZoraProposal.sol#L120](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L120)
[ListOnOpenseaProposal.sol#L221](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L221)
[FractionalizeProposal.sol#L67](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/FractionalizeProposal.sol#L67)
[PartyGovernanceNFT.sol#L179](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L179)
[PartyGovernanceNFT.sol#L179](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L179)
[TokenDistributor.sol#L385](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L385)
[TokenDistributor.sol#L411](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L411)

**Description:**
Assert should not be used except for tests, require should be used.

Prior to Solidity 0.8.0, pressing a confirm consumes the remainder of the process's available gas instead of returning it, as request()/revert() did.

Assertion() should be avoided even after solidity version 0.8.0, because its documentation states "The Assert function generates an error of type Panic(uint256). … Code that works properly should never Panic, even on invalid external input. If this happens, you need to fix it in your contract. there's a mistake".


## [N-05] Include return parameters in NatSpec comments

**Context:**
All Contracts

**Description:**
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.

https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

**Recommendation:**
Include return parameters in NatSpec comments

_Recommendation  Code Style: (from Uniswap3)_
```js
    /// @notice Adds liquidity for the given recipient/tickLower/tickUpper position
    /// @dev The caller of this method receives a callback in the form of IUniswapV3MintCallback#uniswapV3MintCallback
    /// in which they must pay any token0 or token1 owed for the liquidity. The amount of token0/token1 due depends
    /// on tickLower, tickUpper, the amount of liquidity, and the current price.
    /// @param recipient The address for which the liquidity will be created
    /// @param tickLower The lower tick of the position in which to add liquidity
    /// @param tickUpper The upper tick of the position in which to add liquidity
    /// @param amount The amount of liquidity to mint
    /// @param data Any data that should be passed through to the callback
    /// @return amount0 The amount of token0 that was paid to mint the given amount of liquidity. Matches the value in the callback
    /// @return amount1 The amount of token1 that was paid to mint the given amount of liquidity. Matches the value in the callback
    function mint(
        address recipient,
        int24 tickLower,
        int24 tickUpper,
        uint128 amount,
        bytes calldata data
    ) external returns (uint256 amount0, uint256 amount1);
```


## [N-06] ```0``` address check

**Context:**
[Globals.sol#L23](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L23)
[Globals.sol#L27](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L27)
[PartyGovernance.sol#L451](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L451)
[PartyGovernance.sol#L783](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L783)
[PartyGovernance.sol#L973](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L973)
[Crowdfund.sol#L167](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L167)

**Description:**
Also check of the address to protect the code from 0x0 address  problem just in case. This is best practice or instead of suggesting that they verify address != 0x0, you could add some good natspec comments explaining what is valid and what is invalid and what are the implications of accidentally using an invalid address.

**Recommendation:**
like this ;
```if (_treasury == address(0)) revert ADDRESS_ZERO();```


## [N-07] Long lines are not suitable for the ‘Solidity Style Guide’

**Context:**
[ProposalExecutionEngine.sol#L73](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L73)
[CrowdfundFactory.sol#L18](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L18)
[Globals.sol#L71](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L71)
[Globals.sol#L75](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L75)
[Globals.sol#L79](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L79)

**Description:**
It is generally recommended that lines in the source code should not exceed 80-120 characters. Today's screens are much larger, so in some cases it makes sense to expand that. The lines above should be split when they reach that length, as the files will most likely be on GitHub and GitHub always uses a scrollbar when the length is more than 164 characters.

(why-is-80-characters-the-standard-limit-for-code-width)[https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width]

**Recommendation:**
Multiline output parameters and return statements should follow the same style recommended for wrapping long lines found in the Maximum Line Length section.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html#introduction

```js
thisFunctionCallIsReallyLong(
    longArgument1,
    longArgument2,
    longArgument3
);
```

## [N-08] Undefined dependencies

**Context:**
[PartyGovernanceNFT.sol#L6](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L6)

**Description:**
OpenZeppelin's dependency "openzeppelin/contracts/interfaces/IERC 2981.sol" is not defined in package.json

```js
package.json : 

 "devDependencies": {
        "@types/chai": "^4.3.1",
        "@types/mocha": "^9.1.1",
        "@types/sinon-chai": "^3.2.8",
        "@types/yargs": "^17.0.10",
        "chai": "^4.3.6",
        "ethereum-waffle": "^3.4.4",
        "ethers": "^5.6.9",
        "mocha": "^10.0.0",
        "typescript": "^4.6.4"
    },
    "dependencies": {
        "colors": "^1.4.0",
        "glob": "^7.1.6",
        "glob-promise": "^4.2.2",
        "yargs": "^17.5.1"
    }
```

**Recommendation:**
Add to OpenZeppelin dependencies in package.json


## [N-09] For modern and more readable code; Update import usages

**Context:**
All Contracts

**Description:**
Our Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct ```polluted the source code``` with an unnecessary object we were not using because we did not need it. This was breaking the rule of modularity and modular programming: ```only import what you need```Specific imports with curly braces allow us to apply this rule better.

**Recommendation:**
```import {contract1 , contract2} from "filename.sol";```


## [L-01] Floating pragma

**Description:**
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
https://swcregistry.io/docs/SWC-103

Floating Pragma List: 
pragma ^0.8.0.   (all contracts)

**Recommendation:**
Lock the pragma version and also consider known bugs (https://github.com/ethereum/solidity/releases) for the compiler version that is chosen.


## [L-02] Missing event for critical parameters change

**Context:**
[TokenDistributor.sol#L313](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L313)
[TokenDistributor.sol#L98](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L98)
[ProposalExecutionEngine.sol#L116](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L116)
[ListOnZoraProposal.sol#L78](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L78)
[PartyGovernanceNFT.sol#L169](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L169)

**Description:**
Events help non-contract tools to track changes, and events prevent users from being surprised by changes.

**Recommendation:**
Add ```event-emit```

## [L-03] Add to blacklist function

**Description:**
Cryptocurrency mixing service, Tornado Cash, has been blacklisted in the OFAC.
A lot of blockchain companies, token projects, NFT Projects have ```blacklisted``` all Ethereum addresses owned by Tornado Cash listed in the US Treasury Department's sanction against the protocol.
https://home.treasury.gov/policy-issues/financial-sanctions/recent-actions/20220808
In addition, these platforms even ban accounts that have received ETH on their account with Tornadocash.

Some of these Projects;
* USDC (https://www.circle.com/en/usdc)
* Flashbots (https://www.paradigm.xyz/portfolio/flashbots )
* Aave (https://aave.com/)
* Uniswap
* Balancer
* Infura
* Alchemy 
* Opensea
* dYdX 

Details on the subject;
https://twitter.com/bantg/status/1556712790894706688?s=20&t=HUTDTeLikUr6Dv9JdMF7AA

https://cryptopotato.com/defi-protocol-aave-bans-justin-sun-after-he-randomly-received-0-1-eth-from-tornado-cash/

For this reason, every project in the Ethereum network must have a blacklist function, this is a good method to avoid legal problems in the future, apart from the current need.

Transactions from the project by an account funded by Tonadocash or banned by OFAC can lead to legal problems.Especially American citizens may want to get addresses to the blacklist legally, this is not an obligation

If you think that such legal prohibitions should be made directly by validators, this may not be possible:
https://www.paradigm.xyz/2022/09/base-layer-neutrality

```The ban on Tornado Cash makes little sense, because in the end, no one can prevent people from using other mixer smart contracts, or forking the existing ones. It neither hinders cybercrime, nor privacy.```

Here is the most beautiful and close to the project example; Manifold

Manifold Contract
https://etherscan.io/address/0xe4e4003afe3765aca8149a82fc064c0b125b9e5a#code

```js
     modifier nonBlacklistRequired(address extension) {
         require(!_blacklistedExtensions.contains(extension), "Extension blacklisted");
         _;
     }
```
Recommended Mitigation Steps add to Blacklist function and modifier

## [L-04] No check if OnERC721Received is implemented

**Context:**
[Crowdfund.sol#L480](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L480)

**Description:**
When minting a NFT, the function does not check if a receiving contract implements onERC721Received().
Check if OnERC721Received is implemented.
If this is intended, ```safemint``` should be used instead of ```mint```


**Recommendation:**
Consider using ```_safeMint``` instead of ```_mint```
## [L-05] Insufficient tests

**Context:**
[Globals.sol](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol)

**Description:**
It is crucial to write tests with possibly 100% coverage for smart contract systems.
Contract of top in Context were found to be not included in the test cases.

**Recommendation:**
It is recommended to write proper test for all possible code flows and specially edge cases.

## [L-06] Return value of transfer not checked

**Context:**
[Crowdfund.sol#L301](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L301)
[Crowdfund.sol#L487](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L487)
[PartyGovernanceNFT.sol#L143](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol#L143)

**Description:**
When there’s a failure in transfer()/transferFrom() the function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment.

**Recommendation:**
Add return value checks.






