# SMART CONTRACT VulnerableDAO.sol


Summary
 - [uninitialized-state](#uninitialized-state) (1 results) (High)
 - [timestamp](#timestamp) (1 results) (Low)
 - [solc-version](#solc-version) (2 results) (Informational)
 - [naming-convention](#naming-convention) (1 results) (Informational)
## uninitialized-state
Impact: High
Confidence: High
 - [ ] ID-0
[VulnerableDAO.disputes](../contracts/VulnerableDAO.sol#L32) is never initialized. It is used in:
	- [VulnerableDAO.query_dispute(uint256)](../contracts/VulnerableDAO.sol#L168-L170)

../contracts/VulnerableDAO.sol#L32


## timestamp
Impact: Low
Confidence: Medium
 - [ ] ID-1
[VulnerableDAO.lotteryNFT(address)](../contracts/VulnerableDAO.sol#L141-L159) uses timestamp for comparisons
	Dangerous comparisons:
	- [randomNumber < THRESHOLD](../contracts/VulnerableDAO.sol#L151)

../contracts/VulnerableDAO.sol#L141-L159


## solc-version
Impact: Informational
Confidence: High
 - [ ] ID-2
solc-0.8.20 is not recommended for deployment

 - [ ] ID-3
Pragma version[^0.8.13](../contracts/VulnerableDAO.sol#L2) allows old versions

../contracts/VulnerableDAO.sol#L2


## naming-convention
Impact: Informational
Confidence: High
 - [ ] ID-4
Function [VulnerableDAO.query_dispute(uint256)](../contracts/VulnerableDAO.sol#L168-L170) is not in mixedCase

../contracts/VulnerableDAO.sol#L168-L170


