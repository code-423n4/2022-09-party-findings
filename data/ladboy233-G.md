

## ABI.ENCODE() IS LESS EFFICIENT THAN ABI.ENCODEPACKED()
   

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L164


```
                    return abi.encode(ListOnOpenseaStep.ListedOnZora, ZoraProgressData({
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L219


```
            return abi.encode(ListOnOpenseaStep.ListedOnOpenSea, orderHash, expiry);
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnZoraProposal.sol#L115


```
            return abi.encode(ZoraStep.ListedOnZora, ZoraProgressData({
```

## <ARRAY>.LENGTH SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP

The overheads outlined below are PER LOOP, excluding the first loop

storage arrays incur a Gwarmaccess (100 gas)
memory arrays use MLOAD (3 gas)
calldata arrays use CALLDATALOAD (3 gas)
Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306


```
        for (uint256 i=0; i < opts.hosts.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L230


```
        for (uint256 i = 0; i < infos.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L239


```
        for (uint256 i = 0; i < infos.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52


```
            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61


```
        for (uint256 i = 0; i < calls.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L78


```
            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14


```
        for (uint256 i = 0; i < preciousTokens.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L32


```
        for (uint256 i = 0; i < preciousTokens.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291


```
            for (uint256 i = 0; i < fees.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62


```
        for (uint256 i; i < hosts.length; i++) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L182


```
        for (uint256 i = 0; i < contributors.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L302


```
        for (uint256 i = 0; i < preciousTokens.length; ++i) {
```
            

## IT COSTS MORE GAS TO INITIALIZE NON-CONSTANT/NON-IMMUTABLE VARIABLES TO ZERO THAN TO LET THE DEFAULT OF ZERO BE APPLIED


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyGovernance.sol#L306


```
        for (uint256 i=0; i < opts.hosts.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L230


```
        for (uint256 i = 0; i < infos.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/distribution/TokenDistributor.sol#L239


```
        for (uint256 i = 0; i < infos.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L52


```
            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L61


```
        for (uint256 i = 0; i < calls.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ArbitraryCallsProposal.sol#L78


```
            for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L14


```
        for (uint256 i = 0; i < preciousTokens.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/LibProposal.sol#L32


```
        for (uint256 i = 0; i < preciousTokens.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/proposals/ListOnOpenseaProposal.sol#L291


```
            for (uint256 i = 0; i < fees.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L182


```
        for (uint256 i = 0; i < contributors.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L244


```
        for (uint256 i = 0; i < numContributions; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L302


```
        for (uint256 i = 0; i < preciousTokens.length; ++i) {
```
            

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L350


```
            for (uint256 i = 0; i < numContributions; ++i) {
```
            

## ++I COSTS LESS GAS THAN I++, ESPECIALLY WHEN IT�S USED IN FOR-LOOPS (--I/I-- TOO)
    
Saves 6 gas per loop


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62


```
        for (uint256 i; i < hosts.length; i++) {
```
            
   
## function can be marked as external instead of public


https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L193


```
    function contribute(address delegate, bytes memory gateData)
```
            
## Avoid using storage keywords instead view function that do not modify state.

Can use memory instead of storage.

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L240

```
        Contribution[] storage contributions = _contributionsByContributor[contributor];
```

https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L346

```
        Contribution[] storage contributions = _contributionsByContributor[contributor];
```
