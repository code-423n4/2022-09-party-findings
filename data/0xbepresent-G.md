1 - Internal functions only called once can be inlined to save gas
==

Not inlining costs more gas because of extra ```JUMP``` instructions and additional stack operations needed for function calls.

Instances of this issue:

```
contracts/party/PartyGovernance.sol#L848    function _getVotingPowerSnapshotAt(address voter, uint40 timestamp, uint256 hintIndex)
```

https://github.com/PartyDAO/party-contracts-c4/blob/d129d64779/contracts/party/PartyGovernance.sol#L848

