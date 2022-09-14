# 1. [G-1]  ++i  cost less gas compare i++ in for loops

File CollectionBuyCrowdfund.sol, function onlyHost, line 62:

    for (uint256 i; i < hosts.length; i++) {
       ...
    }

Becomes:

    for (uint256 i; i < hosts.length; ++i) {
        ...
    }

##### Instances include:

File PartyHelpers.sol, line 64, 85, 115

# 2. [G-2] Cache the length of arrays in the loops

File CollectionBuyCrowdfund.sol, function onlyHost, line 62:

    for (uint256 i; i < hosts.length; i++) {
       ...
    }

Becomes:

    uint256 lengthHosts = hosts.length;
    for (uint256 i; i < lengthHosts; ++i) {
       ...
    }

##### Instances include:

File Crowdfund.sol, line 180, 300
File PartyGovernance.sol, line 306
File ArbitraryCallsProposal.sol, line 52, 61, 78
File LibProposal.sol, line 14, 32
File ListOnOpenseaProposal.sol, line 291
File PartyHelpers.sol, line 64, 85
File ERC1155.sol, line 80, 118

# 3. [G-3] Using uncheck blocks to save gas - increments in for loop can be uncheck
Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block

 File CollectionBuyCrowdfund.sol, function onlyHost, line 62:

    for (uint256 i; i < hosts.length; i++) {
       ...
    }

Becomes:

    uint256 lengthHosts = hosts.length;
    for (uint256 i; i < lengthHosts;) {
          ...
          unchecked { ++i; }
      }

##### Instances include:

File Crowdfund.sol, line 180, 300, 348
File PartyGovernance.sol, line 306
File ArbitraryCallsProposal.sol, line 52, 61, 78
File LibProposal.sol, line 14, 32
File ListOnOpenseaProposal.sol, line 291
File PartyHelpers.sol, line 64, 85, 115
File ERC1155.sol, line 118
File Strings.sol, line 57

# 4. [G-4] Initialize uint256 i; is cheaper than uint256 i = 0

##### Instances include:

File Crowdfund.sol, line 180, 242, 300, 348
File PartyGovernance.sol, line 306
File ArbitraryCallsProposal.sol, line 52, 61, 78
File LibProposal.sol, line 14, 32
File ListOnOpenseaProposal.sol, line 291
File PartyHelpers.sol, line 64, 85, 115
File ERC1155.sol, line 80, 118, 168,  198




