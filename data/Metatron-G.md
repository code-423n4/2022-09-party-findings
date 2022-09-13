I've sorted the issues from the most helpful (in terms of gas saving) to the minor

### [G-01] An arrays length should be cached to save gas in for-loops

An array’s length should be cached to save gas in for-loops
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset).
Caching the array length in the stack saves around **3 gas per iteration**.

I've found 12 locations in 7 files:

```
contracts/crowdfund/CollectionBuyCrowdfund.sol:
  61          bool isHost;
  62:         for (uint256 i; i < hosts.length; i++) {
  63              if (hosts[i] == msg.sender) {

contracts/crowdfund/Crowdfund.sol:
  179          CrowdfundLifecycle lc = getCrowdfundLifecycle();
  180:         for (uint256 i = 0; i < contributors.length; ++i) {
  181              _burn(contributors[i], lc, party_);

  299          // Transfer the acquired NFTs to the new party.
  300:         for (uint256 i = 0; i < preciousTokens.length; ++i) {
  301              preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);

contracts/distribution/TokenDistributor.sol:
  229          amountsClaimed = new uint128[](infos.length);
  230:         for (uint256 i = 0; i < infos.length; ++i) {
  231              amountsClaimed[i] = claim(infos[i], partyTokenIds[i]);

  238      {
  239:         for (uint256 i = 0; i < infos.length; ++i) {
  240              claimFee(infos[i], recipients[i]);

contracts/party/PartyGovernance.sol:
  305          // Set the party hosts.
  306:         for (uint256 i=0; i < opts.hosts.length; ++i) {
  307              isHost[opts.hosts[i]] = true;

contracts/proposals/ArbitraryCallsProposal.sol:
  51          if (!isUnanimous) {
  52:             for (uint256 i = 0; i < hadPreciouses.length; ++i) {
  53                  hadPreciouses[i] = _getHasPrecious(

  60          uint256 ethAvailable = msg.value;
  61:         for (uint256 i = 0; i < calls.length; ++i) {
  62              // Execute an arbitrary call.

  77          if (!isUnanimous) {
  78:             for (uint256 i = 0; i < hadPreciouses.length; ++i) {
  79                  if (hadPreciouses[i]) {

contracts/proposals/LibProposal.sol:
  13      {
  14:         for (uint256 i = 0; i < preciousTokens.length; ++i) {
  15              if (token == preciousTokens[i]) {

  31      {
  32:         for (uint256 i = 0; i < preciousTokens.length; ++i) {
  33              if (token == preciousTokens[i] && tokenId == preciousTokenIds[i]) {

contracts/proposals/ListOnOpenseaProposal.sol:
  290              cons.recipient = payable(address(this));
  291:             for (uint256 i = 0; i < fees.length; ++i) {
  292                  cons = orderParams.consideration[1 + i];
```

---------------------------------------------------------------------------

### [G-02] Using default values is cheaper than assignment

If a variable is not set/initialized, it is assumed to have the default value 0 for uint, and false for boolean.
Explicitly initializing it with its default value is an anti-pattern and wastes gas.
For example: ```uint8 i = 0;``` should be replaced with ```uint8 i;```

I've found 14 locations in 6 files:

```
contracts/crowdfund/Crowdfund.sol:
  179          CrowdfundLifecycle lc = getCrowdfundLifecycle();
  180:         for (uint256 i = 0; i < contributors.length; ++i) {
  181              _burn(contributors[i], lc, party_);

  241          uint256 numContributions = contributions.length;
  242:         for (uint256 i = 0; i < numContributions; ++i) {
  243              ethContributed += contributions[i].amount;

  299          // Transfer the acquired NFTs to the new party.
  300:         for (uint256 i = 0; i < preciousTokens.length; ++i) {
  301              preciousTokens[i].transferFrom(address(this), address(party_), preciousTokenIds[i]);

  347              uint256 numContributions = contributions.length;
  348:             for (uint256 i = 0; i < numContributions; ++i) {
  349                  Contribution memory c = contributions[i];

contracts/distribution/TokenDistributor.sol:
  229          amountsClaimed = new uint128[](infos.length);
  230:         for (uint256 i = 0; i < infos.length; ++i) {
  231              amountsClaimed[i] = claim(infos[i], partyTokenIds[i]);

  238      {
  239:         for (uint256 i = 0; i < infos.length; ++i) {
  240              claimFee(infos[i], recipients[i]);

contracts/party/PartyGovernance.sol:
  305          // Set the party hosts.
  306:         for (uint256 i=0; i < opts.hosts.length; ++i) {
  307              isHost[opts.hosts[i]] = true;

  431          uint256 high = snaps.length;
  432:         uint256 low = 0;
  433          while (low < high) {

contracts/proposals/ArbitraryCallsProposal.sol:
  51          if (!isUnanimous) {
  52:             for (uint256 i = 0; i < hadPreciouses.length; ++i) {
  53                  hadPreciouses[i] = _getHasPrecious(

  60          uint256 ethAvailable = msg.value;
  61:         for (uint256 i = 0; i < calls.length; ++i) {
  62              // Execute an arbitrary call.

  77          if (!isUnanimous) {
  78:             for (uint256 i = 0; i < hadPreciouses.length; ++i) {
  79                  if (hadPreciouses[i]) {

contracts/proposals/LibProposal.sol:
  13      {
  14:         for (uint256 i = 0; i < preciousTokens.length; ++i) {
  15              if (token == preciousTokens[i]) {

  31      {
  32:         for (uint256 i = 0; i < preciousTokens.length; ++i) {
  33              if (token == preciousTokens[i] && tokenId == preciousTokenIds[i]) {

contracts/proposals/ListOnOpenseaProposal.sol:
  290              cons.recipient = payable(address(this));
  291:             for (uint256 i = 0; i < fees.length; ++i) {
  292                  cons = orderParams.consideration[1 + i];
```

---------------------------------------------------------------------------

### [G-03] Upgrade pragma to 0.8.16 to save gas

Across the whole solution, the declared pragma is 0.8.
Upgrading to 0.8.16 has many benefits, cheaper on gas is one of them.
The following information is regarding 0.8.15 over 0.8.14. Assume that over 0.8 the save is higher!
Note that for 0.8.16 even more small gas saving applies.
```
According to the release note of 0.8.15: https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/
The benchmark shows saving of 2.5-10% Bytecode size,
Saving 2-8% Deployment gas,
And saving up to 6.2% Runtime gas.
```

---------------------------------------------------------------------------

### [G-04] != 0 is cheaper than > 0

!= 0 costs less gas compared to > 0 for unsigned integers even when optimizer enabled
All of the following findings are uint - so >0 and != have exactly the same effect.
** saves 6 gas ** each

I've found 2 locations in 1 file:

```
contracts/crowdfund/Crowdfund.sol:
  143          uint96 initialBalance = address(this).balance.safeCastUint256ToUint96();
  144:         if (initialBalance > 0) {
  145              // If this contract has ETH, either passed in during deployment or

  470              _getFinalContribution(contributor);
  471:         if (votingPower > 0) {
  472              // Get the address to delegate voting power to. If null, delegate to self.

```

---------------------------------------------------------------------------

### [G-05] ++i costs less gas compared to i++

* Most places are using the ++i pattern - good job!

++i costs **about 5 gas less per iteration** compared to i++ for unsigned integer.
This statement is true even with the optimizer enabled.
Summarized my results where i is used in a loop, is unsigned integer, and you safely can be changed to ++i without changing any behavior,

I've found 1 location to improve only:

```
contracts/crowdfund/CollectionBuyCrowdfund.sol:
  61          bool isHost;
  62:         for (uint256 i; i < hosts.length; i++) {
  63              if (hosts[i] == msg.sender) {
```

---------------------------------------------------------------------------

### [G-06] Custom errors save gas

* Most places across the solution are already using custom errors - good job!

From solidity 0.8.4 and above (although across the solution pragma 0.8^ is declared)
Custom errors from are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)
Source: https://blog.soliditylang.org/2021/04/21/custom-errors/:
```Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.```

I've found 3 locations in 2 files:

```
contracts/proposals/ProposalExecutionEngine.sol:
  245          }
  246:         require(proposalType != ProposalType.Invalid);
  247:         require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));
  248      }

contracts/utils/ReadOnlyDelegateCall.sol:
  19          // Attempt to gate to only `_readOnlyDelegateCall()` invocations.
  20:         require(msg.sender == address(this));
  21          (bool s, bytes memory r) = impl.delegatecall(callData);
```

