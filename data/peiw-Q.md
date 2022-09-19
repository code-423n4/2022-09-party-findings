- Use a more recent version of Solidity.
	- Use latest 0.8.17 for security. [release note](https://blog.soliditylang.org/2022/09/08/solidity-0.8.17-release-announcement/)

- Use of floating pragma
	- Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
	- Instance:
		- 
	

- Open TO-DOs:
	- Instance:
		- https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/renderers/DummyERC721Renderer.sol#L8