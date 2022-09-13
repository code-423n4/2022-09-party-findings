### [G-01] ```abi.encode()``` is less efficient than ```abi.encodePacked()```


#### Impact
Consider using ```abi.encodePacked()``` instead for efficieny.


#### Findings:
```
contracts/utils/ReadOnlyDelegateCall.sol:L23        abi.encode(s, r).rawRevert();

contracts/renderers/DummyERC721Renderer.sol:L9        return string(abi.encode(address(this), tokenId));

contracts/proposals/ListOnZoraProposal.sol:L115            return abi.encode(ZoraStep.ListedOnZora, ZoraProgressData({

contracts/proposals/ListOnOpenseaProposal.sol:L164                    return abi.encode(ListOnOpenseaStep.ListedOnZora, ZoraProgressData({

contracts/proposals/ListOnOpenseaProposal.sol:L219            return abi.encode(ListOnOpenseaStep.ListedOnOpenSea, orderHash, expiry);

contracts/vendor/solmate/ERC20.sol:L135                            abi.encode(

contracts/vendor/solmate/ERC20.sol:L168                abi.encode(

```

### [G-02] Using bools for storage incurs overhead.


#### Impact
 
```
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
```
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27 Use ```uint256(1)``` and ```uint256(2)``` for true/false to avoid a Gwarmaccess ([100 gas](https://gist.github.com/IllIllI000/1b70014db712f8572a72378321250058)), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past


#### Findings:
```
contracts/distribution/TokenDistributor.sol:L30        mapping(uint256 => bool) hasPartyTokenClaimed;

contracts/distribution/TokenDistributor.sol:L62    bool public emergencyActionsDisabled;

contracts/utils/LibERC20Compat.sol:L26            if (abi.decode(r, (bool))) {

contracts/globals/Globals.sol:L12    mapping(uint256 => mapping(bytes32 => bool)) private _includedWordValues;

contracts/party/PartyGovernance.sol:L148        mapping (address => bool) hasVoted;

contracts/party/PartyGovernance.sol:L197    bool public emergencyExecuteDisabled;

contracts/party/PartyGovernance.sol:L207    mapping(address => bool) public isHost;

contracts/renderers/PartyGovernanceNFTRenderer.sol:L26    mapping(address => bool) isHost;

contracts/renderers/PartyGovernanceNFTRenderer.sol:L36    mapping(address => mapping(address => bool)) isApprovedForAll;

contracts/vendor/solmate/ERC721.sol:L50    mapping(address => mapping(address => bool)) public isApprovedForAll;

contracts/vendor/solmate/ERC1155.sol:L24    mapping(address => mapping(address => bool)) public isApprovedForAll;

contracts/crowdfund/Crowdfund.sol:L106    bool private _splitRecipientHasBurned;

```
### [G-03] Cache Array Length Outside of Loop


#### Impact
Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.


#### Findings:
```
contracts/distribution/TokenDistributor.sol:L230        for (uint256 i = 0; i < infos.length; ++i) {

contracts/distribution/TokenDistributor.sol:L239        for (uint256 i = 0; i < infos.length; ++i) {

contracts/utils/PartyHelpers.sol:L64        for (uint256 i = 0; i < members.length; i++) {

contracts/utils/PartyHelpers.sol:L85        for (uint256 i = 0; i < voters.length; i++) {

contracts/party/PartyGovernance.sol:L306        for (uint256 i=0; i < opts.hosts.length; ++i) {

contracts/proposals/ArbitraryCallsProposal.sol:L52            for (uint256 i = 0; i < hadPreciouses.length; ++i) {

contracts/proposals/ArbitraryCallsProposal.sol:L61        for (uint256 i = 0; i < calls.length; ++i) {

contracts/proposals/ArbitraryCallsProposal.sol:L78            for (uint256 i = 0; i < hadPreciouses.length; ++i) {

contracts/proposals/LibProposal.sol:L14        for (uint256 i = 0; i < preciousTokens.length; ++i) {

contracts/proposals/LibProposal.sol:L32        for (uint256 i = 0; i < preciousTokens.length; ++i) {

contracts/proposals/ListOnOpenseaProposal.sol:L291            for (uint256 i = 0; i < fees.length; ++i) {

contracts/vendor/solmate/ERC1155.sol:L80        for (uint256 i = 0; i < ids.length; ) {

contracts/vendor/solmate/ERC1155.sol:L118            for (uint256 i = 0; i < owners.length; ++i) {

contracts/crowdfund/CollectionBuyCrowdfund.sol:L62        for (uint256 i; i < hosts.length; i++) {

contracts/crowdfund/Crowdfund.sol:L180        for (uint256 i = 0; i < contributors.length; ++i) {

contracts/crowdfund/Crowdfund.sol:L300        for (uint256 i = 0; i < preciousTokens.length; ++i) {

```

### [G-04] Use custom errors rather than ```revert()```/```require()``` string to save gas


#### Impact
Custom errors are available from solidity version 0.8.4, it saves around 50 gas each time they are hit by avoiding having to allocate and store the revert string.


#### Findings:
```
contracts/utils/PartyHelpers.sol:L49        revert("PartyHelpers::Unknown CrowdfundType");

contracts/utils/vendor/Strings.sol:L61        require(value == 0, "Strings: hex length insufficient");

contracts/vendor/solmate/ERC721.sol:L35        require((owner = _ownerOf[id]) != address(0), "NOT_MINTED");

contracts/vendor/solmate/ERC721.sol:L39        require(owner != address(0), "ZERO_ADDRESS");

contracts/vendor/solmate/ERC721.sol:L68        require(msg.sender == owner || isApprovedForAll[owner][msg.sender], "NOT_AUTHORIZED");

contracts/vendor/solmate/ERC721.sol:L86        require(from == _ownerOf[id], "WRONG_FROM");

contracts/vendor/solmate/ERC721.sol:L88        require(to != address(0), "INVALID_RECIPIENT");

contracts/vendor/solmate/ERC721.sol:L157        require(to != address(0), "INVALID_RECIPIENT");

contracts/vendor/solmate/ERC721.sol:L159        require(_ownerOf[id] == address(0), "ALREADY_MINTED");

contracts/vendor/solmate/ERC721.sol:L174        require(owner != address(0), "NOT_MINTED");

contracts/vendor/solmate/ERC1155.sol:L49        require(msg.sender == from || isApprovedForAll[from][msg.sender], "NOT_AUTHORIZED");

contracts/vendor/solmate/ERC1155.sol:L72        require(ids.length == amounts.length, "LENGTH_MISMATCH");

contracts/vendor/solmate/ERC1155.sol:L74        require(msg.sender == from || isApprovedForAll[from][msg.sender], "NOT_AUTHORIZED");

contracts/vendor/solmate/ERC1155.sol:L111        require(owners.length == ids.length, "LENGTH_MISMATCH");

contracts/vendor/solmate/ERC1155.sol:L166        require(idsLength == amounts.length, "LENGTH_MISMATCH");

contracts/vendor/solmate/ERC1155.sol:L196        require(idsLength == amounts.length, "LENGTH_MISMATCH");

contracts/vendor/solmate/ERC20.sol:L124        require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");

contracts/vendor/solmate/ERC20.sol:L153            require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");

contracts/crowdfund/CrowdfundNFT.sol:L29        revert('ALWAYS FAILING');

```
### [G-05] Empty blocks should be removed or emit something


#### Impact
The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting.


#### Findings:
```
contracts/party/Party.sol:L28    constructor(IGlobals globals) PartyGovernanceNFT(globals) {}

contracts/party/Party.sol:L47    receive() external payable {}

contracts/crowdfund/CrowdfundNFT.sol:L52    {}

contracts/crowdfund/CrowdfundNFT.sol:L59    {}

contracts/crowdfund/CrowdfundNFT.sol:L66    {}

contracts/crowdfund/CrowdfundNFT.sol:L73    {}

contracts/crowdfund/CrowdfundNFT.sol:L80    {}

contracts/crowdfund/CollectionBuyCrowdfund.sol:L77    constructor(IGlobals globals) BuyCrowdfundBase(globals) {}

contracts/crowdfund/BuyCrowdfund.sol:L62    constructor(IGlobals globals) BuyCrowdfundBase(globals) {}

contracts/crowdfund/BuyCrowdfundBase.sol:L67    constructor(IGlobals globals) Crowdfund(globals) {}

contracts/crowdfund/AuctionCrowdfund.sol:L104    constructor(IGlobals globals) Crowdfund(globals) {}

contracts/crowdfund/AuctionCrowdfund.sol:L144    receive() external payable {}

```
### [G-06] Use a more recent version of solidity.


#### Impact
Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining 
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads 
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings 
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value.


#### Findings:
```
contracts/vendor/solmate/ERC721.sol:L5      pragma solidity >=0.8.0;

contracts/vendor/solmate/ERC20.sol:L5      pragma solidity >=0.8.0;

```
### [G-07] Functions guaranteed to revert when called by normal users can be declared as payable.


#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.


#### Findings:
```
contracts/distribution/TokenDistributor.sol:L327    function disableEmergencyActions() onlyPartyDao external {

contracts/globals/Globals.sol:L27    function transferMultiSig(address newMultiSig) external onlyMultisig {

contracts/globals/Globals.sol:L59    function setBytes32(uint256 key, bytes32 value) external onlyMultisig {

contracts/globals/Globals.sol:L63    function setUint256(uint256 key, uint256 value) external onlyMultisig {

contracts/globals/Globals.sol:L67    function setAddress(uint256 key, address value) external onlyMultisig {

contracts/globals/Globals.sol:L71    function setIncludesBytes32(uint256 key, bytes32 value, bool isIncluded) external onlyMultisig {

contracts/globals/Globals.sol:L75    function setIncludesUint256(uint256 key, uint256 value, bool isIncluded) external onlyMultisig {

contracts/globals/Globals.sol:L79    function setIncludesAddress(uint256 key, address value, bool isIncluded) external onlyMultisig {

contracts/party/PartyGovernance.sol:L451    function delegateVotingPower(address delegate) external onlyDelegateCall {

contracts/party/PartyGovernance.sol:L458    function abdicate(address newPartyHost) external onlyHost onlyDelegateCall {

contracts/party/PartyGovernance.sol:L616    function veto(uint256 proposalId) external onlyHost onlyDelegateCall {

contracts/party/PartyGovernance.sol:L800    function disableEmergencyExecute() external onlyPartyDaoOrHost onlyDelegateCall {

contracts/party/PartyGovernanceNFT.sol:L169    function abdicate() external onlyMinter onlyDelegateCall {

contracts/crowdfund/AuctionCrowdfund.sol:L149    function bid() external onlyDelegateCall {

```
### [G-08] ```X += Y``` costs more gas than ```X = X + Y``` for state variables.


#### Impact
Consider changing ```X += Y``` to ```X = X + Y``` to save gas.


#### Findings:
```
contracts/party/PartyGovernance.sol:L595        values.votes += votingPower;

contracts/vendor/solmate/ERC1155.sol:L52        balanceOf[to][id] += amount;

contracts/vendor/solmate/ERC1155.sol:L85            balanceOf[to][id] += amount;

contracts/vendor/solmate/ERC1155.sol:L145        balanceOf[to][id] += amount;

contracts/vendor/solmate/ERC1155.sol:L169            balanceOf[to][ids[i]] += amounts[i];

contracts/vendor/solmate/ERC20.sol:L81            balanceOf[to] += amount;

contracts/vendor/solmate/ERC20.sol:L103            balanceOf[to] += amount;

contracts/vendor/solmate/ERC20.sol:L183        totalSupply += amount;

contracts/vendor/solmate/ERC20.sol:L188            balanceOf[to] += amount;

contracts/crowdfund/Crowdfund.sol:L243            ethContributed += contributions[i].amount;

contracts/crowdfund/Crowdfund.sol:L352                    ethOwed += c.amount;

contracts/crowdfund/Crowdfund.sol:L355                    ethUsed += c.amount;

contracts/crowdfund/Crowdfund.sol:L359                    ethUsed += partialEthUsed;

contracts/crowdfund/Crowdfund.sol:L374            votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up

contracts/crowdfund/Crowdfund.sol:L411            totalContributions += amount;

contracts/crowdfund/Crowdfund.sol:L427                    lastContribution.amount += amount;

```

### [G-09] ++i costs less gas than i++, especially when it's used in for loops.


#### Impact
Saves 6 gas per loop.


#### Findings:
```
contracts/utils/PartyHelpers.sol:L64        for (uint256 i = 0; i < members.length; i++) {

contracts/utils/PartyHelpers.sol:L85        for (uint256 i = 0; i < voters.length; i++) {

contracts/utils/PartyHelpers.sol:L115        for (uint256 i = 0; i < count; i++) {

contracts/utils/vendor/Strings.sol:L22            digits++;

contracts/utils/vendor/Strings.sol:L44            length++;

contracts/crowdfund/CollectionBuyCrowdfund.sol:L62        for (uint256 i; i < hosts.length; i++) {

```
### [G-10] Splitting ```require()``` statements that use && saves gas.


#### Impact
Consider splitting the ```require()``` statements to save gas.


#### Findings:
```
contracts/vendor/solmate/ERC20.sol:L153            require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");

```

### [G-11] Explicit initialization with zero not required


#### Impact
Explicit initialization with zero is not required for variable declaration because uints are 0 by default. Removing this will reduce contract size and save a bit of gas.


#### Findings:
```
contracts/distribution/TokenDistributor.sol:L230        for (uint256 i = 0; i < infos.length; ++i) {

contracts/distribution/TokenDistributor.sol:L239        for (uint256 i = 0; i < infos.length; ++i) {

contracts/utils/PartyHelpers.sol:L64        for (uint256 i = 0; i < members.length; i++) {

contracts/utils/PartyHelpers.sol:L85        for (uint256 i = 0; i < voters.length; i++) {

contracts/utils/PartyHelpers.sol:L115        for (uint256 i = 0; i < count; i++) {

contracts/utils/vendor/Strings.sol:L42        uint256 length = 0;

contracts/party/PartyGovernance.sol:L432        uint256 low = 0;

contracts/proposals/ArbitraryCallsProposal.sol:L52            for (uint256 i = 0; i < hadPreciouses.length; ++i) {

contracts/proposals/ArbitraryCallsProposal.sol:L61        for (uint256 i = 0; i < calls.length; ++i) {

contracts/proposals/ArbitraryCallsProposal.sol:L78            for (uint256 i = 0; i < hadPreciouses.length; ++i) {

contracts/proposals/LibProposal.sol:L14        for (uint256 i = 0; i < preciousTokens.length; ++i) {

contracts/proposals/LibProposal.sol:L32        for (uint256 i = 0; i < preciousTokens.length; ++i) {

contracts/proposals/ListOnOpenseaProposal.sol:L291            for (uint256 i = 0; i < fees.length; ++i) {

contracts/vendor/solmate/ERC1155.sol:L80        for (uint256 i = 0; i < ids.length; ) {

contracts/vendor/solmate/ERC1155.sol:L118            for (uint256 i = 0; i < owners.length; ++i) {

contracts/vendor/solmate/ERC1155.sol:L168        for (uint256 i = 0; i < idsLength; ) {

contracts/vendor/solmate/ERC1155.sol:L198        for (uint256 i = 0; i < idsLength; ) {

contracts/crowdfund/Crowdfund.sol:L180        for (uint256 i = 0; i < contributors.length; ++i) {

contracts/crowdfund/Crowdfund.sol:L242        for (uint256 i = 0; i < numContributions; ++i) {

contracts/crowdfund/Crowdfund.sol:L300        for (uint256 i = 0; i < preciousTokens.length; ++i) {

contracts/crowdfund/Crowdfund.sol:L348            for (uint256 i = 0; i < numContributions; ++i) {

```
### [G-12] ```++i```/```i++``` should be ```unchecked{++i}```/```unchecked{i++}``` when it is not possible for them to overflow, as is the case when used in for- and while-loops.


#### Impact
The ```unchecked``` keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop.


#### Findings:
```
contracts/distribution/TokenDistributor.sol:L230        for (uint256 i = 0; i < infos.length; ++i) {

contracts/distribution/TokenDistributor.sol:L239        for (uint256 i = 0; i < infos.length; ++i) {

contracts/utils/PartyHelpers.sol:L64        for (uint256 i = 0; i < members.length; i++) {

contracts/utils/PartyHelpers.sol:L85        for (uint256 i = 0; i < voters.length; i++) {

contracts/utils/PartyHelpers.sol:L115        for (uint256 i = 0; i < count; i++) {

contracts/party/PartyGovernance.sol:L306        for (uint256 i=0; i < opts.hosts.length; ++i) {

contracts/proposals/ArbitraryCallsProposal.sol:L52            for (uint256 i = 0; i < hadPreciouses.length; ++i) {

contracts/proposals/ArbitraryCallsProposal.sol:L61        for (uint256 i = 0; i < calls.length; ++i) {

contracts/proposals/ArbitraryCallsProposal.sol:L78            for (uint256 i = 0; i < hadPreciouses.length; ++i) {

contracts/proposals/LibProposal.sol:L14        for (uint256 i = 0; i < preciousTokens.length; ++i) {

contracts/proposals/LibProposal.sol:L32        for (uint256 i = 0; i < preciousTokens.length; ++i) {

contracts/proposals/ListOnOpenseaProposal.sol:L291            for (uint256 i = 0; i < fees.length; ++i) {

contracts/crowdfund/CollectionBuyCrowdfund.sol:L62        for (uint256 i; i < hosts.length; i++) {

contracts/crowdfund/Crowdfund.sol:L180        for (uint256 i = 0; i < contributors.length; ++i) {

contracts/crowdfund/Crowdfund.sol:L242        for (uint256 i = 0; i < numContributions; ++i) {

contracts/crowdfund/Crowdfund.sol:L300        for (uint256 i = 0; i < preciousTokens.length; ++i) {

contracts/crowdfund/Crowdfund.sol:L348            for (uint256 i = 0; i < numContributions; ++i) {

```
