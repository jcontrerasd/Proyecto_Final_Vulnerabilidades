# SMART CONTRACT VulnerableBank.sol


Summary
 - [reentrancy-eth](#reentrancy-eth) (1 results) (High)
 - [events-maths](#events-maths) (1 results) (Low)
 - [reentrancy-benign](#reentrancy-benign) (1 results) (Low)
 - [reentrancy-events](#reentrancy-events) (1 results) (Low)
 - [solc-version](#solc-version) (2 results) (Informational)
 - [naming-convention](#naming-convention) (4 results) (Informational)
 - [immutable-states](#immutable-states) (1 results) (Optimization)
## reentrancy-eth
Impact: High
Confidence: Medium
 - [ ] ID-0
Reentrancy in [VulnerableBank.distributeBenefits(uint256)](../contracts/VulnerableBank.sol#L93-L113):
	External calls:
	- [returnRewards(percentage)](../contracts/VulnerableBank.sol#L95)
		- [(success) = address(msg.sender).call{value: reward}()](../contracts/VulnerableBank.sol#L49)
	State variables written after the call(s):
	- [total_invested -= amount](../contracts/VulnerableBank.sol#L107)
	[VulnerableBank.total_invested](../contracts/VulnerableBank.sol#L20) can be used in cross function reentrancies:
	- [VulnerableBank.distributeBenefits(uint256)](../contracts/VulnerableBank.sol#L93-L113)
	- [VulnerableBank.returnRewards(uint256)](../contracts/VulnerableBank.sol#L45-L53)

../contracts/VulnerableBank.sol#L93-L113


## events-maths
Impact: Low
Confidence: Medium
 - [ ] ID-1
[VulnerableBank.updateConfig(uint256)](../contracts/VulnerableBank.sol#L75-L77) should emit an event for: 
	- [distribute_period = n_blocks](../contracts/VulnerableBank.sol#L76) 

../contracts/VulnerableBank.sol#L75-L77


## reentrancy-benign
Impact: Low
Confidence: Medium
 - [ ] ID-2
Reentrancy in [VulnerableBank.distributeBenefits(uint256)](../contracts/VulnerableBank.sol#L93-L113):
	External calls:
	- [returnRewards(percentage)](../contracts/VulnerableBank.sol#L95)
		- [(success) = address(msg.sender).call{value: reward}()](../contracts/VulnerableBank.sol#L49)
	State variables written after the call(s):
	- [latest_distribution = block.number](../contracts/VulnerableBank.sol#L103)

../contracts/VulnerableBank.sol#L93-L113


## reentrancy-events
Impact: Low
Confidence: Medium
 - [ ] ID-3
Reentrancy in [VulnerableBank.distributeBenefits(uint256)](../contracts/VulnerableBank.sol#L93-L113):
	External calls:
	- [returnRewards(percentage)](../contracts/VulnerableBank.sol#L95)
		- [(success) = address(msg.sender).call{value: reward}()](../contracts/VulnerableBank.sol#L49)
	Event emitted after the call(s):
	- [Benefits(amount)](../contracts/VulnerableBank.sol#L112)

../contracts/VulnerableBank.sol#L93-L113


## solc-version
Impact: Informational
Confidence: High
 - [ ] ID-4
solc-0.8.20 is not recommended for deployment

 - [ ] ID-5
Pragma version[^0.8.13](../contracts/VulnerableBank.sol#L2) allows old versions

../contracts/VulnerableBank.sol#L2


## naming-convention
Impact: Informational
Confidence: High
 - [ ] ID-6
Variable [VulnerableBank.distribute_period](../contracts/VulnerableBank.sol#L26) is not in mixedCase

../contracts/VulnerableBank.sol#L26


 - [ ] ID-7
Variable [VulnerableBank.total_invested](../contracts/VulnerableBank.sol#L20) is not in mixedCase

../contracts/VulnerableBank.sol#L20


 - [ ] ID-8
Variable [VulnerableBank.latest_distribution](../contracts/VulnerableBank.sol#L28) is not in mixedCase

../contracts/VulnerableBank.sol#L28


 - [ ] ID-9
Parameter [VulnerableBank.updateConfig(uint256).n_blocks](../contracts/VulnerableBank.sol#L75) is not in mixedCase

../contracts/VulnerableBank.sol#L75


## immutable-states
Impact: Optimization
Confidence: High
 - [ ] ID-10
[VulnerableBank.admin](../contracts/VulnerableBank.sol#L22) should be immutable 

../contracts/VulnerableBank.sol#L22


