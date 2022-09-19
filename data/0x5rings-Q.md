Findings:  require()/revert() strings longer than 32 bytes cost extra gas

Code: contracts/KoansMarketWrapper.sol - line 67, line 92
Contracts/NounsMarketWrapper.sol - line 65, line 90

Explanation:

-------------------

Findings:  REQUIRE() SHOULD BE USED INSTEAD OF ASSERT()

Code: contracts/CrowdfundNFT.sol - line 166
Contracts/TokenDistributor.sol - line 385, line 411
Contracts/ListOnOpenseaProposal.sol - line 221


Explanation:

Prior to solidity version 0.8.0, hitting an assert consumes the remainder of the transaction’s available gas rather than returning it, as require()/revert() do. assert() should be avoided even past solidity version 0.8.0 as its documentation states that “The assert function creates an error of type Panic(uint256). … Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix”.

Mitigation: Use of Custom Errors or require

-------------------

Findings:  _SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE

Code: contracts/Crowdfund.sol - line 451
Contracts/PartyGovernance.sol - line 132

Explanation: _mint() is discouraged in favour of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both OpenZeppelin and solmate have versions of this function


-------------------

Findings: FUNCTION MISSING COMMENTS

Code: Some functions are missing comments. Can lead to confusion for future devs. / readability.

Explanation: contracts/Crowdfund.sol - line 377, line 443, line 490
Contracts/CrowdfundFactory.sol - line 106
Contracts/crowdfundNFT.sol - line 136, line 149, line 159

(More instances than can count)

-------------------

Findings: Use of Block.timestamp

Code: contract/crowdfund/BuyCrowdfundBase.sol - line  73, line 159

Explanation: Block timestamps should not be used to be the deciding factor whether the _expiry ends. Miners have the ability to adjust timestamps, rearrange tx orders. Which can be dangerous / allows a user to game the system and _Crowdfund.

Mitigation: Use of Block.number, whereby miners are unable to easily manipulate the block number.

-------------------

Findings: UNUSED/EMPTY RECEIVE()/FALLBACK() FUNCTION

Code: AuctionCrowdfund.sol - line 144, 

Explanation:  Empty blocks should be removed or emit something / fallback or receive function

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth)))

-------------------

Findings:  USE OF FLOATING PRAGMA
Code: contracts * - (contract versions)

Explanation: Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

-------------------
