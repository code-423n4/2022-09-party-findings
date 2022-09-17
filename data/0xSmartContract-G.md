### Gas Optimizations List
| Number | Optimization Details | Per gas saved | Context |
|:--:|:-------| :-----:| :-----:|
| [G-01] |No need ```return``` keyword for gas efficient | ~10 / ~2000 | 3 |
| [G-02] | Shifting cheaper than division | 146 | 1 |
| [G-03] | Using ```uint256``` instead of ```bool``` in mappings is more gas efficient | 146 | 2 |
| [G-04] | Use assembly to write address storage values | 33 | 4 |
| [G-05] | Function ordering via method ID | 22 | All contracts |
| [G-06] | ```x += y``` costs more gas than ```x = x + y``` for state variables | 16 | 8 |
| [G-07] | Use assembly to check for ```address(0)``` | 6 | 1 |
| [G-08] | Using ```>``` instead of ```>=```  is more gas efficient |3 | 2 |
| [G-09] | Use inline assembly for contract balance |2.4k |2 |
| [G-10] | Deploy the contract with `clone` instead of `new` | | 1 |
| [G-11] | It is more efficient to use `abi.encodePacked` instead of `abi.encode` | 164 | 4 |
| [G-12] | Setting the constructor to payable | 13 | 8 |
| [G-13] | Catching the array length prior to loop | 13 | 5 |
| [G-14] | Use ++index instead of index++ to increment a loop counter | 5 | 1 |
| [G-15] | Free gas savings for using solidity 0.8.10+ |  | All contracts |
| [G-16] | Use a different pattern when using for loops | ~400 | 10 |

Total 16 issues


## [G-01] No need ```return``` keyword for gas efficient [~10 / ~2000 gas saved]

**Context:**
[AuctionCrowdfund.sol#L289](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/AuctionCrowdfund.sol#L289)
[BuyCrowdfund.sol#L107](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfund.sol#L107)
[BuyCrowdfundBase.sol#L172](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L172)

**Recommendation:**
You must remove the ```return``` keyword from the specified contexts.

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
    
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        c0.token();
        c1.token();
    }
}

contract Contract0 {
     address addr = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;
     function token() external view returns (address) {
        return addr;
    }
}

contract Contract1 {
    address addr = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;
     function token() external view returns (address) {
        addr;
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 50817                                     ┆ 211             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ token                                     ┆ 2258            ┆ 2258 ┆ 2258   ┆ 2258 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 46011                                     ┆ 187             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ token                                     ┆ 149             ┆ 149 ┆ 149    ┆ 149 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
```

## [G-02] Shifting cheaper than division [146 gas per instance]

**Context:**
[PartyGovernance.sol#L434](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L434)

**Description:**
A division by 2 can be calculated by shifting one to the right. While the ```DIV``` opcode uses ```5 gas```, the ```SHR``` opcode only uses ```3 gas``` Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

**Recommendation:**
Instead of dividing by ``2`` use ```>> 1```.
like this:
```js
medium = high - (high - low) >> 1;
```
**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs

```js
contract GasTest is DSTest {
    
    Contract1 c1;
    Contract2 c2;
    
    function setUp() public {
        c1 = new Contract1();
        c2 = new Contract2();
    }
    
    function testGas() public {
        c1.checkPoint();
        c2.checkPoint();
    }
}

contract Contract1 {
    uint32 a = 10;
    uint32 b = 2;
    
    function checkPoint() public view returns (uint32) {
        uint32 center = a/b;
        return(center);
    }
}

contract Contract2 { 
    uint32 a = 10;
    uint32 b = 2;
    
    function checkPoint() public view returns (uint32) {
        uint32 center = a >> 1;
        return(center);
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 70035                                     ┆ 293             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ checkPoint                                ┆ 2413            ┆ 2413 ┆ 2413   ┆ 2413 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract2 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 49417                                     ┆ 189             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ checkPoint                                ┆ 2267            ┆ 2267 ┆ 2267   ┆ 2267 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```

## [G-03] Using ```uint256``` instead of ```bool``` in mappings is more gas efficient [146 gas per instance]

**Context:** 
[PartyGovernance.sol#L148](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L148)
[PartyGovernance.sol#L207](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L207)


**Description:**
OpenZeppelin uint256 and bool comparison: 
Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled. The values being non-zero value makes deployment a bit more expensive.

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past.

**Recommendation:**
Use  uint256(1) and uint256(2) instead of bool.

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs

```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
     
        c0._setPolicyPermissions(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4, 79, true);
        c1._setPolicyPermissions(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4, 79, 2);
    }
}

contract Contract0 {
    mapping(address => mapping(uint256  => bool)) public modulePermissions;
    error boolcheck();

    function _setPolicyPermissions(address addr_, uint256 id, bool log) external {
       modulePermissions[addr_][id] = log; 
       if(modulePermissions[addr_][id] = false) revert boolcheck();
    }
}
contract Contract1 {
    mapping(address => mapping(uint256  => uint256)) public modulePermissions;
    error boolcheck();

    // Use uint256 instead of bool    (1 = false , 2 = true) 
    function _setPolicyPermissions(address addr_, uint256 id, uint256 log) external {
       modulePermissions[addr_][id] == log; 
       if(modulePermissions[addr_][id] == 1) revert boolcheck();
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 88335                                     ┆ 473             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ _setPolicyPermissions                     ┆ 2750            ┆ 2750 ┆ 2750   ┆ 2750 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 87135                                     ┆ 467             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ _setPolicyPermissions                     ┆ 2604            ┆ 2604 ┆ 2604   ┆ 2604 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```

## [G-04] Use assembly to write address storage values [33 gas per instance]

**Context:**
[BuyCrowdfundBase.sol#L127](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L127)
[CrowdfundNFT.sol#L43-L44](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol#L43-L44)
[Globals.sol#L24](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L24)
[Globals.sol#L28](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/globals/Globals.sol#L28)


**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTestFoundry is DSTest {
    Contract1 c1;
    Contract2 c2;
    
    function setUp() public {
        c1 = new Contract1();
        c2 = new Contract2();
    }
    
    function testGas() public {
        c1.setRewardTokenAndAmount(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4, 356);
        c2.setRewardTokenAndAmount(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4, 356);
    }
}

contract Contract1  {
    address rewardToken ;
    uint256 reward;

    function setRewardTokenAndAmount(address token_, uint256 reward_) external {
        rewardToken = token_;
        reward = reward_;
    }
}

contract Contract2  {
    address rewardToken ;
    uint256 reward;

    function setRewardTokenAndAmount(address token_, uint256 reward_) external {
        assembly {
            sstore(rewardToken.slot, token_)
            sstore(reward.slot, reward_)           

        }
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆       ┆        ┆       ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 50899                                     ┆ 285             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setRewardTokenAndAmount                   ┆ 44490           ┆ 44490 ┆ 44490  ┆ 44490 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/GasTest.t.sol:Contract2 contract ┆                 ┆       ┆        ┆       ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 38087                                     ┆ 221             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setRewardTokenAndAmount                   ┆ 44457           ┆ 44457 ┆ 44457  ┆ 44457 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
```

## [G-05] Function ordering via method ID [22 gas per instance]

**Context:** 
All Contracts

**Description:** 
Contracts most called functions could simply save gas by function ordering via ```Method ID```. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because ```22 gas``` are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. 

**Recommendation:** 
Find a lower ```method ID``` name for the most called functions for example Call() vs. Call1() is cheaper by ```22 gas```
For example, the function IDs in the ``` TokenDistributor.sol``` contract will be the most used; A lower method ID may be given.

**Proof of Consept:**
https://coinsbench.com/advanced-gas-optimizations-tips-for-solidity-85c47f413dc5

TokenDistributor.sol function names can be named and sorted according to METHOD ID

```js
Sighash | Function Signature
========================
61882623 => getRemainingMemberSupply(ITokenDistributorParty,uint256)
95b95ec5 => createNativeDistribution(ITokenDistributorParty,address,uint16)
62cef8a8 => createErc20Distribution(IERC20,ITokenDistributorParty,address,uint16)
036b6643 => claim(DistributionInfo,uint256)
b46c55d2 => claimFee(DistributionInfo,address)
c42c346f => batchClaim(DistributionInfo[],uint256[])
d2cbb377 => batchClaimFee(DistributionInfo[],address)
d7b650d0 => getClaimAmount(ITokenDistributorParty,uint256,uint256)
f3e58fd4 => wasFeeClaimed(ITokenDistributorParty,uint256)
4379109f => hasPartyTokenIdClaimed(ITokenDistributorParty,uint256,uint256)
0c45050c => emergencyRemoveDistribution(ITokenDistributorParty,uint256)
e94cf7cd => emergencyWithdraw(TokenType,address,address,uint256)
3c82db3a => disableEmergencyActions()
8f5fd65f => _createDistribution(CreateDistributionArgs)
1d1b8b76 => _transfer(TokenType,address,address,uint256)
1d57d09e => _getDistributionHash(DistributionInfo)
1959690f => _getBalanceId(TokenType,address)
```

## [G-06] ```x += y``` costs more gas than ```x = x + y``` for state variables [16 gas per instance]

**Context:**
[Crowdfund.sol#L243](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L243)
[Crowdfund.sol#L352](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L352)
[Crowdfund.sol#L355](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L355)
[Crowdfund.sol#L374](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L374)
[Crowdfund.sol#L411](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L411)
[Crowdfund.sol#L427](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L427)
[PartyGovernance.sol#L595](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L595)
[PartyGovernance.sol#L959](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L959)

**Description:**
```x += y``` costs more gas than ```x = x + y``` for state variables.

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        c0.foo(33);
        c1.foo(33);
    }
}

contract Contract0 {
    uint256 _totalBorrow = 60;
    
    function foo(uint256 _interestEarned) public {
        _totalBorrow += interestEarned;
    }
}

contract Contract1 {
    uint256 _totalBorrow = 60;
    function foo(uint256 _interestEarned) public {
        _totalBorrow = _totalBorrow + interestEarned;
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 70805                                     ┆ 279             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 5302            ┆ 5302 ┆ 5302   ┆ 5302 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 69805                                     ┆ 274             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 5286            ┆ 5286 ┆ 5286   ┆ 5286 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```

## [G-07] Use assembly to check for ```address(0)``` [6 gas per instance]

**Context:** 
[Crowdfund.sol#L388-L389](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L388-L389)

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    function testGas() public view {
        c0.setOperator(address(this));
        c1.setOperator(address(this));
    }
}
contract Contract0 {
    error Callback_InvalidParams();
    function setOperator(address operator_) public pure {
        if (address(operator_) == address(0)) revert Callback_InvalidParams();
    }
}
contract Contract1 {
    function setOperator(address operator_) public pure {
        assembly {
            if iszero(operator_) {
                mstore(0x00, "Callback_InvalidParams")
                revert(0x00, 0x20)
            }
        }
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 50899                                     ┆ 285             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setOperator                               ┆ 258             ┆ 258 ┆ 258    ┆ 258 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 44893                                     ┆ 255             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ setOperator                               ┆ 252             ┆ 252 ┆ 252    ┆ 252 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
```

## [G-08] Using ```>``` instead of ```>=```  is more gas efficient [3 gas per instance]

**Context:**
[Crowdfund.sol#L423](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L423)
[ArbitraryCallsProposal.sol#L156](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L156)

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js

contract GasTest is DSTest {
    
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        c0._castVote0(2);
        c1._castVote1(2);
    }
}

contract Contract0 {
error INVALID_VOTE0();

    function _castVote0(uint256 _support) public returns (uint256) {
        if (_support >= 3) revert INVALID_VOTE0();
        return _support;
    }
}

contract Contract1 {
error INVALID_VOTE1();

    function _castVote1(uint256 _support) public returns (uint256) {
        if (_support > 2) revert INVALID_VOTE1();
        return _support;
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 43893                                     ┆ 250             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ _castVote0                                ┆ 275             ┆ 275 ┆ 275    ┆ 275 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 44093                                     ┆ 251             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ _castVote1                                ┆ 278             ┆ 278 ┆ 278    ┆ 278 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
```

## [G-09] Use inline assembly for contract balance [2.4k gas per instance]

**Context:**
[Crowdfund.sol#L143](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L143)
[TokenDistributor.sol#L111](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L111)

**Description:**
You  can use ```balance(address)``` instead of ```address.balance()``` when getting an contract’s balance of ETH.

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.

```js
contract GasTest is DSTest {
    
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        
        c0._withdraw(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4);
        c1._withdraw(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4);
    }
}

contract Contract0 {
    
    function _withdraw(address addr) external {
        uint256 amount = address(this).balance;
        (bool sent, ) = addr.call{ value: amount }('');
    }
}

contract Contract1 {
    
    function _withdraw(address addr) external returns (uint256) {
        uint256 amount ;
        assembly {
            amount := selfbalance()
        }
        (bool sent, ) = addr.call{ value: amount }('');
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 55305                                     ┆ 308             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ _withdraw                                 ┆ 2949            ┆ 2949 ┆ 2949   ┆ 2949 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 59511                                     ┆ 329             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ _withdraw                                 ┆ 509             ┆ 509 ┆ 509    ┆ 509 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
```

## [G-10] Deploy the contract with `clone` instead of `new`

**Context:**
[Crowdfund.sol#L315-L317](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L315-L317)
[PartyFactory.sol#L47](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyFactory.sol#L47)

**Description:**
There’s a way to save a significant amount of gas on deployment using Clones: 
https://www.youtube.com/watch?v=3Mw-pMmJ7TA .
It is extremely inexpensive to distribute by Cloneing with Create2.

[Gas usage difference between the two? (answer: a new clone is 10x cheaper)](https://github.com/porter-finance/v1-core/issues/15#issuecomment-1035639516)

## [G-11] It is more efficient to use `abi.encodePacked` instead of `abi.encode` [164 gas saved]

**Context:**
[ListOnZoraProposal.sol#L115](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L115)
[LibERC20Compat.sol#L16](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibERC20Compat.sol#L16)
[LibSafeERC721.sol#L22](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/LibSafeERC721.sol#L22)
[ReadOnlyDelegateCall.sol#L33](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/ReadOnlyDelegateCall.sol#L33)

**Description:**
 It is more efficient to use `abi.encodePacked` instead of `abi.encode`.

_When are these used?_
```abi.encode:```
abi.encode encode its parameters using the ABI specs. The ABI was designed to make calls to contracts. Parameters are padded to 32 bytes. If you are making calls to a contract you likely have to use abi.encode
If you are dealing with more than one dynamic data type as it prevents collision.

```abi.encodePacked:```
abi.encodePacked encode its parameters using the minimal space required by the type. Encoding an uint8 it will use 1 byte. It is used when you want to save some space, and not calling a contract.Takes all types of data and any amount of input.

```js
contract GasTest is DSTest {
  
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        
        c0.collision("AAA","BBB");
        c1.hard("AAA","BBB");
    }
}

contract Contract0 {

    // abi.encode
    // (AAA,BBB) keccak = 0xd6da8b03238087e6936e7b951b58424ff2783accb08af423b2b9a71cb02bd18b
    // (AA,ABBB) keccak = 0x54bc5894818c61a0ab427d51905bc936ae11a8111a8b373e53c8a34720957abb
    function collision(string memory _text, string memory _anotherText)
        public pure returns (bytes32)
    {
        return keccak256(abi.encode(_text, _anotherText));
    }
}

contract Contract1 {

    // abi.encodePacked
    // (AAA,BBB) keccak = 0xf6568e65381c5fc6a447ddf0dcb848248282b798b2121d9944a87599b7921358
    // (AA,ABBB) keccak = 0xf6568e65381c5fc6a447ddf0dcb848248282b798b2121d9944a87599b7921358

    function hard(string memory _text, string memory _anotherText)
        public pure returns (bytes32)
    {
        return keccak256(abi.encodePacked(_text, _anotherText));
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 132377                                    ┆ 693             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ collision                                 ┆ 1737            ┆ 1737 ┆ 1737   ┆ 1737 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 119365                                    ┆ 628             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ hard                                      ┆ 1573            ┆ 1573 ┆ 1573   ┆ 1573 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```

## [G-12] Setting the constructor to payable [13 gas per instance]

**Context:**
[ListOnZoraProposal.sol#L70](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol#L70)
[PartyGovernance.sol#L266](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L266)
[Crowdfund.sol#L118](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L118)
[ListOnOpenseaProposal.sol#L108](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol#L108)
[BuyCrowdfundBase.sol#L67](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol#L67)
[Implementation.sol#L11](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/utils/Implementation.sol#L11)
[ProposalExecutionEngine.sol#L83](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol#L83)
[TokenDistributor.sol#L93](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L93)

**Description:**
You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of ```msg.value == 0``` and saves ```13 gas``` on deployment with no security risks.

**Recommendation:**
Set the constructor to ```payable```

**Proof Of Concept:**
https://forum.openzeppelin.com/t/a-collection-of-gas-optimisation-tricks/19966/5?u=pcaversaccio

The optimizer was turned on and set to 10000 runs.

```js
contract GasTestFoundry is DSTest {
    
    Contract1 c1;
    Contract2 c2;
    
    function setUp() public {
        c1 = new Contract1();
        c2 = new Contract2();
    }
    
    function testGas() public {
        c1.x();
        c2.x();
    }
}

contract Contract1 {
    
    uint256 public dummy;
    constructor() payable {
        dummy = 1;
    }
    
    function x() public {

    }
}

contract Contract2 {
    
    uint256 public dummy;
    constructor() {
        dummy = 1;
    }
    
    function x() public {
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 49563                                     ┆ 159             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤

╭───────────────────────────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ src/test/GasTest.t.sol:Contract2 contract ┆                 ┆     ┆        ┆     ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 49587                                     ┆ 172             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
```

## [G-13] Catching the array length prior to loop [13 gas per  6 arrays instance]

**Context:** 
[CollectionBuyCrowdfund.sol#L62](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62)
[Crowdfund.sol#L180](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180)
[Crowdfund.sol#L300](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300)
[TokenDistributor.sol#L230](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230)
[TokenDistributor.sol#L239](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239)

**Description:** 
One can save gas by caching the array length (in stack) and using that set variable in the loop. Replace state variable reads and writes within loops with local variable reads and writes. This is done by assigning state variable values to new local variables, reading and/or writing the local variables in a loop, then after the loop assigning any changed local variables to their equivalent state variables.

**Recommendation:** 
Simply do something like so before the for loop:  ```uint length = variable.length``` Then add length in place of  ```variable.length```  in the for loop.

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs

```js
contract GasTest is DSTest {
    Contract1 c1;
    Contract2 c2;

    function setUp() public {
        c1 = new Contract1();
        c2 = new Contract2();

    }

    function testGas() public {
        address[] memory Inputs = new address[](6);
        Inputs[0] = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;
        Inputs[1] = 0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2;
        Inputs[2] = 0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db;
        Inputs[3] = 0x78731D3Ca6b7E34aC0F824c42a7cC18A495cabaB;
        Inputs[4] = 0x617F2E2fD72FD9D5503197092aC168c91465E7f2;
        Inputs[5] = 0x17F6AD8Ef982297579C203069C1DbfFE4348c372;

        c1.executeProposal(Inputs);
        c2.executeProposal(Inputs);
    }
}

contract Contract1 {

    function executeProposal(address[] memory test) public pure {
        uint256 a = test.length;
        for (uint256 step; step  < a; ) {
            unchecked {
                ++step;
            }
        }
    }
}

contract Contract2 { 

    function executeProposal(address[] memory test) public pure {
        for (uint256 step; step < test.length; ) {
            unchecked {
                ++step;
            }
        }
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 92941                                     ┆ 496             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ executeProposal                           ┆ 1653            ┆ 1653 ┆ 1653   ┆ 1653 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract2 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 92541                                     ┆ 494             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ executeProposal                           ┆ 1666            ┆ 1666 ┆ 1666   ┆ 1666 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```

## [G-14] Use ```++index instead``` of ```index++``` to increment a loop counter [5 gas per instance]

**Context:**
[CollectionBuyCrowdfund.sol#L62](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62)

**Description:**
Due to reduced stack operations, using ++index saves 5 gas per iteration.

**Recommendation:**
Use ++index to increment a loop counter.

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.
```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        c0.foo();
        c1.foo();
    }
}

contract Contract0 {
    
    uint256 versionNFTDropCollection;
    
    function foo() external {
        ++versionNFTDropCollection;
    }
}

contract Contract1 {
    
    uint256 versionNFTDropCollection;
    
    function foo() external {
        versionNFTDropCollection++;
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆       ┆        ┆       ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 43893                                     ┆ 250             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 22303           ┆ 22303 ┆ 22303  ┆ 22303 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬───────┬────────┬───────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆       ┆        ┆       ┆         │
╞═══════════════════════════════════════════╪═════════════════╪═══════╪════════╪═══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 44293                                     ┆ 252             ┆       ┆        ┆       ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg   ┆ median ┆ max   ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ foo                                       ┆ 22308           ┆ 22308 ┆ 22308  ┆ 22308 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴───────┴────────┴───────┴─────────╯
```

## [G-15] Free gas savings for using solidity 0.8.10+

**Context:**
All Contracts

**Description:**
Solidity 0.8.10 has a useful change which reduced gas costs of external calls which expect a return value: https://blog.soliditylang.org/2021/11/09/solidity-0.8.10-release-announcement/
Code Generator: Skip existence check for external contract if return data is expected. In this case, the ABI decoder will revert if the contract does not exist.

**Recommendation:**
Consider to upgrade pragma to at least 0.8.10

## [G-16] Use a different pattern when using for loops [~400 gas per instance]

**Context:** 
[CollectionBuyCrowdfund.sol#L62-L66](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol#L62-L66)
[Crowdfund.sol#L180](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L180)
[Crowdfund.sol#L242](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L242)
[Crowdfund.sol#L300](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol#L300)
[TokenDistributor.sol#L230](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L230)
[TokenDistributor.sol#L239](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol#L239)
[PartyGovernance.sol#L306](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol#L306)
[ArbitraryCallsProposal.sol#L52](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L52)
[ArbitraryCallsProposal.sol#L61](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L61)
[ArbitraryCallsProposal.sol#L78](https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol#L78)

**Description:**
In the use of for loops, this structure, which will reduce gas costs, can be preferred. It saves approximately 400 gas in an array with 6 members.

**Proof Of Concept:**
The optimizer was turned on and set to 10000 runs.
```js
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;
    
    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }
    
    function testGas() public {
        address[] memory Inputs = new address[](6);
        Inputs[0] = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;
        Inputs[1] = 0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2;
        Inputs[2] = 0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db;
        Inputs[3] = 0x78731D3Ca6b7E34aC0F824c42a7cC18A495cabaB;
        Inputs[4] = 0x617F2E2fD72FD9D5503197092aC168c91465E7f2;
        Inputs[5] = 0x17F6AD8Ef982297579C203069C1DbfFE4348c372;
        
        c0.dummy(Inputs);
        c1.dummy(Inputs);
    }
}

contract Contract0 {
    
    function dummy(address[] memory test) public {
        for (uint256 i = 0; i < test.length; ++i) {

        }
    }
}

contract Contract1 {
    
    function dummy(address[] memory test) public {
        for (uint256 i = 0; i < test.length; i = unchecked_inc(i)) {
            // do something that doesn't change the value of i
        }
    }
    
    function unchecked_inc(uint i) internal pure returns (uint) {
        unchecked {
            return i + 1;
        }
    }
}
```
**Gas Report:**
```js
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract0 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 113159                                    ┆ 597             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ dummy                                     ┆ 2044            ┆ 2044 ┆ 2044   ┆ 2044 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
╭───────────────────────────────────────────┬─────────────────┬──────┬────────┬──────┬─────────╮
│ src/test/GasTest.t.sol:Contract1 contract ┆                 ┆      ┆        ┆      ┆         │
╞═══════════════════════════════════════════╪═════════════════╪══════╪════════╪══════╪═════════╡
│ Deployment Cost                           ┆ Deployment Size ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 92541                                     ┆ 494             ┆      ┆        ┆      ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name                             ┆ min             ┆ avg  ┆ median ┆ max  ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ dummy                                     ┆ 1666            ┆ 1666 ┆ 1666   ┆ 1666 ┆ 1       │
╰───────────────────────────────────────────┴─────────────────┴──────┴────────┴──────┴─────────╯
```