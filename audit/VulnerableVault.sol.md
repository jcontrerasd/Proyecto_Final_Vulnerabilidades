# SMART CONTRACT VulnerableVault.sol


Summary
 - [unchecked-lowlevel](#unchecked-lowlevel) (1 results) (Medium)
 - [uninitialized-local](#uninitialized-local) (1 results) (Medium)
 - [missing-zero-check](#missing-zero-check) (2 results) (Low)
 - [reentrancy-events](#reentrancy-events) (2 results) (Low)
 - [solc-version](#solc-version) (2 results) (Informational)
 - [low-level-calls](#low-level-calls) (2 results) (Informational)
 - [naming-convention](#naming-convention) (2 results) (Informational)
 - [unused-state](#unused-state) (1 results) (Informational)
 - [immutable-states](#immutable-states) (2 results) (Optimization)
## unchecked-lowlevel
Impact: Medium
Confidence: Medium
 - [ ] ID-0
[VulnerableVault.claimRewards()](../contracts/VulnerableVault.sol#L90-L105) ignores return value by [powerseller_nft.call(abi.encodeWithSignature(checkPrivilege(address),msg.sender))](../contracts/VulnerableVault.sol#L93-L98)

../contracts/VulnerableVault.sol#L90-L105


## uninitialized-local
Impact: Medium
Confidence: Medium
 - [ ] ID-1
[VulnerableVault.claimRewards().amount](../contracts/VulnerableVault.sol#L91) is a local variable never initialized

../contracts/VulnerableVault.sol#L91


## missing-zero-check
Impact: Low
Confidence: Medium
 - [ ] ID-2
[VulnerableVault.constructor(address,address).shop](../contracts/VulnerableVault.sol#L57) lacks a zero-check on :
		- [shop_addr = shop](../contracts/VulnerableVault.sol#L59)

../contracts/VulnerableVault.sol#L57


 - [ ] ID-3
[VulnerableVault.constructor(address,address).token](../contracts/VulnerableVault.sol#L57) lacks a zero-check on :
		- [powerseller_nft = token](../contracts/VulnerableVault.sol#L58)

../contracts/VulnerableVault.sol#L57


## reentrancy-events
Impact: Low
Confidence: Medium
 - [ ] ID-4
Reentrancy in [VulnerableVault.doUnstake(uint256)](../contracts/VulnerableVault.sol#L74-L83):
	External calls:
	- [(success) = address(msg.sender).call{value: amount}()](../contracts/VulnerableVault.sol#L79)
	Event emitted after the call(s):
	- [Unstake(msg.sender,amount)](../contracts/VulnerableVault.sol#L82)

../contracts/VulnerableVault.sol#L74-L83


 - [ ] ID-5
Reentrancy in [VulnerableVault.claimRewards()](../contracts/VulnerableVault.sol#L90-L105):
	External calls:
	- [powerseller_nft.call(abi.encodeWithSignature(checkPrivilege(address),msg.sender))](../contracts/VulnerableVault.sol#L93-L98)
	Event emitted after the call(s):
	- [Rewards(msg.sender,amount)](../contracts/VulnerableVault.sol#L104)

../contracts/VulnerableVault.sol#L90-L105


## solc-version
Impact: Informational
Confidence: High
 - [ ] ID-6
solc-0.8.20 is not recommended for deployment

 - [ ] ID-7
Pragma version[^0.8.20](../contracts/VulnerableVault.sol#L2) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../contracts/VulnerableVault.sol#L2


## low-level-calls
Impact: Informational
Confidence: High
 - [ ] ID-8
Low level call in [VulnerableVault.doUnstake(uint256)](../contracts/VulnerableVault.sol#L74-L83):
	- [(success) = address(msg.sender).call{value: amount}()](../contracts/VulnerableVault.sol#L79)

../contracts/VulnerableVault.sol#L74-L83


 - [ ] ID-9
Low level call in [VulnerableVault.claimRewards()](../contracts/VulnerableVault.sol#L90-L105):
	- [powerseller_nft.call(abi.encodeWithSignature(checkPrivilege(address),msg.sender))](../contracts/VulnerableVault.sol#L93-L98)

../contracts/VulnerableVault.sol#L90-L105


## naming-convention
Impact: Informational
Confidence: High
 - [ ] ID-10
Variable [VulnerableVault.powerseller_nft](../contracts/VulnerableVault.sol#L19) is not in mixedCase

../contracts/VulnerableVault.sol#L19


 - [ ] ID-11
Variable [VulnerableVault.shop_addr](../contracts/VulnerableVault.sol#L21) is not in mixedCase

../contracts/VulnerableVault.sol#L21


## unused-state
Impact: Informational
Confidence: High
 - [ ] ID-12
[VulnerableVault.lockedFunds](../contracts/VulnerableVault.sol#L17) is never used in [VulnerableVault](../contracts/VulnerableVault.sol#L12-L121)

../contracts/VulnerableVault.sol#L17


## immutable-states
Impact: Optimization
Confidence: High
 - [ ] ID-13
[VulnerableVault.powerseller_nft](../contracts/VulnerableVault.sol#L19) should be immutable 

../contracts/VulnerableVault.sol#L19


 - [ ] ID-14
[VulnerableVault.shop_addr](../contracts/VulnerableVault.sol#L21) should be immutable 

../contracts/VulnerableVault.sol#L21


