# Gas Optimizations

## Summary

|               | Issue         | Instances     |
| :-------------: |:-------------|:-------------:|
| 1      | Variables inside struct should be packed to save gas     | 2    |
| 2      | Usage of `uints/ints` smaller than 32 bytes (256 bits) incurs overhead  |  5 |
| 3      | Using `bool` for storage incurs overhead   |  5 |
| 4      | `array.length` should not be looked up in every loop in a for loop |  8 |
| 5      | It costs more gas to initialize variables to zero than to let the default of zero be applied    |  11  |
| 6      | `X += Y/X -= Y` costs more gas than `X = X + Y/X = X - Y` for state variables  |  14 |
| 7      | Use of `++i` cost less gas than `i++` in for loops    |  8  |
| 8      | ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as in the case when used in for & while loops |  8  |

## Findings

### 1- Variables inside `struct` should be packed to save gas :

As the solidity EVM works with 32 bytes, variables less than 32 bytes should be packed inside a struct so that they can be stored in the same slot, this saves gas when writing to storage.

There are 3 instances of this issue:

```
File: contracts/crowdfund/BuyCrowdfund.sol

19      struct BuyCrowdfundOptions {
            // The name of the crowdfund.
            // This will also carry over to the governance party.
            string name;
            // The token symbol for both the crowdfund and the governance NFTs.
            string symbol;
            // The ERC721 contract of the NFT being bought.
            IERC721 nftContract;
            // ID of the NFT being bought.
            uint256 nftTokenId;
            // How long this crowdfund has to bid on the NFT, in seconds.
            uint40 duration;
            // Maximum amount this crowdfund will pay for the NFT.
            // If zero, no maximum.
            uint96 maximumPrice;
            // An address that receives a portion of the final voting power
            // when the party transitions into governance.
            address payable splitRecipient;
            // What percentage (in bps) of the final total voting power `splitRecipient`
            // receives.
            uint16 splitBps;
            // If ETH is attached during deployment, it will be interpreted
            // as a contribution. This is who gets credit for that contribution.
            address initialContributor;
            // If there is an initial contribution, this is who they will delegate their
            // voting power to when the crowdfund transitions to governance.
            address initialDelegate;
            // The gatekeeper contract to use (if non-null) to restrict who can
            // contribute to this crowdfund.
            IGateKeeper gateKeeper;
            // The gate ID within the gateKeeper contract to use.
            bytes12 gateKeeperId;
            // Fixed governance options (i.e. cannot be changed) that the governance
            // `Party` will be created with if the crowdfund succeeds.
            FixedGovernanceOpts governanceOpts;
        }
```

This should be rearranged as follow to save gas :

```
struct BuyCrowdfundOptions {
    // The name of the crowdfund.
    // This will also carry over to the governance party.
    string name;
    // The token symbol for both the crowdfund and the governance NFTs.
    string symbol;
    // The ERC721 contract of the NFT being bought.
    IERC721 nftContract;
    // ID of the NFT being bought.
    uint256 nftTokenId;
    // How long this crowdfund has to bid on the NFT, in seconds.
    uint40 duration;
    // Maximum amount this crowdfund will pay for the NFT.
    // If zero, no maximum.
    uint96 maximumPrice;
    // What percentage (in bps) of the final total voting power `splitRecipient`
    // receives.
    uint16 splitBps;
    // An address that receives a portion of the final voting power
    // when the party transitions into governance.
    address payable splitRecipient;
    // If ETH is attached during deployment, it will be interpreted
    // as a contribution. This is who gets credit for that contribution.
    address initialContributor;
    // If there is an initial contribution, this is who they will delegate their
    // voting power to when the crowdfund transitions to governance.
    address initialDelegate;
    // The gatekeeper contract to use (if non-null) to restrict who can
    // contribute to this crowdfund.
    IGateKeeper gateKeeper;
    // The gate ID within the gateKeeper contract to use.
    bytes12 gateKeeperId;
    // Fixed governance options (i.e. cannot be changed) that the governance
    // `Party` will be created with if the crowdfund succeeds.
    FixedGovernanceOpts governanceOpts;
}
```

The second instance :

```
File: contracts/crowdfund/BuyCrowdfundBase.sol

20      struct BuyCrowdfundBaseOptions {
            // The name of the crowdfund.
            // This will also carry over to the governance party.
            string name;
            // The token symbol for both the crowdfund and the governance NFTs.
            string symbol;
            // How long this crowdfund has to bid on the NFT, in seconds.
            uint40 duration;
            // Maximum amount this crowdfund will pay for the NFT.
            // If zero, no maximum.
            uint96 maximumPrice;
            // An address that receieves an extra share of the final voting power
            // when the party transitions into governance.
            address payable splitRecipient;
            // What percentage (in bps) of the final total voting power `splitRecipient`
            // receives.
            uint16 splitBps;
            // If ETH is attached during deployment, it will be interpreted
            // as a contribution. This is who gets credit for that contribution.
            address initialContributor;
            // If there is an initial contribution, this is who they will delegate their
            // voting power to when the crowdfund transitions to governance.
            address initialDelegate;
            // The gatekeeper contract to use (if non-null) to restrict who can
            // contribute to this crowdfund.
            IGateKeeper gateKeeper;
            // The gatekeeper contract to use (if non-null).
            bytes12 gateKeeperId;
            // Governance options.
            FixedGovernanceOpts governanceOpts;
        }
```

This should be rearranged as follow to save gas :

```
struct BuyCrowdfundBaseOptions {
    // The name of the crowdfund.
    // This will also carry over to the governance party.
    string name;
    // The token symbol for both the crowdfund and the governance NFTs.
    string symbol;
    // How long this crowdfund has to bid on the NFT, in seconds.
    uint40 duration;
    // Maximum amount this crowdfund will pay for the NFT.
    // If zero, no maximum.
    uint96 maximumPrice;
    // What percentage (in bps) of the final total voting power `splitRecipient`
    // receives.
    uint16 splitBps;
    // An address that receieves an extra share of the final voting power
    // when the party transitions into governance.
    address payable splitRecipient;
    // If ETH is attached during deployment, it will be interpreted
    // as a contribution. This is who gets credit for that contribution.
    address initialContributor;
    // If there is an initial contribution, this is who they will delegate their
    // voting power to when the crowdfund transitions to governance.
    address initialDelegate;
    // The gatekeeper contract to use (if non-null) to restrict who can
    // contribute to this crowdfund.
    IGateKeeper gateKeeper;
    // The gatekeeper contract to use (if non-null).
    bytes12 gateKeeperId;
    // Governance options.
    FixedGovernanceOpts governanceOpts;
}
```

The third instance :

```
File: contracts/crowdfund/CollectionBuyCrowdfund.sol

20      struct CollectionBuyCrowdfundOptions {
            // The name of the crowdfund.
            // This will also carry over to the governance party.
            string name;
            // The token symbol for both the crowdfund and the governance NFTs.
            string symbol;
            // The ERC721 contract of the NFT being bought.
            IERC721 nftContract;
            // How long this crowdfund has to bid on the NFT, in seconds.
            uint40 duration;
            // Maximum amount this crowdfund will pay for the NFT.
            // If zero, no maximum.
            uint96 maximumPrice;
            // An address that receives a portion of the final voting power
            // when the party transitions into governance.
            address payable splitRecipient;
            // What percentage (in bps) of the final total voting power `splitRecipient`
            // receives.
            uint16 splitBps;
            // If ETH is attached during deployment, it will be interpreted
            // as a contribution. This is who gets credit for that contribution.
            address initialContributor;
            // If there is an initial contribution, this is who they will delegate their
            // voting power to when the crowdfund transitions to governance.
            address initialDelegate;
            // The gatekeeper contract to use (if non-null) to restrict who can
            // contribute to this crowdfund.
            IGateKeeper gateKeeper;
            // The gate ID within the gateKeeper contract to use.
            bytes12 gateKeeperId;
            // Fixed governance options (i.e. cannot be changed) that the governance
            // `Party` will be created with if the crowdfund succeeds.
            FixedGovernanceOpts governanceOpts;
        }
```

This should be rearranged as follow to save gas :

```
struct CollectionBuyCrowdfundOptions {
        // The name of the crowdfund.
        // This will also carry over to the governance party.
        string name;
        // The token symbol for both the crowdfund and the governance NFTs.
        string symbol;
        // The ERC721 contract of the NFT being bought.
        IERC721 nftContract;
        // How long this crowdfund has to bid on the NFT, in seconds.
        uint40 duration;
        // Maximum amount this crowdfund will pay for the NFT.
        // If zero, no maximum.
        uint96 maximumPrice;
        // What percentage (in bps) of the final total voting power `splitRecipient`
        // receives.
        uint16 splitBps;
        // An address that receives a portion of the final voting power
        // when the party transitions into governance.
        address payable splitRecipient;
        // If ETH is attached during deployment, it will be interpreted
        // as a contribution. This is who gets credit for that contribution.
        address initialContributor;
        // If there is an initial contribution, this is who they will delegate their
        // voting power to when the crowdfund transitions to governance.
        address initialDelegate;
        // The gatekeeper contract to use (if non-null) to restrict who can
        // contribute to this crowdfund.
        IGateKeeper gateKeeper;
        // The gate ID within the gateKeeper contract to use.
        bytes12 gateKeeperId;
        // Fixed governance options (i.e. cannot be changed) that the governance
        // `Party` will be created with if the crowdfund succeeds.
        FixedGovernanceOpts governanceOpts;
    }
```

### 2- Usage of `uints/ints` smaller than 32 bytes (256 bits) incurs overhead :

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size as you can check [here](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html).

So use `uint256`/`int256` for state variables and then downcast to lower sizes where needed.

There are 6 instances of this issue:

```
File: contracts/crowdfund/BuyCrowdfundBase.sol

60       uint40 public expiry;
62       uint96 public maximumPrice;
64       uint96 public settledPrice;

File: contracts/crowdfund/Crowdfund.sol

93       uint96 public totalContributions;
104      uint16 public splitBps;

File: contracts/party/PartyGovernance.sol

199      uint16 public feeBps;
```

### 3- Using `bool` for storage incurs overhead  :

Use `uint256(1)` and `uint256(2)` instead of `true/false` to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from `false` to `true`, after having been `true` in the past

There are 3 instances of this issue:

```
File: contracts/crowdfund/Crowdfund.sol

106      bool private _splitRecipientHasBurned;

File: contracts/distribution/TokenDistributor.sol

62       bool public emergencyActionsDisabled;

File: contracts/party/PartyGovernance.sol

197      bool public emergencyExecuteDisabled;
```

### 4- `array.length` should not be looked up in every loop in a for loop :

There are 9 instances of this issue:

```
File: contracts/crowdfund/CollectionBuyCrowdfund.sol

62      for (uint256 i; i < hosts.length; i++)

File: contracts/crowdfund/Crowdfund.sol

180     for (uint256 i = 0; i < contributors.length; ++i)
300     for (uint256 i = 0; i < preciousTokens.length; ++i)

File: contracts/distribution/TokenDistributor.sol

230     for (uint256 i = 0; i < infos.length; ++i)
239     for (uint256 i = 0; i < infos.length; ++i)

File: contracts/party/PartyGovernance.sol

306     for (uint256 i=0; i < opts.hosts.length; ++i)

File: contracts/proposals/ArbitraryCallsProposal.sol

52      for (uint256 i = 0; i < hadPreciouses.length; ++i)
61      for (uint256 i = 0; i < calls.length; ++i)
78      for (uint256 i = 0; i < hadPreciouses.length; ++i)
```

### 5- Use of ++i cost less gas than i++/i=i+1 in for loops :

There is 1 instances of this issue:

```
File: contracts/crowdfund/CollectionBuyCrowdfund.sol

62      for (uint256 i; i < hosts.length; i++)
```

### 6- ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as in the case when used in for & while loops :

There are 11 instances of this issue:

```
File: contracts/crowdfund/CollectionBuyCrowdfund.sol

62      for (uint256 i; i < hosts.length; i++)

File: contracts/crowdfund/Crowdfund.sol

180     for (uint256 i = 0; i < contributors.length; ++i)
242     for (uint256 i = 0; i < numContributions; ++i)
300     for (uint256 i = 0; i < preciousTokens.length; ++i)
348     for (uint256 i = 0; i < numContributions; ++i)

File: contracts/distribution/TokenDistributor.sol

230     for (uint256 i = 0; i < infos.length; ++i)
239     for (uint256 i = 0; i < infos.length; ++i)

File: contracts/party/PartyGovernance.sol

306     for (uint256 i=0; i < opts.hosts.length; ++i)

File: contracts/proposals/ArbitraryCallsProposal.sol

52      for (uint256 i = 0; i < hadPreciouses.length; ++i)
61      for (uint256 i = 0; i < calls.length; ++i)
78      for (uint256 i = 0; i < hadPreciouses.length; ++i)
```

### 7- It costs more gas to initialize variables to zero than to let the default of zero be applied :

If a variable is not set/initialized, it is assumed to have the default value (0 for uint or int, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

There are 10 instances of this issue:

```
File: contracts/crowdfund/Crowdfund.sol

180     for (uint256 i = 0; i < contributors.length; ++i)
242     for (uint256 i = 0; i < numContributions; ++i)
300     for (uint256 i = 0; i < preciousTokens.length; ++i)
348     for (uint256 i = 0; i < numContributions; ++i)

File: contracts/distribution/TokenDistributor.sol

230     for (uint256 i = 0; i < infos.length; ++i)
239     for (uint256 i = 0; i < infos.length; ++i)

File: contracts/party/PartyGovernance.sol

306     for (uint256 i=0; i < opts.hosts.length; ++i)

File: contracts/proposals/ArbitraryCallsProposal.sol

52      for (uint256 i = 0; i < hadPreciouses.length; ++i)
61      for (uint256 i = 0; i < calls.length; ++i)
78      for (uint256 i = 0; i < hadPreciouses.length; ++i)
```

### 8- `X += Y/X -= Y` costs more gas than `X = X + Y/X = X - Y` for state variables :

There is 1 instance of this issue:

```
File: contracts/crowdfund/Crowdfund.sol

411     totalContributions += amount;

File: contracts/distribution/TokenDistributor.sol

381     _storedBalances[balanceId] -= amount;
```