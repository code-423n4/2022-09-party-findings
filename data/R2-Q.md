## 1. No checks for 0-addresses
### Details
- governanceOpts.hosts
- globals
- initialContributor
- preciousToken

No checks if this is ``address(0)``
In some cases it can lead to wrong protocol logic


## 2. Use constant, don't repeat yourself
### Details
Use ``VETO_VALUE`` instead of ``uint96(int96(-1))`` in ``PartyGovernance._getProposalStatus()``