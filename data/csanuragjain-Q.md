## Unreachable code - Incorrect logic

Contract:
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L425

Issue:
lastContribution.previousTotalContributions == previousTotalContributions will never be true in [_contribute](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L378)

```
function _contribute(
        address contributor,
        uint96 amount,
        address delegate,
        uint96 previousTotalContributions,
        bytes memory gateData
    )
        internal
    {
...
Contribution[] storage contributions = _contributionsByContributor[contributor];
            uint256 numContributions = contributions.length;
            if (numContributions >= 1) {
                Contribution memory lastContribution = contributions[numContributions - 1];
                if (lastContribution.previousTotalContributions == previousTotalContributions) {
                    // No one else has contributed since so just reuse the last entry.
                    lastContribution.amount += amount;
                    contributions[numContributions - 1] = lastContribution;
                    return;
                }
            }
....
}
```

## Steps

1. Assume User A contributes amount 5 which means

```
totalContributions: 5
previousTotalContributions: 0,
amount: 5
```

2. Now same user again contributes amount 10 which means

```
lastContribution.previousTotalContributions = 0
previousTotalContributions=5
```

3. So even when multiple contribution were made by same person lastContribution.previousTotalContributions != previousTotalContributions

## Lifecycle expires beforehand

Contract:
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L276

Issue:
It seems that CrowdFund bidding expires at block.timestamp = expiry even though it should only expire post block.timestamp > expiry. This will cause users to be unable to vote at very last moment

Recommendation:
Revise the [getCrowdfundLifecycle function](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L262)

```
if (block.timestamp > expiry) {
            // Expired. `finalize()` needs to be called.
            return CrowdfundLifecycle.Expired;
        }
```