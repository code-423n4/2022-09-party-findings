## Use `error` instead of `revert()` with string
[CrowdfundNFT.sol L29](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L29)

Errors are used thoroughly across the project so I would advise to keep consistency and save a little gas by also using an `error` in the `alwaysRevert()` modifier.

## Return parameter should be marked `@return` instead of `@param` in NatSpec
[ITokenDistributor.sol L61](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#L61)
[ITokenDistributor.sol L80](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#L80)
[ITokenDistributor.sol L95](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#L95)
[ITokenDistributor.sol L110](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/ITokenDistributor.sol#L110)

The return parameter should be marked with `@return` as defined by the [NatSpec format](https://docs.soliditylang.org/en/v0.8.16/natspec-format.html).