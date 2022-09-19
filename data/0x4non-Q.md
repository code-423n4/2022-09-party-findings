# Low

## There is a reentrancy on `batchBurn`
On `batchBurn` [Crowdfund.sol#L175](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/crowdfund/Crowdfund.sol#L175) there is a reentrancy issue, everytime a nft is burn it will transfer ether so it will be possible to reenter, this is not dangerous, how ever is recommend to use a nonReentrant modifier.

## Add length validation in `createParty` method
[PartyFactory.sol#L26](https://github.com/PartyDAO/party-contracts-c4/blob/3896577b8f0fa16cba129dc2867aba786b730c1b/contracts/party/PartyFactory.sol#L26), the `createParty` should `require(preciousTokens.length == preciousTokenIds.length, "!lingth");` otherwise contract could end ind a wrong state.




# QA


## Use solmate `SafeCast` & `SafeTransfer` instead of a custom implementation
if you use [SafeCastLib.sol](https://github.com/transmissions11/solmate/blob/main/src/utils/SafeCastLib.sol) and [SafeTransferLib.sol](https://github.com/transmissions11/solmate/blob/main/src/utils/SafeTransferLib.sol) it will be less lines of code to audit!


## Use plain revert instead of modifier in `CrowdfundNFT.sol` , this will remove the `unreachable code` warning

```diff
diff --git a/contracts/crowdfund/CrowdfundNFT.sol b/contracts/crowdfund/CrowdfundNFT.sol
index 8c6208f..d194021 100644
--- a/contracts/crowdfund/CrowdfundNFT.sol
+++ b/contracts/crowdfund/CrowdfundNFT.sol
@@ -25,11 +25,6 @@ contract CrowdfundNFT is IERC721, EIP165, ReadOnlyDelegateCall {
 
     mapping (uint256 => address) private _owners;
 
-    modifier alwaysRevert() {
-        revert('ALWAYS FAILING');
-        _; // Compiler requires this.
-    }
-
     // Set the `Globals` contract.
     constructor(IGlobals globals) {
         _GLOBALS = globals;
@@ -46,38 +41,33 @@ contract CrowdfundNFT is IERC721, EIP165, ReadOnlyDelegateCall {
 
     /// @notice DO NOT CALL. This is a soulbound NFT and cannot be transferred.
     ///         Attempting to call this function will always fail.
-    function transferFrom(address, address, uint256)
-        external
-        alwaysRevert
-    {}
+    function transferFrom(address, address, uint256) pure external {
+        revert('ALWAYS FAILING');
+    }
 
     /// @notice DO NOT CALL. This is a soulbound NFT and cannot be transferred.
     ///         Attempting to call this function will always fail.
-    function safeTransferFrom(address, address, uint256)
-        external
-        alwaysRevert
-    {}
+    function safeTransferFrom(address, address, uint256) pure external {
+        revert('ALWAYS FAILING');
+    }
 
     /// @notice DO NOT CALL. This is a soulbound NFT and cannot be transferred.
     ///         Attempting to call this function will always fail.
-    function safeTransferFrom(address, address, uint256, bytes calldata)
-        external
-        alwaysRevert
-    {}
+    function safeTransferFrom(address, address, uint256, bytes calldata) pure external {
+        revert('ALWAYS FAILING');
+    }
 
 
     /// @notice DO NOT CALL. This is a soulbound NFT and cannot be transferred.
     ///         Attempting to call this function will always fail.
-    function approve(address, uint256)
-        external
-        alwaysRevert
-    {}
+    function approve(address, uint256) pure external {
+        revert('ALWAYS FAILING');
+    }
 
     /// @notice DO NOT CALL. This is a soulbound NFT and cannot be transferred.
     ///         Attempting to call this function will always fail.
-    function setApprovalForAll(address, bool)
-        external
-        alwaysRevert
-    {}
+    function setApprovalForAll(address, bool) pure external {
+        revert('ALWAYS FAILING');
+    }
 
     /// @notice This is a soulbound NFT and cannot be transferred.
     ///         Attempting to call this function will always return null.
```
