# SMART CONTRACT  
## Contratos a auditar:
### [oiebs_Faillapop_DAO.sol](../contracts/iebs_Faillapop_DAO.sol)
### [oiebs_Faillapop_ERC20.sol](../contracts/iebs_Faillapop_ERC20.sol)
### [oiebs_Faillapop_shop.sol](../contracts/iebs_Faillapop_shop.sol)
### [oiebs_Faillapop_vault.sol](../contracts/iebs_Faillapop_vault.sol)
## Interfaces a auditar :
### [IFP_DAO.sol](../interfaces/IFP_DAO.sol)
### [IFP_NFT.sol](../interfaces/IFP_NFT.sol)
### [IFP_Shop.sol](../interfaces/IFP_Shop.sol)
### [FP_Vault.sol](../interfaces/IFP_Vault.sol)




Summary
 - [arbitrary-send-eth](#arbitrary-send-eth) (2 results) (High)
 - [reentrancy-eth](#reentrancy-eth) (5 results) (High)
 - [uninitialized-state](#uninitialized-state) (1 results) (High)
 - [reentrancy-no-eth](#reentrancy-no-eth) (4 results) (Medium)
 - [unchecked-lowlevel](#unchecked-lowlevel) (1 results) (Medium)
 - [missing-zero-check](#missing-zero-check) (4 results) (Low)
 - [reentrancy-benign](#reentrancy-benign) (4 results) (Low)
 - [reentrancy-events](#reentrancy-events) (11 results) (Low)
 - [timestamp](#timestamp) (1 results) (Low)
 - [pragma](#pragma) (1 results) (Informational)
 - [dead-code](#dead-code) (1 results) (Informational)
 - [solc-version](#solc-version) (20 results) (Informational)
 - [low-level-calls](#low-level-calls) (5 results) (Informational)
 - [missing-inheritance](#missing-inheritance) (1 results) (Informational)
 - [naming-convention](#naming-convention) (16 results) (Informational)
 - [similar-names](#similar-names) (1 results) (Informational)
 - [too-many-digits](#too-many-digits) (1 results) (Informational)
 - [unused-state](#unused-state) (1 results) (Informational)
 - [constable-states](#constable-states) (1 results) (Optimization)
 - [immutable-states](#immutable-states) (3 results) (Optimization)
## arbitrary-send-eth
Impact: High
Confidence: Medium
 - [ ] ID-0
[FP_Shop.closeSale(uint256,bool)](../contracts/iebs_Faillapop_shop.sol#L344-L357) sends eth to arbitrary user
	Dangerous calls:
	- [(success) = address(seller).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L350)

../contracts/iebs_Faillapop_shop.sol#L344-L357


 - [ ] ID-1
[FP_Shop.reimburse(uint256)](../contracts/iebs_Faillapop_shop.sol#L377-L386) sends eth to arbitrary user
	Dangerous calls:
	- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)

../contracts/iebs_Faillapop_shop.sol#L377-L386


## reentrancy-eth
Impact: High
Confidence: Medium
 - [ ] ID-2
Reentrancy in [FP_Shop.removeMaliciousSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L321-L334):
	External calls:
	- [reimburse(itemId)](../contracts/iebs_Faillapop_shop.sol#L325)
		- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	- [reimburse(itemId)](../contracts/iebs_Faillapop_shop.sol#L327)
		- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	- [closeDispute(itemId)](../contracts/iebs_Faillapop_shop.sol#L328)
		- [daoContract.cancelDispute(dId)](../contracts/iebs_Faillapop_shop.sol#L417)
	- [closeSale(itemId,false)](../contracts/iebs_Faillapop_shop.sol#L332)
		- [(success) = address(seller).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L350)
		- [vaultContract.doUnlock(seller,price)](../contracts/iebs_Faillapop_shop.sol#L354)
	External calls sending eth:
	- [reimburse(itemId)](../contracts/iebs_Faillapop_shop.sol#L325)
		- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	- [reimburse(itemId)](../contracts/iebs_Faillapop_shop.sol#L327)
		- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	- [closeSale(itemId,false)](../contracts/iebs_Faillapop_shop.sol#L332)
		- [(success) = address(seller).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L350)
	State variables written after the call(s):
	- [closeSale(itemId,false)](../contracts/iebs_Faillapop_shop.sol#L332)
		- [delete offered_items[itemId]](../contracts/iebs_Faillapop_shop.sol#L356)
	[FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77) can be used in cross function reentrancies:
	- [FP_Shop.cancelActiveSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L259-L265)
	- [FP_Shop.closeSale(uint256,bool)](../contracts/iebs_Faillapop_shop.sol#L344-L357)
	- [FP_Shop.disputeSale(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L155-L163)
	- [FP_Shop.disputedSaleReply(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L294-L299)
	- [FP_Shop.doBuy(uint256)](../contracts/iebs_Faillapop_shop.sol#L138-L147)
	- [FP_Shop.endDispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L182-L199)
	- [FP_Shop.itemReceived(uint256)](../contracts/iebs_Faillapop_shop.sol#L169-L175)
	- [FP_Shop.modifySale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L232-L252)
	- [FP_Shop.newSale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L208-L223)
	- [FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77)
	- [FP_Shop.openDispute(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L394-L407)
	- [FP_Shop.query_sale(uint256)](../contracts/iebs_Faillapop_shop.sol#L433-L435)
	- [FP_Shop.reimburse(uint256)](../contracts/iebs_Faillapop_shop.sol#L377-L386)
	- [FP_Shop.removeMaliciousSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L321-L334)
	- [FP_Shop.returnItem(uint256)](../contracts/iebs_Faillapop_shop.sol#L306-L315)
	- [FP_Shop.setVacationMode(bool)](../contracts/iebs_Faillapop_shop.sol#L272-L285)

../contracts/iebs_Faillapop_shop.sol#L321-L334


 - [ ] ID-3
Reentrancy in [FP_Shop.closeSale(uint256,bool)](../contracts/iebs_Faillapop_shop.sol#L344-L357):
	External calls:
	- [(success) = address(seller).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L350)
	- [vaultContract.doUnlock(seller,price)](../contracts/iebs_Faillapop_shop.sol#L354)
	External calls sending eth:
	- [(success) = address(seller).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L350)
	State variables written after the call(s):
	- [delete offered_items[itemId]](../contracts/iebs_Faillapop_shop.sol#L356)
	[FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77) can be used in cross function reentrancies:
	- [FP_Shop.cancelActiveSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L259-L265)
	- [FP_Shop.closeSale(uint256,bool)](../contracts/iebs_Faillapop_shop.sol#L344-L357)
	- [FP_Shop.disputeSale(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L155-L163)
	- [FP_Shop.disputedSaleReply(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L294-L299)
	- [FP_Shop.doBuy(uint256)](../contracts/iebs_Faillapop_shop.sol#L138-L147)
	- [FP_Shop.endDispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L182-L199)
	- [FP_Shop.itemReceived(uint256)](../contracts/iebs_Faillapop_shop.sol#L169-L175)
	- [FP_Shop.modifySale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L232-L252)
	- [FP_Shop.newSale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L208-L223)
	- [FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77)
	- [FP_Shop.openDispute(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L394-L407)
	- [FP_Shop.query_sale(uint256)](../contracts/iebs_Faillapop_shop.sol#L433-L435)
	- [FP_Shop.reimburse(uint256)](../contracts/iebs_Faillapop_shop.sol#L377-L386)
	- [FP_Shop.removeMaliciousSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L321-L334)
	- [FP_Shop.returnItem(uint256)](../contracts/iebs_Faillapop_shop.sol#L306-L315)
	- [FP_Shop.setVacationMode(bool)](../contracts/iebs_Faillapop_shop.sol#L272-L285)

../contracts/iebs_Faillapop_shop.sol#L344-L357


 - [ ] ID-4
Reentrancy in [FP_Shop.returnItem(uint256)](../contracts/iebs_Faillapop_shop.sol#L306-L315):
	External calls:
	- [reimburse(itemId)](../contracts/iebs_Faillapop_shop.sol#L313)
		- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	- [closeSale(itemId,false)](../contracts/iebs_Faillapop_shop.sol#L314)
		- [(success) = address(seller).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L350)
		- [vaultContract.doUnlock(seller,price)](../contracts/iebs_Faillapop_shop.sol#L354)
	External calls sending eth:
	- [reimburse(itemId)](../contracts/iebs_Faillapop_shop.sol#L313)
		- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	- [closeSale(itemId,false)](../contracts/iebs_Faillapop_shop.sol#L314)
		- [(success) = address(seller).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L350)
	State variables written after the call(s):
	- [closeSale(itemId,false)](../contracts/iebs_Faillapop_shop.sol#L314)
		- [delete offered_items[itemId]](../contracts/iebs_Faillapop_shop.sol#L356)
	[FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77) can be used in cross function reentrancies:
	- [FP_Shop.cancelActiveSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L259-L265)
	- [FP_Shop.closeSale(uint256,bool)](../contracts/iebs_Faillapop_shop.sol#L344-L357)
	- [FP_Shop.disputeSale(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L155-L163)
	- [FP_Shop.disputedSaleReply(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L294-L299)
	- [FP_Shop.doBuy(uint256)](../contracts/iebs_Faillapop_shop.sol#L138-L147)
	- [FP_Shop.endDispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L182-L199)
	- [FP_Shop.itemReceived(uint256)](../contracts/iebs_Faillapop_shop.sol#L169-L175)
	- [FP_Shop.modifySale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L232-L252)
	- [FP_Shop.newSale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L208-L223)
	- [FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77)
	- [FP_Shop.openDispute(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L394-L407)
	- [FP_Shop.query_sale(uint256)](../contracts/iebs_Faillapop_shop.sol#L433-L435)
	- [FP_Shop.reimburse(uint256)](../contracts/iebs_Faillapop_shop.sol#L377-L386)
	- [FP_Shop.removeMaliciousSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L321-L334)
	- [FP_Shop.returnItem(uint256)](../contracts/iebs_Faillapop_shop.sol#L306-L315)
	- [FP_Shop.setVacationMode(bool)](../contracts/iebs_Faillapop_shop.sol#L272-L285)

../contracts/iebs_Faillapop_shop.sol#L306-L315


 - [ ] ID-5
Reentrancy in [FP_Vault.claimRewards()](../contracts/iebs_Faillapop_vault.sol#L184-L204):
	External calls:
	- [nftContract.call(abi.encodeWithSignature(checkPrivilege(address),msg.sender))](../contracts/iebs_Faillapop_vault.sol#L186-L191)
	- [(success) = address(msg.sender).call{value: amount}()](../contracts/iebs_Faillapop_vault.sol#L198)
	External calls sending eth:
	- [(success) = address(msg.sender).call{value: amount}()](../contracts/iebs_Faillapop_vault.sol#L198)
	State variables written after the call(s):
	- [rewardsClaimed[msg.sender] = max_claimable_amount](../contracts/iebs_Faillapop_vault.sol#L201)
	[FP_Vault.rewardsClaimed](../contracts/iebs_Faillapop_vault.sol#L45) can be used in cross function reentrancies:
	- [FP_Vault.claimRewards()](../contracts/iebs_Faillapop_vault.sol#L184-L204)
	- [FP_Vault.rewardsClaimed](../contracts/iebs_Faillapop_vault.sol#L45)

../contracts/iebs_Faillapop_vault.sol#L184-L204


 - [ ] ID-6
Reentrancy in [FP_Shop.removeMaliciousSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L321-L334):
	External calls:
	- [reimburse(itemId)](../contracts/iebs_Faillapop_shop.sol#L325)
		- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	- [reimburse(itemId)](../contracts/iebs_Faillapop_shop.sol#L327)
		- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	- [closeDispute(itemId)](../contracts/iebs_Faillapop_shop.sol#L328)
		- [daoContract.cancelDispute(dId)](../contracts/iebs_Faillapop_shop.sol#L417)
	- [closeSale(itemId,false)](../contracts/iebs_Faillapop_shop.sol#L332)
		- [(success) = address(seller).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L350)
		- [vaultContract.doUnlock(seller,price)](../contracts/iebs_Faillapop_shop.sol#L354)
	- [blacklist(offered_items[itemId].seller)](../contracts/iebs_Faillapop_shop.sol#L333)
		- [vaultContract.doSlash(user)](../contracts/iebs_Faillapop_shop.sol#L367)
	External calls sending eth:
	- [reimburse(itemId)](../contracts/iebs_Faillapop_shop.sol#L325)
		- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	- [reimburse(itemId)](../contracts/iebs_Faillapop_shop.sol#L327)
		- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	- [closeSale(itemId,false)](../contracts/iebs_Faillapop_shop.sol#L332)
		- [(success) = address(seller).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L350)
	State variables written after the call(s):
	- [blacklist(offered_items[itemId].seller)](../contracts/iebs_Faillapop_shop.sol#L333)
		- [_roles[role].hasRole[account] = true](../node_modules/@openzeppelin/contracts/access/AccessControl.sol#L185)
	[AccessControl._roles](../node_modules/@openzeppelin/contracts/access/AccessControl.sol#L55) can be used in cross function reentrancies:
	- [AccessControl._grantRole(bytes32,address)](../node_modules/@openzeppelin/contracts/access/AccessControl.sol#L183-L191)
	- [AccessControl._revokeRole(bytes32,address)](../node_modules/@openzeppelin/contracts/access/AccessControl.sol#L200-L208)
	- [AccessControl.getRoleAdmin(bytes32)](../node_modules/@openzeppelin/contracts/access/AccessControl.sol#L106-L108)
	- [AccessControl.hasRole(bytes32,address)](../node_modules/@openzeppelin/contracts/access/AccessControl.sol#L78-L80)

../contracts/iebs_Faillapop_shop.sol#L321-L334


## uninitialized-state
Impact: High
Confidence: High
 - [ ] ID-7
[FP_DAO.quorum](../contracts/iebs_Faillapop_DAO.sol#L63) is never initialized. It is used in:
	- [FP_DAO.endDispute(uint256)](../contracts/iebs_Faillapop_DAO.sol#L196-L214)

../contracts/iebs_Faillapop_DAO.sol#L63


## reentrancy-no-eth
Impact: Medium
Confidence: Medium
 - [ ] ID-8
Reentrancy in [FP_DAO.endDispute(uint256)](../contracts/iebs_Faillapop_DAO.sol#L196-L214):
	External calls:
	- [buyerWins(itemId)](../contracts/iebs_Faillapop_DAO.sol#L204)
		- [shopContract.returnItem(itemId)](../contracts/iebs_Faillapop_DAO.sol#L274)
	- [sellerWins(itemId)](../contracts/iebs_Faillapop_DAO.sol#L207)
		- [shopContract.endDispute(itemId)](../contracts/iebs_Faillapop_DAO.sol#L282)
	State variables written after the call(s):
	- [delete disputes[disputeId]](../contracts/iebs_Faillapop_DAO.sol#L211)
	[FP_DAO.disputes](../contracts/iebs_Faillapop_DAO.sol#L45) can be used in cross function reentrancies:
	- [FP_DAO.cancelDispute(uint256)](../contracts/iebs_Faillapop_DAO.sol#L221-L227)
	- [FP_DAO.castVote(uint256,bool)](../contracts/iebs_Faillapop_DAO.sol#L146-L162)
	- [FP_DAO.disputes](../contracts/iebs_Faillapop_DAO.sol#L45)
	- [FP_DAO.endDispute(uint256)](../contracts/iebs_Faillapop_DAO.sol#L196-L214)
	- [FP_DAO.newDispute(uint256,string,string)](../contracts/iebs_Faillapop_DAO.sol#L170-L190)
	- [FP_DAO.query_dispute(uint256)](../contracts/iebs_Faillapop_DAO.sol#L298-L300)

../contracts/iebs_Faillapop_DAO.sol#L196-L214


 - [ ] ID-9
Reentrancy in [FP_Shop.modifySale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L232-L252):
	External calls:
	- [vaultContract.doUnlock(msg.sender,priceDifference)](../contracts/iebs_Faillapop_shop.sol#L240)
	- [vaultContract.doLock(msg.sender,priceDifference)](../contracts/iebs_Faillapop_shop.sol#L243)
	State variables written after the call(s):
	- [offered_items[itemId].title = newTitle](../contracts/iebs_Faillapop_shop.sol#L247)
	[FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77) can be used in cross function reentrancies:
	- [FP_Shop.cancelActiveSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L259-L265)
	- [FP_Shop.closeSale(uint256,bool)](../contracts/iebs_Faillapop_shop.sol#L344-L357)
	- [FP_Shop.disputeSale(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L155-L163)
	- [FP_Shop.disputedSaleReply(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L294-L299)
	- [FP_Shop.doBuy(uint256)](../contracts/iebs_Faillapop_shop.sol#L138-L147)
	- [FP_Shop.endDispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L182-L199)
	- [FP_Shop.itemReceived(uint256)](../contracts/iebs_Faillapop_shop.sol#L169-L175)
	- [FP_Shop.modifySale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L232-L252)
	- [FP_Shop.newSale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L208-L223)
	- [FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77)
	- [FP_Shop.openDispute(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L394-L407)
	- [FP_Shop.query_sale(uint256)](../contracts/iebs_Faillapop_shop.sol#L433-L435)
	- [FP_Shop.reimburse(uint256)](../contracts/iebs_Faillapop_shop.sol#L377-L386)
	- [FP_Shop.removeMaliciousSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L321-L334)
	- [FP_Shop.returnItem(uint256)](../contracts/iebs_Faillapop_shop.sol#L306-L315)
	- [FP_Shop.setVacationMode(bool)](../contracts/iebs_Faillapop_shop.sol#L272-L285)
	- [offered_items[itemId].description = newDesc](../contracts/iebs_Faillapop_shop.sol#L248)
	[FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77) can be used in cross function reentrancies:
	- [FP_Shop.cancelActiveSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L259-L265)
	- [FP_Shop.closeSale(uint256,bool)](../contracts/iebs_Faillapop_shop.sol#L344-L357)
	- [FP_Shop.disputeSale(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L155-L163)
	- [FP_Shop.disputedSaleReply(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L294-L299)
	- [FP_Shop.doBuy(uint256)](../contracts/iebs_Faillapop_shop.sol#L138-L147)
	- [FP_Shop.endDispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L182-L199)
	- [FP_Shop.itemReceived(uint256)](../contracts/iebs_Faillapop_shop.sol#L169-L175)
	- [FP_Shop.modifySale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L232-L252)
	- [FP_Shop.newSale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L208-L223)
	- [FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77)
	- [FP_Shop.openDispute(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L394-L407)
	- [FP_Shop.query_sale(uint256)](../contracts/iebs_Faillapop_shop.sol#L433-L435)
	- [FP_Shop.reimburse(uint256)](../contracts/iebs_Faillapop_shop.sol#L377-L386)
	- [FP_Shop.removeMaliciousSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L321-L334)
	- [FP_Shop.returnItem(uint256)](../contracts/iebs_Faillapop_shop.sol#L306-L315)
	- [FP_Shop.setVacationMode(bool)](../contracts/iebs_Faillapop_shop.sol#L272-L285)
	- [offered_items[itemId].price = newPrice](../contracts/iebs_Faillapop_shop.sol#L249)
	[FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77) can be used in cross function reentrancies:
	- [FP_Shop.cancelActiveSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L259-L265)
	- [FP_Shop.closeSale(uint256,bool)](../contracts/iebs_Faillapop_shop.sol#L344-L357)
	- [FP_Shop.disputeSale(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L155-L163)
	- [FP_Shop.disputedSaleReply(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L294-L299)
	- [FP_Shop.doBuy(uint256)](../contracts/iebs_Faillapop_shop.sol#L138-L147)
	- [FP_Shop.endDispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L182-L199)
	- [FP_Shop.itemReceived(uint256)](../contracts/iebs_Faillapop_shop.sol#L169-L175)
	- [FP_Shop.modifySale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L232-L252)
	- [FP_Shop.newSale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L208-L223)
	- [FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77)
	- [FP_Shop.openDispute(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L394-L407)
	- [FP_Shop.query_sale(uint256)](../contracts/iebs_Faillapop_shop.sol#L433-L435)
	- [FP_Shop.reimburse(uint256)](../contracts/iebs_Faillapop_shop.sol#L377-L386)
	- [FP_Shop.removeMaliciousSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L321-L334)
	- [FP_Shop.returnItem(uint256)](../contracts/iebs_Faillapop_shop.sol#L306-L315)
	- [FP_Shop.setVacationMode(bool)](../contracts/iebs_Faillapop_shop.sol#L272-L285)

../contracts/iebs_Faillapop_shop.sol#L232-L252


 - [ ] ID-10
Reentrancy in [FP_Shop.closeDispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L414-L420):
	External calls:
	- [daoContract.cancelDispute(dId)](../contracts/iebs_Faillapop_shop.sol#L417)
	State variables written after the call(s):
	- [delete disputed_items[itemId]](../contracts/iebs_Faillapop_shop.sol#L419)
	[FP_Shop.disputed_items](../contracts/iebs_Faillapop_shop.sol#L81) can be used in cross function reentrancies:
	- [FP_Shop.closeDispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L414-L420)
	- [FP_Shop.disputeSale(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L155-L163)
	- [FP_Shop.disputed_items](../contracts/iebs_Faillapop_shop.sol#L81)
	- [FP_Shop.endDispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L182-L199)
	- [FP_Shop.openDispute(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L394-L407)
	- [FP_Shop.query_dispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L429-L431)

../contracts/iebs_Faillapop_shop.sol#L414-L420


 - [ ] ID-11
Reentrancy in [FP_Shop.endDispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L182-L199):
	External calls:
	- [closeDispute(itemId)](../contracts/iebs_Faillapop_shop.sol#L187)
		- [daoContract.cancelDispute(dId)](../contracts/iebs_Faillapop_shop.sol#L417)
	State variables written after the call(s):
	- [delete disputed_items[itemId]](../contracts/iebs_Faillapop_shop.sol#L194)
	[FP_Shop.disputed_items](../contracts/iebs_Faillapop_shop.sol#L81) can be used in cross function reentrancies:
	- [FP_Shop.closeDispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L414-L420)
	- [FP_Shop.disputeSale(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L155-L163)
	- [FP_Shop.disputed_items](../contracts/iebs_Faillapop_shop.sol#L81)
	- [FP_Shop.endDispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L182-L199)
	- [FP_Shop.openDispute(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L394-L407)
	- [FP_Shop.query_dispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L429-L431)
	- [offered_items[itemId].state = State.Sold](../contracts/iebs_Faillapop_shop.sol#L195)
	[FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77) can be used in cross function reentrancies:
	- [FP_Shop.cancelActiveSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L259-L265)
	- [FP_Shop.closeSale(uint256,bool)](../contracts/iebs_Faillapop_shop.sol#L344-L357)
	- [FP_Shop.disputeSale(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L155-L163)
	- [FP_Shop.disputedSaleReply(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L294-L299)
	- [FP_Shop.doBuy(uint256)](../contracts/iebs_Faillapop_shop.sol#L138-L147)
	- [FP_Shop.endDispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L182-L199)
	- [FP_Shop.itemReceived(uint256)](../contracts/iebs_Faillapop_shop.sol#L169-L175)
	- [FP_Shop.modifySale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L232-L252)
	- [FP_Shop.newSale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L208-L223)
	- [FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77)
	- [FP_Shop.openDispute(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L394-L407)
	- [FP_Shop.query_sale(uint256)](../contracts/iebs_Faillapop_shop.sol#L433-L435)
	- [FP_Shop.reimburse(uint256)](../contracts/iebs_Faillapop_shop.sol#L377-L386)
	- [FP_Shop.removeMaliciousSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L321-L334)
	- [FP_Shop.returnItem(uint256)](../contracts/iebs_Faillapop_shop.sol#L306-L315)
	- [FP_Shop.setVacationMode(bool)](../contracts/iebs_Faillapop_shop.sol#L272-L285)

../contracts/iebs_Faillapop_shop.sol#L182-L199


## unchecked-lowlevel
Impact: Medium
Confidence: Medium
 - [ ] ID-12
[FP_Vault.claimRewards()](../contracts/iebs_Faillapop_vault.sol#L184-L204) ignores return value by [nftContract.call(abi.encodeWithSignature(checkPrivilege(address),msg.sender))](../contracts/iebs_Faillapop_vault.sol#L186-L191)

../contracts/iebs_Faillapop_vault.sol#L184-L204


## missing-zero-check
Impact: Low
Confidence: Medium
 - [ ] ID-13
[FP_Vault.updateConfig(address,address,address).newNft](../contracts/iebs_Faillapop_vault.sol#L171) lacks a zero-check on :
		- [nftContract = newNft](../contracts/iebs_Faillapop_vault.sol#L174)

../contracts/iebs_Faillapop_vault.sol#L171


 - [ ] ID-14
[FP_Vault.constructor(address,address,address).token](../contracts/iebs_Faillapop_vault.sol#L93) lacks a zero-check on :
		- [nftContract = token](../contracts/iebs_Faillapop_vault.sol#L98)

../contracts/iebs_Faillapop_vault.sol#L93


 - [ ] ID-15
[FP_DAO.updateConfig(string,string,address,address).newShop](../contracts/iebs_Faillapop_DAO.sol#L129) lacks a zero-check on :
		- [shop_addr = newShop](../contracts/iebs_Faillapop_DAO.sol#L133)

../contracts/iebs_Faillapop_DAO.sol#L129


 - [ ] ID-16
[FP_DAO.constructor(string,address,address,address).shop](../contracts/iebs_Faillapop_DAO.sol#L111) lacks a zero-check on :
		- [shop_addr = shop](../contracts/iebs_Faillapop_DAO.sol#L113)

../contracts/iebs_Faillapop_DAO.sol#L111


## reentrancy-benign
Impact: Low
Confidence: Medium
 - [ ] ID-17
Reentrancy in [FP_Vault.distributeSlashing(uint256)](../contracts/iebs_Faillapop_vault.sol#L210-L218):
	External calls:
	- [(data) = nftContract.call(abi.encodeWithSignature(totalPowersellers()))](../contracts/iebs_Faillapop_vault.sol#L213)
	State variables written after the call(s):
	- [max_claimable_amount = newMax](../contracts/iebs_Faillapop_vault.sol#L217)

../contracts/iebs_Faillapop_vault.sol#L210-L218


 - [ ] ID-18
Reentrancy in [FP_DAO.endDispute(uint256)](../contracts/iebs_Faillapop_DAO.sol#L196-L214):
	External calls:
	- [sellerWins(itemId)](../contracts/iebs_Faillapop_DAO.sol#L207)
		- [shopContract.endDispute(itemId)](../contracts/iebs_Faillapop_DAO.sol#L282)
	State variables written after the call(s):
	- [disputeResult[disputeId] = 2](../contracts/iebs_Faillapop_DAO.sol#L208)

../contracts/iebs_Faillapop_DAO.sol#L196-L214


 - [ ] ID-19
Reentrancy in [FP_DAO.endDispute(uint256)](../contracts/iebs_Faillapop_DAO.sol#L196-L214):
	External calls:
	- [buyerWins(itemId)](../contracts/iebs_Faillapop_DAO.sol#L204)
		- [shopContract.returnItem(itemId)](../contracts/iebs_Faillapop_DAO.sol#L274)
	State variables written after the call(s):
	- [disputeResult[disputeId] = 1](../contracts/iebs_Faillapop_DAO.sol#L205)

../contracts/iebs_Faillapop_DAO.sol#L196-L214


 - [ ] ID-20
Reentrancy in [FP_Shop.newSale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L208-L223):
	External calls:
	- [vaultContract.doLock(msg.sender,price)](../contracts/iebs_Faillapop_shop.sol#L218)
	State variables written after the call(s):
	- [offerIndex += 1](../contracts/iebs_Faillapop_shop.sol#L220)

../contracts/iebs_Faillapop_shop.sol#L208-L223


## reentrancy-events
Impact: Low
Confidence: Medium
 - [ ] ID-21
Reentrancy in [FP_DAO.endDispute(uint256)](../contracts/iebs_Faillapop_DAO.sol#L196-L214):
	External calls:
	- [buyerWins(itemId)](../contracts/iebs_Faillapop_DAO.sol#L204)
		- [shopContract.returnItem(itemId)](../contracts/iebs_Faillapop_DAO.sol#L274)
	- [sellerWins(itemId)](../contracts/iebs_Faillapop_DAO.sol#L207)
		- [shopContract.endDispute(itemId)](../contracts/iebs_Faillapop_DAO.sol#L282)
	Event emitted after the call(s):
	- [EndDispute(disputeId,itemId)](../contracts/iebs_Faillapop_DAO.sol#L213)

../contracts/iebs_Faillapop_DAO.sol#L196-L214


 - [ ] ID-22
Reentrancy in [FP_Vault.doSlash(address)](../contracts/iebs_Faillapop_vault.sol#L154-L163):
	External calls:
	- [distributeSlashing(amount)](../contracts/iebs_Faillapop_vault.sol#L160)
		- [(data) = nftContract.call(abi.encodeWithSignature(totalPowersellers()))](../contracts/iebs_Faillapop_vault.sol#L213)
	Event emitted after the call(s):
	- [Slashed(badUser,amount)](../contracts/iebs_Faillapop_vault.sol#L162)

../contracts/iebs_Faillapop_vault.sol#L154-L163


 - [ ] ID-23
Reentrancy in [FP_Vault.doUnstake(uint256)](../contracts/iebs_Faillapop_vault.sol#L115-L124):
	External calls:
	- [(success) = address(msg.sender).call{value: amount}()](../contracts/iebs_Faillapop_vault.sol#L120)
	Event emitted after the call(s):
	- [Unstake(msg.sender,amount)](../contracts/iebs_Faillapop_vault.sol#L123)

../contracts/iebs_Faillapop_vault.sol#L115-L124


 - [ ] ID-24
Reentrancy in [FP_Shop.modifySale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L232-L252):
	External calls:
	- [vaultContract.doUnlock(msg.sender,priceDifference)](../contracts/iebs_Faillapop_shop.sol#L240)
	- [vaultContract.doLock(msg.sender,priceDifference)](../contracts/iebs_Faillapop_shop.sol#L243)
	Event emitted after the call(s):
	- [ModifyItem(itemId,newTitle)](../contracts/iebs_Faillapop_shop.sol#L251)

../contracts/iebs_Faillapop_shop.sol#L232-L252


 - [ ] ID-25
Reentrancy in [FP_Shop.blacklist(address)](../contracts/iebs_Faillapop_shop.sol#L364-L370):
	External calls:
	- [vaultContract.doSlash(user)](../contracts/iebs_Faillapop_shop.sol#L367)
	Event emitted after the call(s):
	- [BlacklistSeller(user)](../contracts/iebs_Faillapop_shop.sol#L369)

../contracts/iebs_Faillapop_shop.sol#L364-L370


 - [ ] ID-26
Reentrancy in [FP_Shop.reimburse(uint256)](../contracts/iebs_Faillapop_shop.sol#L377-L386):
	External calls:
	- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	Event emitted after the call(s):
	- [Reimburse(buyer,price)](../contracts/iebs_Faillapop_shop.sol#L385)

../contracts/iebs_Faillapop_shop.sol#L377-L386


 - [ ] ID-27
Reentrancy in [FP_Shop.removeMaliciousSale(uint256)](../contracts/iebs_Faillapop_shop.sol#L321-L334):
	External calls:
	- [reimburse(itemId)](../contracts/iebs_Faillapop_shop.sol#L325)
		- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	- [reimburse(itemId)](../contracts/iebs_Faillapop_shop.sol#L327)
		- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	- [closeDispute(itemId)](../contracts/iebs_Faillapop_shop.sol#L328)
		- [daoContract.cancelDispute(dId)](../contracts/iebs_Faillapop_shop.sol#L417)
	- [closeSale(itemId,false)](../contracts/iebs_Faillapop_shop.sol#L332)
		- [(success) = address(seller).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L350)
		- [vaultContract.doUnlock(seller,price)](../contracts/iebs_Faillapop_shop.sol#L354)
	- [blacklist(offered_items[itemId].seller)](../contracts/iebs_Faillapop_shop.sol#L333)
		- [vaultContract.doSlash(user)](../contracts/iebs_Faillapop_shop.sol#L367)
	External calls sending eth:
	- [reimburse(itemId)](../contracts/iebs_Faillapop_shop.sol#L325)
		- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	- [reimburse(itemId)](../contracts/iebs_Faillapop_shop.sol#L327)
		- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)
	- [closeSale(itemId,false)](../contracts/iebs_Faillapop_shop.sol#L332)
		- [(success) = address(seller).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L350)
	Event emitted after the call(s):
	- [BlacklistSeller(user)](../contracts/iebs_Faillapop_shop.sol#L369)
		- [blacklist(offered_items[itemId].seller)](../contracts/iebs_Faillapop_shop.sol#L333)
	- [RoleGranted(role,account,_msgSender())](../node_modules/@openzeppelin/contracts/access/AccessControl.sol#L186)
		- [blacklist(offered_items[itemId].seller)](../contracts/iebs_Faillapop_shop.sol#L333)

../contracts/iebs_Faillapop_shop.sol#L321-L334


 - [ ] ID-28
Reentrancy in [FP_Vault.claimRewards()](../contracts/iebs_Faillapop_vault.sol#L184-L204):
	External calls:
	- [nftContract.call(abi.encodeWithSignature(checkPrivilege(address),msg.sender))](../contracts/iebs_Faillapop_vault.sol#L186-L191)
	- [(success) = address(msg.sender).call{value: amount}()](../contracts/iebs_Faillapop_vault.sol#L198)
	External calls sending eth:
	- [(success) = address(msg.sender).call{value: amount}()](../contracts/iebs_Faillapop_vault.sol#L198)
	Event emitted after the call(s):
	- [RewardsClaimed(msg.sender,amount)](../contracts/iebs_Faillapop_vault.sol#L203)

../contracts/iebs_Faillapop_vault.sol#L184-L204


 - [ ] ID-29
Reentrancy in [FP_Shop.newSale(uint256,string,string,uint256)](../contracts/iebs_Faillapop_shop.sol#L208-L223):
	External calls:
	- [vaultContract.doLock(msg.sender,price)](../contracts/iebs_Faillapop_shop.sol#L218)
	Event emitted after the call(s):
	- [NewItem(currentId,title)](../contracts/iebs_Faillapop_shop.sol#L222)

../contracts/iebs_Faillapop_shop.sol#L208-L223


 - [ ] ID-30
Reentrancy in [FP_Shop.openDispute(uint256,string)](../contracts/iebs_Faillapop_shop.sol#L394-L407):
	External calls:
	- [dispute.disputeId = daoContract.newDispute(itemId,dispute.buyerReasoning,dispute.sellerReasoning)](../contracts/iebs_Faillapop_shop.sol#L399-L403)
	Event emitted after the call(s):
	- [OpenDispute(buyer,itemId)](../contracts/iebs_Faillapop_shop.sol#L406)

../contracts/iebs_Faillapop_shop.sol#L394-L407


 - [ ] ID-31
Reentrancy in [FP_DAO.lotteryNFT(address)](../contracts/iebs_Faillapop_DAO.sol#L251-L267):
	External calls:
	- [nftContract.mintCoolNFT(user)](../contracts/iebs_Faillapop_DAO.sol#L262)
	Event emitted after the call(s):
	- [AwardNFT(user)](../contracts/iebs_Faillapop_DAO.sol#L266)

../contracts/iebs_Faillapop_DAO.sol#L251-L267


## timestamp
Impact: Low
Confidence: Medium
 - [ ] ID-32
[FP_DAO.lotteryNFT(address)](../contracts/iebs_Faillapop_DAO.sol#L251-L267) uses timestamp for comparisons
	Dangerous comparisons:
	- [randomNumber < THRESHOLD](../contracts/iebs_Faillapop_DAO.sol#L261)

../contracts/iebs_Faillapop_DAO.sol#L251-L267


## pragma
Impact: Informational
Confidence: High
 - [ ] ID-33
Different versions of Solidity are used:
	- Version used: ['^0.8.13', '^0.8.20']
	- [^0.8.13](../contracts/interfaces/IFP_DAO.sol#L3)
	- [^0.8.13](../contracts/interfaces/IFP_NFT.sol#L3)
	- [^0.8.13](../contracts/interfaces/IFP_Shop.sol#L3)
	- [^0.8.13](../contracts/interfaces/IFP_Vault.sol#L3)
	- [^0.8.20](../node_modules/@openzeppelin/contracts/access/AccessControl.sol#L4)
	- [^0.8.20](../node_modules/@openzeppelin/contracts/access/IAccessControl.sol#L4)
	- [^0.8.20](../node_modules/@openzeppelin/contracts/interfaces/draft-IERC6093.sol#L3)
	- [^0.8.20](../node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L4)
	- [^0.8.20](../node_modules/@openzeppelin/contracts/token/ERC20/IERC20.sol#L4)
	- [^0.8.20](../node_modules/@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol#L4)
	- [^0.8.20](../node_modules/@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol#L4)
	- [^0.8.20](../node_modules/@openzeppelin/contracts/utils/Context.sol#L4)
	- [^0.8.20](../node_modules/@openzeppelin/contracts/utils/Pausable.sol#L4)
	- [^0.8.20](../node_modules/@openzeppelin/contracts/utils/introspection/ERC165.sol#L4)
	- [^0.8.20](../node_modules/@openzeppelin/contracts/utils/introspection/IERC165.sol#L4)
	- [^0.8.20](../contracts/iebs_Faillapop_DAO.sol#L2)
	- [^0.8.20](../contracts/iebs_Faillapop_ERC20.sol#L2)
	- [^0.8.20](../contracts/iebs_Faillapop_shop.sol#L2)
	- [^0.8.20](../contracts/iebs_Faillapop_vault.sol#L2)

../contracts/interfaces/IFP_DAO.sol#L3


## dead-code
Impact: Informational
Confidence: Medium
 - [ ] ID-34
[FP_Token._beforeTokenTransfer(address,address,uint256)](../contracts/iebs_Faillapop_ERC20.sol#L36-L43) is never used and should be removed

../contracts/iebs_Faillapop_ERC20.sol#L36-L43


## solc-version
Impact: Informational
Confidence: High
 - [ ] ID-35
Pragma version[^0.8.20](../contracts/iebs_Faillapop_shop.sol#L2) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../contracts/iebs_Faillapop_shop.sol#L2


 - [ ] ID-36
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/token/ERC20/IERC20.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/token/ERC20/IERC20.sol#L4


 - [ ] ID-37
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol#L4


 - [ ] ID-38
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/interfaces/draft-IERC6093.sol#L3) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/interfaces/draft-IERC6093.sol#L3


 - [ ] ID-39
Pragma version[^0.8.20](../contracts/iebs_Faillapop_vault.sol#L2) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../contracts/iebs_Faillapop_vault.sol#L2


 - [ ] ID-40
Pragma version[^0.8.13](../contracts/interfaces/IFP_DAO.sol#L3) allows old versions

../contracts/interfaces/IFP_DAO.sol#L3


 - [ ] ID-41
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/utils/Pausable.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/utils/Pausable.sol#L4


 - [ ] ID-42
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L4


 - [ ] ID-43
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/access/AccessControl.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/access/AccessControl.sol#L4


 - [ ] ID-44
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/access/IAccessControl.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/access/IAccessControl.sol#L4


 - [ ] ID-45
solc-0.8.20 is not recommended for deployment

 - [ ] ID-46
Pragma version[^0.8.13](../contracts/interfaces/IFP_Shop.sol#L3) allows old versions

../contracts/interfaces/IFP_Shop.sol#L3


 - [ ] ID-47
Pragma version[^0.8.13](../contracts/interfaces/IFP_Vault.sol#L3) allows old versions

../contracts/interfaces/IFP_Vault.sol#L3


 - [ ] ID-48
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol#L4


 - [ ] ID-49
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/utils/introspection/IERC165.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/utils/introspection/IERC165.sol#L4


 - [ ] ID-50
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/utils/Context.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/utils/Context.sol#L4


 - [ ] ID-51
Pragma version[^0.8.20](../contracts/iebs_Faillapop_ERC20.sol#L2) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../contracts/iebs_Faillapop_ERC20.sol#L2


 - [ ] ID-52
Pragma version[^0.8.20](../contracts/iebs_Faillapop_DAO.sol#L2) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../contracts/iebs_Faillapop_DAO.sol#L2


 - [ ] ID-53
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/utils/introspection/ERC165.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/utils/introspection/ERC165.sol#L4


 - [ ] ID-54
Pragma version[^0.8.13](../contracts/interfaces/IFP_NFT.sol#L3) allows old versions

../contracts/interfaces/IFP_NFT.sol#L3


## low-level-calls
Impact: Informational
Confidence: High
 - [ ] ID-55
Low level call in [FP_Shop.closeSale(uint256,bool)](../contracts/iebs_Faillapop_shop.sol#L344-L357):
	- [(success) = address(seller).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L350)

../contracts/iebs_Faillapop_shop.sol#L344-L357


 - [ ] ID-56
Low level call in [FP_Vault.claimRewards()](../contracts/iebs_Faillapop_vault.sol#L184-L204):
	- [nftContract.call(abi.encodeWithSignature(checkPrivilege(address),msg.sender))](../contracts/iebs_Faillapop_vault.sol#L186-L191)
	- [(success) = address(msg.sender).call{value: amount}()](../contracts/iebs_Faillapop_vault.sol#L198)

../contracts/iebs_Faillapop_vault.sol#L184-L204


 - [ ] ID-57
Low level call in [FP_Vault.doUnstake(uint256)](../contracts/iebs_Faillapop_vault.sol#L115-L124):
	- [(success) = address(msg.sender).call{value: amount}()](../contracts/iebs_Faillapop_vault.sol#L120)

../contracts/iebs_Faillapop_vault.sol#L115-L124


 - [ ] ID-58
Low level call in [FP_Shop.reimburse(uint256)](../contracts/iebs_Faillapop_shop.sol#L377-L386):
	- [(success) = address(buyer).call{value: price}()](../contracts/iebs_Faillapop_shop.sol#L382)

../contracts/iebs_Faillapop_shop.sol#L377-L386


 - [ ] ID-59
Low level call in [FP_Vault.distributeSlashing(uint256)](../contracts/iebs_Faillapop_vault.sol#L210-L218):
	- [(data) = nftContract.call(abi.encodeWithSignature(totalPowersellers()))](../contracts/iebs_Faillapop_vault.sol#L213)

../contracts/iebs_Faillapop_vault.sol#L210-L218


## missing-inheritance
Impact: Informational
Confidence: High
 - [ ] ID-60
[FP_Vault](../contracts/iebs_Faillapop_vault.sol#L18-L244) should inherit from [IFP_Vault](../contracts/interfaces/IFP_Vault.sol#L14-L56)

../contracts/iebs_Faillapop_vault.sol#L18-L244


## naming-convention
Impact: Informational
Confidence: High
 - [ ] ID-61
Function [FP_Shop.query_dispute(uint256)](../contracts/iebs_Faillapop_shop.sol#L429-L431) is not in mixedCase

../contracts/iebs_Faillapop_shop.sol#L429-L431


 - [ ] ID-62
Variable [FP_Shop.disputed_items](../contracts/iebs_Faillapop_shop.sol#L81) is not in mixedCase

../contracts/iebs_Faillapop_shop.sol#L81


 - [ ] ID-63
Parameter [FP_Shop.setVacationMode(bool)._vacationMode](../contracts/iebs_Faillapop_shop.sol#L272) is not in mixedCase

../contracts/iebs_Faillapop_shop.sol#L272


 - [ ] ID-64
Function [FP_DAO.query_dispute(uint256)](../contracts/iebs_Faillapop_DAO.sol#L298-L300) is not in mixedCase

../contracts/iebs_Faillapop_DAO.sol#L298-L300


 - [ ] ID-65
Function [FP_Shop.query_sale(uint256)](../contracts/iebs_Faillapop_shop.sol#L433-L435) is not in mixedCase

../contracts/iebs_Faillapop_shop.sol#L433-L435


 - [ ] ID-66
Variable [FP_Vault.max_claimable_amount](../contracts/iebs_Faillapop_vault.sol#L43) is not in mixedCase

../contracts/iebs_Faillapop_vault.sol#L43


 - [ ] ID-67
Contract [IFP_Shop](../contracts/interfaces/IFP_Shop.sol#L14-L90) is not in CapWords

../contracts/interfaces/IFP_Shop.sol#L14-L90


 - [ ] ID-68
Variable [FP_Shop.offered_items](../contracts/iebs_Faillapop_shop.sol#L77) is not in mixedCase

../contracts/iebs_Faillapop_shop.sol#L77


 - [ ] ID-69
Contract [IFP_NFT](../contracts/interfaces/IFP_NFT.sol#L5-L15) is not in CapWords

../contracts/interfaces/IFP_NFT.sol#L5-L15


 - [ ] ID-70
Contract [IFP_DAO](../contracts/interfaces/IFP_DAO.sol#L12-L63) is not in CapWords

../contracts/interfaces/IFP_DAO.sol#L12-L63


 - [ ] ID-71
Variable [FP_DAO.shop_addr](../contracts/iebs_Faillapop_DAO.sol#L55) is not in mixedCase

../contracts/iebs_Faillapop_DAO.sol#L55


 - [ ] ID-72
Contract [FP_Shop](../contracts/iebs_Faillapop_shop.sol#L16-L438) is not in CapWords

../contracts/iebs_Faillapop_shop.sol#L16-L438


 - [ ] ID-73
Contract [IFP_Vault](../contracts/interfaces/IFP_Vault.sol#L14-L56) is not in CapWords

../contracts/interfaces/IFP_Vault.sol#L14-L56


 - [ ] ID-74
Contract [FP_Vault](../contracts/iebs_Faillapop_vault.sol#L18-L244) is not in CapWords

../contracts/iebs_Faillapop_vault.sol#L18-L244


 - [ ] ID-75
Contract [FP_Token](../contracts/iebs_Faillapop_ERC20.sol#L13-L45) is not in CapWords

../contracts/iebs_Faillapop_ERC20.sol#L13-L45


 - [ ] ID-76
Contract [FP_DAO](../contracts/iebs_Faillapop_DAO.sol#L17-L303) is not in CapWords

../contracts/iebs_Faillapop_DAO.sol#L17-L303


## similar-names
Impact: Informational
Confidence: Medium
 - [ ] ID-77
Variable [FP_DAO.fptContract](../contracts/iebs_Faillapop_DAO.sol#L61) is too similar to [FP_DAO.nftContract](../contracts/iebs_Faillapop_DAO.sol#L59)

../contracts/iebs_Faillapop_DAO.sol#L61


## too-many-digits
Impact: Informational
Confidence: Medium
 - [ ] ID-78
[FP_Token.constructor()](../contracts/iebs_Faillapop_ERC20.sol#L17-L22) uses literals with too many digits:
	- [_mint(msg.sender,1000000 * 10 ** decimals())](../contracts/iebs_Faillapop_ERC20.sol#L20)

../contracts/iebs_Faillapop_ERC20.sol#L17-L22


## unused-state
Impact: Informational
Confidence: High
 - [ ] ID-79
[FP_DAO.DEFAULT_QUORUM](../contracts/iebs_Faillapop_DAO.sol#L23) is never used in [FP_DAO](../contracts/iebs_Faillapop_DAO.sol#L17-L303)

../contracts/iebs_Faillapop_DAO.sol#L23


## constable-states
Impact: Optimization
Confidence: High
 - [ ] ID-80
[FP_DAO.quorum](../contracts/iebs_Faillapop_DAO.sol#L63) should be constant 

../contracts/iebs_Faillapop_DAO.sol#L63


## immutable-states
Impact: Optimization
Confidence: High
 - [ ] ID-81
[FP_DAO.fptContract](../contracts/iebs_Faillapop_DAO.sol#L61) should be immutable 

../contracts/iebs_Faillapop_DAO.sol#L61


 - [ ] ID-82
[FP_Shop.daoContract](../contracts/iebs_Faillapop_shop.sol#L87) should be immutable 

../contracts/iebs_Faillapop_shop.sol#L87


 - [ ] ID-83
[FP_Shop.vaultContract](../contracts/iebs_Faillapop_shop.sol#L85) should be immutable 

../contracts/iebs_Faillapop_shop.sol#L85


