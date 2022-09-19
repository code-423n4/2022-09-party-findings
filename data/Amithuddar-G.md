1)++i costs less gas than i++, especially when it’s used in for-loops (--i/i-- too)

Saves 6 gas PER LOOP

File: party-contracts-c4\contracts\crowdfund\CollectionBuyCrowdfund.sol
  62,43:         for (uint256 i; i < hosts.length; i++) {
  
2)<ARRAY>.LENGTH SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP
The overheads outlined below are PER LOOP, excluding the first loop

storage arrays incur a Gwarmaccess (100 gas)
memory arrays use MLOAD (3 gas)
calldata arrays use CALLDATALOAD (3 gas)
Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset
   
File: party-contracts-c4\contracts\crowdfund\CollectionBuyCrowdfund.sol
  62,43:         for (uint256 i; i < hosts.length; i++) {
  
File: party-contracts-c4\contracts\crowdfund\Crowdfund.sol
  180,14:         for (uint256 i = 0; i < contributors.length; ++i) {
  300,14:         for (uint256 i = 0; i < preciousTokens.length; ++i) {        

File: party-contracts-c4\contracts\distribution\TokenDistributor.sol
  230,14:         for (uint256 i = 0; i < infos.length; ++i) {
  239,14:         for (uint256 i = 0; i < infos.length; ++i) {

File:   52,18:             for (uint256 i = 0; i < hadPreciouses.length; ++i) {
  61,14:         for (uint256 i = 0; i < calls.length; ++i) {
  78,18:             for (uint256 i = 0; i < hadPreciouses.length; ++i) {

File: party-contracts-c4\contracts\proposals\LibProposal.sol
  14,14:         for (uint256 i = 0; i < preciousTokens.length; ++i) {
  32,14:         for (uint256 i = 0; i < preciousTokens.length; ++i) {  

File: party-contracts-c4\contracts\proposals\ListOnOpenseaProposal.sol
  291,18:             for (uint256 i = 0; i < fees.length; ++i) {
  
    
3) ++i/i++ should be unchecked{++i}/unchecked{++i} when it is not possible for them to overflow,
as is the case when used in for- and while-loops    

File: party-contracts-c4\contracts\crowdfund\CollectionBuyCrowdfund.sol
  62,43:         for (uint256 i; i < hosts.length; i++) {
  
File: party-contracts-c4\contracts\crowdfund\Crowdfund.sol
  180,14:         for (uint256 i = 0; i < contributors.length; ++i) {
  242,14:         for (uint256 i = 0; i < numContributions; ++i) {
  300,14:         for (uint256 i = 0; i < preciousTokens.length; ++i) {
  348,18:             for (uint256 i = 0; i < numContributions; ++i) {          

File: party-contracts-c4\contracts\distribution\TokenDistributor.sol
  230,14:         for (uint256 i = 0; i < infos.length; ++i) {
  239,14:         for (uint256 i = 0; i < infos.length; ++i) {

File:   52,18:             for (uint256 i = 0; i < hadPreciouses.length; ++i) {
  61,14:         for (uint256 i = 0; i < calls.length; ++i) {
  78,18:             for (uint256 i = 0; i < hadPreciouses.length; ++i) {

File: party-contracts-c4\contracts\proposals\LibProposal.sol
  14,14:         for (uint256 i = 0; i < preciousTokens.length; ++i) {
  32,14:         for (uint256 i = 0; i < preciousTokens.length; ++i) {  

File: party-contracts-c4\contracts\proposals\ListOnOpenseaProposal.sol
  291,18:             for (uint256 i = 0; i < fees.length; ++i) {

  


4)It costs more gas to initialize variables to zero than to let the default of zero be applied

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for (uint256 i = 0; i < numIterations; ++i) { should be replaced with for (uint256 i; i < numIterations; ++i) {  

File: party-contracts-c4\contracts\crowdfund\Crowdfund.sol
  180,14:         for (uint256 i = 0; i < contributors.length; ++i) {
  242,14:         for (uint256 i = 0; i < numContributions; ++i) {
  300,14:         for (uint256 i = 0; i < preciousTokens.length; ++i) {
  348,18:             for (uint256 i = 0; i < numContributions; ++i) {          

File: party-contracts-c4\contracts\distribution\TokenDistributor.sol
  230,14:         for (uint256 i = 0; i < infos.length; ++i) {
  239,14:         for (uint256 i = 0; i < infos.length; ++i) {

File:   52,18:             for (uint256 i = 0; i < hadPreciouses.length; ++i) {
  61,14:         for (uint256 i = 0; i < calls.length; ++i) {
  78,18:             for (uint256 i = 0; i < hadPreciouses.length; ++i) {

File: party-contracts-c4\contracts\proposals\LibProposal.sol
  14,14:         for (uint256 i = 0; i < preciousTokens.length; ++i) {
  32,14:         for (uint256 i = 0; i < preciousTokens.length; ++i) {  

File: party-contracts-c4\contracts\proposals\ListOnOpenseaProposal.sol
  291,18:             for (uint256 i = 0; i < fees.length; ++i) {

5)USE A MORE RECENT VERSION OF SOLIDITY

Use a solidity version of at least 0.8.2 to get compiler automatic inlining

Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads

Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

contracts\crowdfund\AuctionCrowdfund.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\crowdfund\BuyCrowdfund.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\crowdfund\BuyCrowdfundBase.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\crowdfund\CollectionBuyCrowdfund.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\crowdfund\Crowdfund.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\crowdfund\CrowdfundFactory.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\crowdfund\CrowdfundNFT.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\distribution\ITokenDistributor.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\distribution\ITokenDistributorParty.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\distribution\TokenDistributor.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\gatekeepers\AllowListGateKeeper.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\gatekeepers\IGateKeeper.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\gatekeepers\TokenGateKeeper.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\globals\Globals.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\globals\IGlobals.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\globals\LibGlobals.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\market-wrapper\FoundationMarketWrapper.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\market-wrapper\IMarketWrapper.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\market-wrapper\KoansMarketWrapper.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\market-wrapper\NounsMarketWrapper.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\market-wrapper\ZoraMarketWrapper.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\party\IPartyFactory.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\party\Party.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\party\PartyFactory.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\party\PartyGovernance.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\party\PartyGovernanceNFT.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\proposals\ArbitraryCallsProposal.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\proposals\FractionalizeProposal.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\proposals\IProposalExecutionEngine.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\proposals\LibProposal.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\proposals\ListOnOpenseaProposal.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\proposals\ListOnZoraProposal.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\proposals\ProposalExecutionEngine.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\proposals\ProposalStorage.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\proposals\ZoraHelpers.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\proposals\vendor\FractionalV1.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\proposals\vendor\IOpenseaConduitController.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  

contracts\proposals\vendor\IOpenseaExchange.sol:
  1  // SPDX-License-Identifier: Beta Software
  2: pragma solidity ^0.8;
  3  



6)X = X + Y IS CHEAPER THAN X += Y 


contracts\crowdfund\Crowdfund.sol:
  242          for (uint256 i = 0; i < numContributions; ++i) {
  243:             ethContributed += contributions[i].amount;
  244          }

  351                      // This entire contribution was not used.
  352:                     ethOwed += c.amount;
  353                  } else if (c.previousTotalContributions + c.amount <= totalEthUsed) {
  354                      // This entire contribution was used.
  355:                     ethUsed += c.amount;
  356                  } else {

  358                      uint256 partialEthUsed = totalEthUsed - c.previousTotalContributions;
  359:                     ethUsed += partialEthUsed;
  360                      ethOwed = c.amount - partialEthUsed;

  373              // voting power.
  374:             votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up
  375          }

  410              // Increase total contributions.
  411:             totalContributions += amount;
  412  

  426                      // No one else has contributed since so just reuse the last entry.
  427:                     lastContribution.amount += amount;
  428                      contributions[numContributions - 1] = lastContribution;

contracts\party\PartyGovernance.sol:
  594          uint96 votingPower = getVotingPowerAt(msg.sender, values.proposedTime, snapIndex);
  595:         values.votes += votingPower;
  596          info.values = values;






