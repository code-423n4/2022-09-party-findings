1: PartyGovernance.sol accept()

If the user participates in the crowdfunding but has not burn(), at this time votingPower==0, if  go to vote, can succeed, but votingPower==0, lost the original should have votingPower
Suggest adding votingPower==0 can not vote

```
    function accept(uint256 proposalId, uint256 snapIndex)
        public
        onlyDelegateCall
        returns (uint256 totalVotes)
    {
    ..

    // Increase the total votes that have been cast on this proposal.
    uint96 votingPower = getVotingPowerAt(msg.sender, values.proposedTime, snapIndex);    
+++ require(votingPower!=0);
```

2: Crowdfund.sol _initialize()
Suggest adding the judgment that opts.initialContributor is not address(0), to avoid losing the amount
```
    function _initialize(CrowdfundOptions memory opts)
        internal
    {
    ..
        uint96 initialBalance = address(this).balance.safeCastUint256ToUint96();
        if (initialBalance > 0) {
+++        require(opts.initialContributor!=address(0));
            // If this contract has ETH, either passed in during deployment or
            // pre-existing, credit it to the `initialContributor`.
            _contribute(opts.initialContributor, initialBalance, opts.initialDelegate, 0, "");
        }    
```