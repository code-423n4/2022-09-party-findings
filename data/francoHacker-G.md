You can get cheaper for loops (at least 25 gas, however can be up to 80 gas under certain conditions), by rewriting:

 for (uint256 i = 0; i < hosts.length; /** NOTE: Removed i++ **/ ) {
             
                unchecked { ++i; }
        }       

Instances:

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62-L67