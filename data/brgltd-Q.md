# [L-01] Non library/interface files should use fixed compiler verion

Locking the pragma helps to ensure that contracts do not accidentally get deployed using an outdated compiler version.

Note that pragma statements can be allowed to float when a contract is intended for consumption by other developers, as in the case with contracts in a library or a package.

The following 20 contracts are not libraries/abstract/interfaces and are floating the pragma.

```
contracts/utils/EIP165.sol,
contracts/tokens/ERC1155Receiver.sol,
contracts/tokens/ERC721Receiver.sol,
contracts/utils/Proxy.sol,
contracts/utils/ReadOnlyDelegateCall.sol,
contracts/party/Party.sol,
contracts/party/PartyFactory.sol,
contracts/proposals/ProposalStorage.sol,
contracts/proposals/FractionalizeProposal.sol,
contracts/globals/Globals.sol,
contracts/crowdfund/BuyCrowdfund.sol,
contracts/crowdfund/CollectionBuyCrowdfund.sol,
contracts/crowdfund/CrowdfundFactory.sol,
contracts/crowdfund/CrowdfundNFT.sol,
contracts/party/PartyGovernanceNFT.sol,
contracts/proposals/ListOnZoraProposal.sol,
contracts/crowdfund/AuctionCrowdfund.sol,
contracts/proposals/ArbitraryCallsProposal.sol,
contracts/proposals/ProposalExecutionEngine.sol,
contracts/distribution/TokenDistributor.sol,
```

# [L-02] Validate if `totalVotingPower` is not zero

`totalVotingPower` gets initialized by a function argument.

```
function _initialize(
    GovernanceOpts memory opts,
    IERC721[] memory preciousTokens,
    uint256[] memory preciousTokenIds
)
    internal
    virtual
{
    ...
    _governanceValues = GovernanceValues({
        voteDuration: opts.voteDuration,
        executionDelay: opts.executionDelay,
        passThresholdBps: opts.passThresholdBps,
        totalVotingPower: opts.totalVotingPower
    });
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L271-L298

This value will used as a denominator inside the function `_isUnanimousVotes()`, called by the functions `_getProposalFlags()` and `_getProposalStatus()`.

```
function _getProposalFlags(ProposalStateValues memory pv)
    private
    view
    returns (uint256)
{
    if (_isUnanimousVotes(pv.votes, _governanceValues.totalVotingPower)) {
        return LibProposal.PROPOSAL_FLAG_UNANIMOUS;
    }
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1001-L1007

```
function _getProposalStatus(ProposalStateValues memory pv)
    private
    view
    returns (ProposalStatus status)
{
    ...
    if (_isUnanimousVotes(pv.votes, gv.totalVotingPower)) {
        return ProposalStatus.Ready;
    }
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1012-L1044

```
function _isUnanimousVotes(uint96 totalVotes, uint96 totalVotingPower)
    private
    pure
    returns (bool)
{
    uint256 acceptanceRatio = (totalVotes * 1e4) / totalVotingPower;
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1057-L1062

If `totalVotingPower` is zero (e.g. by a setting mistake), `_isUnanimousVotes()` will always throw a divide-by-zero error. This would cause the need for reconfigurations or redeployments.

## Recommended Mitigation Steps

Validate `totalVotingPower` during initialization, to ensure it's not receiving the value zero. Alternatively, a setter function could be created to modify this value after initialization.

# [L-03] Create a setter function for unanimous offset calculation

To compute unanimity, the project will use an offset of 9999.

```
return acceptanceRatio >= 0.9999e4;
```

Since the calculation is not expected to be 100% precise, an improvement that can be made is to create a setter function for the value `0.9999e4`, in case this value needs to change after deployment.

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L1066

# [L-04] If the auction receives a very large duration, `expiry` will silently overflow

`expiry` will be the result of the duration option plus block.timestapm in `AuctionCrowdfund.sol`.

```
function initialize(AuctionCrowdfundOptions memory opts)
    external
    payable
    onlyConstructor
{
    ...
    expiry = uint40(opts.duration + block.timestamp);
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L110-L118

If `ops.duration` receives a large value (e.g. larger than the current unix timestamp).

Realistically the duration shouldn't be this large, so the only plausible scenario where this would happen is if an incorrect value is passed to the `initialize` function. (e.g. passing a incorrect value by mistake).

## Proof of Concept

- `opts.duration` recieves the maximum uint40 (`1099511627775`), by a setting mistake
- assume the current timestamp is `1663497243`
- `expiry` will silently overflow to `1663497722`. The corret value should be `1101175125498`, but the correct approach for the `initialize` function would be to revert in this case

## Recommended Mitigation Steps

Use a safe cast function to prevent a slient overflow, and thus ensure the `initialize()` function will revert in case of receiving a bad value (large `duration`).

# [NC-01] Use constants for repeatable numbers used in mathematical operations.

The number `1e4` is used 4 times in `_getFinalContribution()` and 3 times in `_initialize()` for the `CrowdFund` contract.

```
votingPower = ((1e4 - splitBps_) * ethUsed) / 1e4;
if (splitRecipient_ == contributor) {
    // Split recipient is also the contributor so just add the split
    // voting power.
    votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up
}
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L370-L375

```
if (opts.governanceOpts.feeBps > 1e4) {
    revert InvalidBpsError(opts.governanceOpts.feeBps);
}
if (opts.governanceOpts.passThresholdBps > 1e4) {
    revert InvalidBpsError(opts.governanceOpts.passThresholdBps);
}
if (opts.splitBps > 1e4) {
    revert InvalidBpsError(opts.splitBps);
}
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L129-L137

Consider refactoring this number into a constant.

# [NC-02] Missing NATSPEC

Consider adding NATSPEC on functions where it's currently missing to improve documentation.

```
function _executeArbitraryCalls( 
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L37

```
function _getFinalContribution(address contributor)
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L339

```
function _executeProposal(
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L804


# [NC-03] Empty receive function

```
receive() external payable {} 
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/Party.sol#L47

If the intention is for the Ether to be used, the function should call another function or process the received Ether.

Note that if the contract is not interacting with native ETH directly, Ethereum Engineering Group/Consensys recommends not using an empty receive function. [Reference](https://youtu.be/QfFjUMPtsM0?t=2565).

# [NC-04] Missing event for critical functions

```
function emergencyExecute(
    address targetAddress,
    bytes calldata targetCallData,
    uint256 amountEth
)
    external
    payable
    onlyPartyDao
    onlyWhenEmergencyExecuteAllowed
    onlyDelegateCall
    returns (bool success)
{
    (success, ) = targetAddress.call{value: amountEth}(targetCallData);
}
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L783-L796

```
function disableEmergencyExecute() external onlyPartyDaoOrHost onlyDelegateCall {
    emergencyExecuteDisabled = true;
}
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L800-L802

# [NC-05] Public functions not called by the contract should be declared external 

```
function contribute(address delegate, bytes memory gateData)
    public
    payable
{
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L191

# [NC-06] Inconsistent use of return named variable

```
function _getFinalPrice()
    internal
    override
    view
    returns (uint256 price)
{
    return lastBid;
}
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L283-L290

```
function _prepareGate(
    IGateKeeper gateKeeper,
    bytes12 gateKeeperId,
    bytes memory createGateCallData
)
    private
    returns (bytes12 newGateKeeperId)
{
    if (
        address(gateKeeper) == address(0) ||
        gateKeeperId != bytes12(0)
    ) {
        // Using an existing gate on the gatekeeper
        // or not using a gate at all.
        return gateKeeperId;
    }
    // Call the gate creation function on the gatekeeper.
    (bool s, bytes memory r) = address(gateKeeper).call(createGateCallData);
    if (!s) {
        r.rawRevert();
    }
    // Result is always a bytes12.
    return abi.decode(r, (bytes12));
}
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundFactory.sol#L107-L130
