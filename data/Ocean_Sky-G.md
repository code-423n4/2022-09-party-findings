[G-01] ++I COSTS LESS GAS COMPARED TO I++ OR I += 1 IN FOR LOOPS (~5 GAS PER ITERATION)

Pre-increments are cheaper than post-increments.

For a uint256 i variable, pre-increment ++i costs 5 gas  less than i++ with the optimizer enabled.

Note that post-increments (or post-decrements) return the old value before incrementing or decrementing, hence the name post-increment:
However, pre-increments (or pre-decrements) return the new value.

Instance:
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62

[G-02] IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED

In Solidity, all variables are set to zeros by default. So, do not explicitly initialize a variable with its default value if it is zero.
As an example: for (uint256 i = 0; i < numIterations; ++i) { should be replaced with for (uint256 i; i < numIterations; ++i) {

Instances:
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L242
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L348

[G-03]  USING UNCHECKED FOR INCREMENTS TO SAVE GAS

In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration but at the cost of some code readability, as this uncheck cannot be made inline.
Source: https://github.com/ethereum/solidity/issues/10695

Sample affected code at file ArbitraryCallsProposal.sol#L52:

for (uint256 i = 0; i < hadPreciouses.length; ++i) {
                hadPreciouses[i] = _getHasPrecious(
                    params.preciousTokens[i],
                    params.preciousTokenIds[i]
                );
 }

can be changed to the following:

for (uint256 i = 0; i < hadPreciouses.length;) {
                hadPreciouses[i] = _getHasPrecious(
                    params.preciousTokens[i],
                    params.preciousTokenIds[i]
                );
            unchecked {
			++i;
		}
        }
 
Other instances subject for changes:
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L242
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L348
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62


