
- `ABI.ENCODE()` is less efficient than `ABI.ENCODEPACKED()`.
	- Instances: 
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC20.sol#L135
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC20.sol#L168
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L164
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L219
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L115
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L23



- `KECCAK256()` should only need to be called on a specific literal once.
	- Instances: 
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC20.sol#L169
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/DummyERC721Renderer.sol#L9


- Splitting `REQUIRE()` statements that use && saves gas.
	- Instance:
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC20.sol#L153


- Use custom errors rather than REVERT()/REQUIRE() strings to save gas.
	- Instances:
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC20.sol#L153
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC20.sol#L124
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC721.sol#L35
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC721.sol#L39
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC721.sol#L35



- `x += y` cost more gas than `x = x + y` for state variables.
	- Instances:
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L243
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L352
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L355
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L359
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L374
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L411
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L427
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC20.sol#L81
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC20.sol#L103
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC20.sol#L183
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC20.sol#L188
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#L51
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#L52
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#L85
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#L118
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#L145
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#L169



- Usage of `UINTS/INTS` smaller than 32 bytes incurs overhead.
	-  When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size. https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Use a larger size then downcast where needed.
	- Instances:
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L94
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L96
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L99
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L170
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L30
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L33
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L39
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L40



- `++i` costs less gas than `i++`, especially when in FOR loops.
	- `++i` costs less gas compared to `i++` or `i += 1` for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration).
	- Instance: 
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/PartyHelpers.sol#L64
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/PartyHelpers.sol#L85
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/PartyHelpers.sol#L115


- `<ARRAY>.length` should not be looked up in every loop of a for-loop.
	- Instance: 
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/PartyHelpers.sol#L64
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/PartyHelpers.sol#L85
	



- Using unchecked blocks to save gas - increments in for loop can be unchecked (SAVE 30-40 GAS PER LOOP ITERATION).
	- The majority of Solidity for loops increment a uint256 variable that starts at 0. These increment operations never need to be checked for over/underflow because the variable will never reach the max number of uint256 (will run out of gas long before that happens). The default over/underflow check wastes gas in every iteration of virtually every for loop .
	- Instance: 
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/PartyHelpers.sol#L85



-  It costs more gas to initialize NON-CONSTANT/NON-IMMUTABLE variables to zero than to let the default of zero be applied.
	- Instance: 
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/vendor/solmate/ERC1155.sol#L118
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/PartyHelpers.sol#L115