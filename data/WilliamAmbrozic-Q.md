## QA (low / non-critical)
18/09/2022 15:25
[Party DAO](https://github.com/code-423n4/2022-09-party)
* * *

### Floating Pragma
- Contracts in scope do not lock the pragma.

Example: [Line 2](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol?plain=1#L2) of TokenDistributor.sol.

[reference](https://swcregistry.io/docs/SWC-103)
* * *