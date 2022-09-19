**Use Solidity Custom Errors instead of `require()` statements with string error**

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/market-wrapper/KoansMarketWrapper.sol#L67](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/market-wrapper/KoansMarketWrapper.sol#L67)

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/market-wrapper/KoansMarketWrapper.sol#L92](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/market-wrapper/KoansMarketWrapper.sol#L92)

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/market-wrapper/NounsMarketWrapper.sol#L65](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/market-wrapper/NounsMarketWrapper.sol#L65)

[https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/market-wrapper/NounsMarketWrapper.sol#L90](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/market-wrapper/NounsMarketWrapper.sol#L90)

Replace the linked `require()` statements that have string error message with if statements + a specific custom error to save gas.