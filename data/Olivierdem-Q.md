1. Event is missing indexed fields
       - Each event should use three indexed fields if there are three or more fields.
                Found in ./contracts/crowdfund/BuyCrowdfundBase.sol, line 52 : event Won(Party party, IERC721 token, uint256 tokenId, uint256 settledPrice)

2. Return values of transferFrom() not checked
        - Not all implementations revert when there's a failure in transferFrom(). The function signature has a boolean return value and they indicate errors that way instead.
               By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment.
            Mitigation: Check the return value of transferFrom()

3. Unused receive() function
        - If the intention is for the Ether to be used, the function should call another function, otherwise it should revert. Found line 144 of ./contracts/crowdfund/AuctionCrowdfund.sol and in ./contracts/party/Party.sol.

4. Several Todos left in the code
        - The code still has some todos, which should be resolved before production, recommend checking and fixing or remove the todos
                Found in CrownfundNFTRendere.sol, DummyERC721Renderer.sol and PartyGovernanceNFTRenderer.sol

5. Use a more recent version of Solidity
        - Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(<bytes>, <bytes>).
        - Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>, <str>).
              Found in ./contracts/renderers/CrowdfundNFTRenderer.sol, line 28