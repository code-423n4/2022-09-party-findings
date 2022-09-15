# gas optimization

### Don't Initialize Variables with Default Value

### Examples: 
```
party-contracts-c4\contracts\crowdfund\Crowdfund.sol::180 => for (uint256 i = 0; i < contributors.length; ++i) {
party-contracts-c4\contracts\crowdfund\Crowdfund.sol::242 => for (uint256 i = 0; i < numContributions; ++i) {
party-contracts-c4\contracts\crowdfund\Crowdfund.sol::300 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4\contracts\crowdfund\Crowdfund.sol::348 => for (uint256 i = 0; i < numContributions; ++i) {
party-contracts-c4\contracts\distribution\TokenDistributor.sol::230 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4\contracts\distribution\TokenDistributor.sol::239 => for (uint256 i = 0; i < infos.length; ++i) {
```

### Cache Array Length Outside of Loop

### Examples: 
```
party-contracts-c4\contracts\crowdfund\CollectionBuyCrowdfund.sol::62 => for (uint256 i; i < hosts.length; i++) {
party-contracts-c4\contracts\crowdfund\Crowdfund.sol::180 => for (uint256 i = 0; i < contributors.length; ++i) {
party-contracts-c4\contracts\crowdfund\Crowdfund.sol::241 => uint256 numContributions = contributions.length;
party-contracts-c4\contracts\crowdfund\Crowdfund.sol::300 => for (uint256 i = 0; i < preciousTokens.length; ++i) {
party-contracts-c4\contracts\crowdfund\Crowdfund.sol::347 => uint256 numContributions = contributions.length;
party-contracts-c4\contracts\crowdfund\Crowdfund.sol::422 => uint256 numContributions = contributions.length;
party-contracts-c4\contracts\distribution\TokenDistributor.sol::229 => amountsClaimed = new uint128[](infos.length);
party-contracts-c4\contracts\distribution\TokenDistributor.sol::230 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4\contracts\distribution\TokenDistributor.sol::239 => for (uint256 i = 0; i < infos.length; ++i) {
party-contracts-c4\contracts\party\PartyGovernance.sol::306 => for (uint256 i=0; i < opts.hosts.length; ++i) {
party-contracts-c4\contracts\party\PartyGovernance.sol::431 => uint256 high = snaps.length;
```

### Use != 0 instead of > 0 for Unsigned Integer Comparison

### Examples
```
party-contracts-c4\contracts\crowdfund\Crowdfund.sol::144 => if (initialBalance > 0) {
party-contracts-c4\contracts\crowdfund\Crowdfund.sol::471 => if (votingPower > 0) {
```


### Use Prefix Increment instead of Postfix Increment if possible


### Examples
```
party-contracts-c4\contracts\crowdfund\CollectionBuyCrowdfund.sol::62 => for (uint256 i; i < hosts.length; i++) {
```

### Use immutable for OpenZeppelin AccessControl's Roles Declarations

### Examples
```
party-contracts-c4\contracts\crowdfund\Crowdfund.sol::331 => mstore(opts, keccak256(add(mload(opts), 0x20), mul(mload(mload(opts)), 32)))
party-contracts-c4\contracts\crowdfund\Crowdfund.sol::333 => h := keccak256(opts, 0xC0)
party-contracts-c4\contracts\distribution\TokenDistributor.sol::397 => keccak256(info, 0xe0),
party-contracts-c4\contracts\party\PartyGovernance.sol::400 => // keccak256(abi.encode(
party-contracts-c4\contracts\party\PartyGovernance.sol::403 => //   keccak256(proposal.proposalData)
party-contracts-c4\contracts\party\PartyGovernance.sol::405 => bytes32 dataHash = keccak256(proposal.proposalData);
party-contracts-c4\contracts\party\PartyGovernance.sol::412 => proposalHash := keccak256(proposal, 0x60)
party-contracts-c4\contracts\party\PartyGovernance.sol::1114 => mstore(0x00, keccak256(
party-contracts-c4\contracts\party\PartyGovernance.sol::1118 => mstore(0x20, keccak256(
party-contracts-c4\contracts\party\PartyGovernance.sol::1122 => h := keccak256(0x00, 0x40)
```


### Use Shift Right/Left instead of Division/Multiplication if possible

### Examples
```
party-contracts-c4\contracts\party\PartyGovernance.sol::434 => uint256 mid = (low + high) / 2;
```

