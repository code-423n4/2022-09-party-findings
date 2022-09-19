# Gas Report
* Use the old `oldHostsFieldValue` value in the `Crowdfund._hashFixedGovernanceOpts`
    ```sol
    let oldHostsFieldValue := mload(opts)
    mstore(opts, keccak256(add(mload(opts), 0x20), mul(mload(mload(opts)), 32)))
    ```
* Use unchecked on this part of the `Crowdfund._getFinalContribution` function:
    ```sol
    ...
    } else {
        // This contribution was partially used.
        uint256 partialEthUsed = totalEthUsed - c.previousTotalContributions;
        ethUsed += partialEthUsed; // this doesn't need to be unchecked
        ethOwed = c.amount - partialEthUsed;
    }
    ```
    The code will look like this:
    ```sol
    ...
    } else {
        // This contribution was partially used.
        unchecked {
            uint256 partialEthUsed = totalEthUsed - c.previousTotalContributions;
            ethOwed = c.amount - partialEthUsed;
        }
        ethUsed += partialEthUsed; // this doesn't need to be unchecked
    }
    ```
* Use > 0 instead of >= 1, use unchecked and cache the expression's result instead of calculating it twice (in the `Crowdfund._contribute` function):
    ```sol
    if (numContributions >= 1) {
        Contribution memory lastContribution = contributions[numContributions - 1];
        if (lastContribution.previousTotalContributions == previousTotalContributions) {
            // No one else has contributed since so just reuse the last entry.
            lastContribution.amount += amount;
            contributions[numContributions - 1] = lastContribution;
            return;
        }
    }
    ```
    The code will look like this:
    ```sol
    if (numContributions > 0) {
        unchecked {
            uint lastIndex = numContributions - 1;
        }
        Contribution memory lastContribution = contributions[lastIndex];
        if (lastContribution.previousTotalContributions == previousTotalContributions) {
            // No one else has contributed since so just reuse the last entry.
            lastContribution.amount += amount;
            contributions[lastIndex] = lastContribution;
            return;
        }
    }
    ```
* Cache `splitRecipient` in the `Crowdfund._burn()` function
* If the amount is 0, don't perform the low level call in `LibAddress.transferETH()`