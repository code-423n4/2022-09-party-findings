## make require into error to save gas 
```
gatekeepers/TokenGateKeeper.sol:40:    /// @notice Creates a gate that requires a minimum balance of a token.
proposals/ProposalExecutionEngine.sol:246:        require(proposalType != ProposalType.Invalid);
proposals/ProposalExecutionEngine.sol:247:        require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));
proposals/ListOnOpenseaProposal.sol:163:                    // Return the next step and data required to execute that step.
proposals/ListOnOpenseaProposal.sol:301:        // Validate the order on-chain so no signature is required to fill it.
utils/vendor/Strings.sol:61:        require(value == 0, "Strings: hex length insufficient");
utils/ReadOnlyDelegateCall.sol:20:        require(msg.sender == address(this));
```
## make array.length cache it to memory varaible 
```
crowdfund/CollectionBuyCrowdfund.sol:62:        for (uint256 i; i < hosts.length; i++) {
```
## make i unchecked to save gas 
```
crowdfund/CollectionBuyCrowdfund.sol:62:        for (uint256 i; i < hosts.length; i++) {
```
## make i++ into ++i to save gas 
```
crowdfund/CollectionBuyCrowdfund.sol:62:        for (uint256 i; i < hosts.length; i++) {
```
##  address(0) into long form ex:0x0000000000000000
```
rowdfund/CrowdfundNFT.sol:127:        if (owner == address(0)) {
crowdfund/CrowdfundNFT.sol:138:        return _owners[uint256(uint160(owner))] != address(0);
crowdfund/CrowdfundNFT.sol:146:            emit Transfer(address(0), owner, tokenId);
crowdfund/CrowdfundNFT.sol:153:            _owners[tokenId] = address(0);
crowdfund/CrowdfundNFT.sol:154:            emit Transfer(owner, address(0), tokenId);
crowdfund/BuyCrowdfundBase.sol:153:            return address(party) != address(0)
crowdfund/Crowdfund.sol:367:        if (splitRecipient_ == address(0)) {
crowdfund/Crowdfund.sol:388:        if (delegate == address(0)) {
crowdfund/Crowdfund.sol:392:        if (gateKeeper != IGateKeeper(address(0))) {
crowdfund/Crowdfund.sol:474:            if (delegate == address(0)) {
crowdfund/CrowdfundFactory.sol:116:            address(gateKeeper) == address(0) ||
crowdfund/AuctionCrowdfund.sol:272:            return address(party) != address(0)
distribution/TokenDistributor.sol:55:    address private constant NATIVE_TOKEN_ADDRESS = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;
party/PartyGovernanceNFT.sol:107:        return (address(0), 0); // Just to make the compiler happy.
party/PartyFactory.sol:36:        if (authority == address(0)) {
party/PartyGovernance.sol:347:    function getVotingPowerAt(address voter, uint40 timestamp)
party/PartyGovernance.sol:361:    function getVotingPowerAt(address voter, uint40 timestamp, uint256 snapIndex)
party/PartyGovernance.sol:422:    function findVotingPowerSnapshotIndex(address voter, uint40 timestamp)
party/PartyGovernance.sol:459:        // 0 is a special case burn address.
party/PartyGovernance.sol:460:        if (newPartyHost != address(0)) {
party/PartyGovernance.sol:507:            IERC20(token).balanceOf(address(this))
party/PartyGovernance.sol:848:    function _getVotingPowerSnapshotAt(address voter, uint40 timestamp, uint256 hintIndex)
party/PartyGovernance.sol:885:        _adjustVotingPower(from, -powerI192, address(0));
party/PartyGovernance.sol:886:        _adjustVotingPower(to, powerI192, address(0));
party/PartyGovernance.sol:898:        oldDelegate = oldDelegate == address(0) ? voter : oldDelegate;
party/PartyGovernance.sol:900:        delegate = delegate == address(0) ? oldDelegate : delegate;
party/PartyGovernance.sol:931:        if (newDelegate == address(0) || oldDelegate == address(0)) {
renderers/PartyGovernanceNFTRenderer.sol:114:        if(_ownerOf[tokenId] == address(0)) {
renderers/PartyGovernanceNFTRenderer.sol:183:        receiver = address(0);
renderers/CrowdfundNFTRenderer.sol:108:        if (Crowdfund(payable(address(this))).ownerOf(tokenId) == address(0)) {
renderers/CrowdfundNFTRenderer.sol:116:        svgParts[1] = textLine(Crowdfund(payable(address(this))).name(), 10, 20);
renderers/CrowdfundNFTRenderer.sol:119:        svgParts[2] = textLine(Crowdfund(payable(address(this))).symbol(), 10, 80);
renderers/CrowdfundNFTRenderer.sol:125:        svgParts[6] = textLine(renderEthContributed(address(uint160(tokenId))), 10, 160);
renderers/CrowdfundNFTRenderer.sol:127:            svgParts[7] = textLine(renderEthUsed(address(uint160(tokenId))), 10, 170);
renderers/CrowdfundNFTRenderer.sol:128:            // svgParts[8] = textLine(renderEthOwed(address(uint160(tokenId))), 10, 180);
gatekeepers/TokenGateKeeper.sol:41:    /// @param  token The token address (eg. ERC20 or ERC721).
globals/Globals.sol:40:        return address(uint160(uint256(_wordValues[key])));
globals/Globals.sol:44:        return Implementation(address(uint160(uint256(_wordValues[key]))));
proposals/ListOnZoraProposal.sol:149:            payable(address(0)),
proposals/ListOnZoraProposal.sol:151:            IERC20(address(0)) // Indicates ETH sale
proposals/ArbitraryCallsProposal.sol:176:                    if (op != address(0)) {
proposals/FractionalizeProposal.sol:69:        vault.updateCurator(address(0));
proposals/ListOnOpenseaProposal.sol:266:        orderParams.orderType = orderParams.zone == address(0)
proposals/ListOnOpenseaProposal.sol:287:            cons.token = address(0);
proposals/ListOnOpenseaProposal.sol:294:                cons.token = address(0);
proposals/ListOnOpenseaProposal.sol:367:            token.approve(address(0), tokenId);
```