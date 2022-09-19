### State variables should not be initialized to their default values
Initializing `uint` variables to their default value of `0` is unnecessary and costs gas
___
[PartyGovernance.sol: L432](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L432)
```solidity
        uint256 low = 0;
```
Change to: 
```solidity
        uint256 low;
```
___
___


### Array length should not be looked up in every iteration of a `for` loop

Since calculating the array length costs gas, it's best to read the length of the array from memory before executing the loop
___
[CollectionBuyCrowdfund.sol: L62-67](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62-L67)
```solidity
        for (uint256 i; i < hosts.length; i++) {
            if (hosts[i] == msg.sender) {
                isHost = true;
                break;
            }
        }
```
Suggestion:
```solidity
        uint256 numHosts = hosts.length; 
        for (uint256 i; i < numHosts; i++) {
            if (hosts[i] == msg.sender) {
                isHost = true;
                break;
            }
        }
```
___
Similarly for the eleven additional `for` loops referenced below:

[ArbitraryCallsProposal.sol: L52-57](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52-L57)

[ArbitraryCallsProposal.sol: L61-74](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61-L74)

[ArbitraryCallsProposal.sol: L78-87](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L78-L87)

[TokenDistributor.sol: L230-232](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L230-L232)

[TokenDistributor.sol: L239-241](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L239-L241)

[ListOnOpenseaProposal.sol: L291-298](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291-L298)

[Crowdfund.sol: L180-182](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L180-L182)

[Crowdfund.sol: L300-302](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L300-L302)

[PartyGovernance.sol: L306-308](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306-L308)

[LibProposal.sol: L14-18](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14-L18)

[LibProposal.sol: L32-36](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L32-L36)
___
___


### Use `++i` instead of `i++` to increase count in a `for` loop
Since use of  `i++` (or equivalent counter) costs more gas, it is better to use `++i`
___
[CollectionBuyCrowdfund.sol: L62-67](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62-L67)
```solidity
        for (uint256 i; i < hosts.length; i++) {
            if (hosts[i] == msg.sender) {
                isHost = true;
                break;
            }
        }
```
Suggestion:
```solidity
        uint256 numHosts = hosts.length; 
        for (uint256 i; i < numHosts; ++i) {
            if (hosts[i] == msg.sender) {
                isHost = true;
                break;
            }
        }
```
```
___
___


### Avoid use of default "checked" behavior in a `for` loop
Underflow/overflow checks are made every time `++i` (or equivalent counter) is called. Such checks are unnecessary since `i` is already limited. Therefore, use `unchecked ++i` instead
___
[CollectionBuyCrowdfund.sol: L62-67](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62-L67)
```solidity
        for (uint256 i; i < hosts.length; i++) {
            if (hosts[i] == msg.sender) {
                isHost = true;
                break;
            }
        }
```
Suggestion:
```solidity
        uint256 numHosts = hosts.length; 
        for (uint256 i; i < numHosts;) {
            if (hosts[i] == msg.sender) {
                isHost = true;
                break;

                unchecked {
                  ++i;
              }
            }
        }
```
___
Similarly for the thirteen additional `for` loops referenced below:

[ArbitraryCallsProposal.sol: L52-57](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52-L57)

[ArbitraryCallsProposal.sol: L61-74](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61-L74)

[ArbitraryCallsProposal.sol: L78-87](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L78-L87)

[TokenDistributor.sol: L230-232](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L230-L232)

[TokenDistributor.sol: L239-241](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L239-L241)

[ListOnOpenseaProposal.sol: L291-298](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291-L298)

[Crowdfund.sol: L180-182](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L180-L182)

[Crowdfund.sol: L241-244](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L241-L244)

[Crowdfund.sol: L300-302](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L300-L302)

[Crowdfund.sol: L347-362](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L347-L362)

[PartyGovernance.sol: L306-308](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306-L308)

[LibProposal.sol: L14-18](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14-L18)

[LibProposal.sol: L32-36](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L32-L36)
___
___
