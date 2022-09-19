## TABLE OF CONTENTS

- [G-01] No need to explicitly initialize variables with default value
- [G-02] Using abi.encode() is less efficient than abi.encodePacked()

## [G-01] No need to explicitly initialize variables with default value

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas. In the similar code above, the initialization of uint256 i = 0 can be simplified to uint256 i

File: ListOnOpenseaProposal.sol [Line 291](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291)
ArbitraryCallsProposal.sol 
[Line 52](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52)
[Line 61](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61)
[Line 78](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61)

```
 for (uint256 i = 0; i < fees.length; ++i) {
                cons = orderParams.consideration[1 + i];
                cons.itemType = IOpenseaExchange.ItemType.NATIVE;
                cons.token = address(0);
                cons.identifierOrCriteria = 0;
                cons.startAmount = cons.endAmount = fees[i];
                cons.recipient = feeRecipients[i];
            }
```
can become
```
 for (uint i; i < fees.length; ++i) {
                cons = orderParams.consideration[1 + i];
                cons.itemType = IOpenseaExchange.ItemType.NATIVE;
                cons.token = address(0);
                cons.identifierOrCriteria = 0;
                cons.startAmount = cons.endAmount = fees[i];
                cons.recipient = feeRecipients[i];
            }
```

## [G-02]  Using abi.encode() is less efficient than abi.encodePacked()

https://ethereum.stackexchange.com/questions/119583/when-to-use-abi-encode-abi-encodepacked-or-abi-encodewithsignature-in-solidity

File: ListOnZoraProposal.sol [Line 115](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L115)
File: ListOnOpenseaProposal.sol [Line 164](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L164), [Line 219](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L219)