## Gas Optimizations
17/09/2022 18:04
[Party DAO](https://github.com/code-423n4/2022-09-party)
**Tools Used:** Manual Analysis
* * *
### CollectionBuyCrowdfund.sol
**Modifiers only used once**
- **onlyHost:** line [60](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol?plain=1#L60).

*Suggestion*: in-line the modifiers per function.
***