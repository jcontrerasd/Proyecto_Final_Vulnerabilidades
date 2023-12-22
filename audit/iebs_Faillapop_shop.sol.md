Summary
 - [reentrancy-eth](#reentrancy-eth) (2 results) (High)
 - [uninitialized-state](#uninitialized-state) (1 results) (High)
 - [reentrancy-no-eth](#reentrancy-no-eth) (1 results) (Medium)
 - [unchecked-lowlevel](#unchecked-lowlevel) (1 results) (Medium)
 - [events-maths](#events-maths) (1 results) (Low)
 - [missing-zero-check](#missing-zero-check) (4 results) (Low)
 - [reentrancy-benign](#reentrancy-benign) (4 results) (Low)
 - [reentrancy-events](#reentrancy-events) (6 results) (Low)
 - [timestamp](#timestamp) (1 results) (Low)
 - [dead-code](#dead-code) (1 results) (Informational)
 - [solc-version](#solc-version) (20 results) (Informational)
 - [low-level-calls](#low-level-calls) (3 results) (Informational)
 - [missing-inheritance](#missing-inheritance) (1 results) (Informational)
 - [naming-convention](#naming-convention) (14 results) (Informational)
 - [similar-names](#similar-names) (1 results) (Informational)
 - [too-many-digits](#too-many-digits) (1 results) (Informational)
 - [unused-state](#unused-state) (1 results) (Informational)
 - [constable-states](#constable-states) (1 results) (Optimization)
 - [immutable-states](#immutable-states) (2 results) (Optimization)
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


 - [ ] ID-1
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


## uninitialized-state
Impact: High
Confidence: High
 - [ ] ID-2
[FP_DAO.quorum](../contracts/iebs_Faillapop_DAO.sol#L63) is never initialized. It is used in:
	- [FP_DAO.endDispute(uint256)](../contracts/iebs_Faillapop_DAO.sol#L196-L214)

../contracts/iebs_Faillapop_DAO.sol#L63


## reentrancy-no-eth
Impact: Medium
Confidence: Medium
 - [ ] ID-3
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


## unchecked-lowlevel
Impact: Medium
Confidence: Medium
 - [ ] ID-4
[FP_Vault.claimRewards()](../contracts/iebs_Faillapop_vault.sol#L184-L204) ignores return value by [nftContract.call(abi.encodeWithSignature(checkPrivilege(address),msg.sender))](../contracts/iebs_Faillapop_vault.sol#L186-L191)

../contracts/iebs_Faillapop_vault.sol#L184-L204


## events-maths
Impact: Low
Confidence: Medium
 - [ ] ID-5
[VulnerableBank.updateConfig(uint256)](../contracts/VulnerableBank.sol#L75-L77) should emit an event for: 
	- [distribute_period = n_blocks](../contracts/VulnerableBank.sol#L76) 

../contracts/VulnerableBank.sol#L75-L77


## missing-zero-check
Impact: Low
Confidence: Medium
 - [ ] ID-6
[FP_Vault.updateConfig(address,address,address).newNft](../contracts/iebs_Faillapop_vault.sol#L171) lacks a zero-check on :
		- [nftContract = newNft](../contracts/iebs_Faillapop_vault.sol#L174)

../contracts/iebs_Faillapop_vault.sol#L171


 - [ ] ID-7
[FP_Vault.constructor(address,address,address).token](../contracts/iebs_Faillapop_vault.sol#L93) lacks a zero-check on :
		- [nftContract = token](../contracts/iebs_Faillapop_vault.sol#L98)

../contracts/iebs_Faillapop_vault.sol#L93


 - [ ] ID-8
[FP_DAO.updateConfig(string,string,address,address).newShop](../contracts/iebs_Faillapop_DAO.sol#L129) lacks a zero-check on :
		- [shop_addr = newShop](../contracts/iebs_Faillapop_DAO.sol#L133)

../contracts/iebs_Faillapop_DAO.sol#L129


 - [ ] ID-9
[FP_DAO.constructor(string,address,address,address).shop](../contracts/iebs_Faillapop_DAO.sol#L111) lacks a zero-check on :
		- [shop_addr = shop](../contracts/iebs_Faillapop_DAO.sol#L113)

../contracts/iebs_Faillapop_DAO.sol#L111


## reentrancy-benign
Impact: Low
Confidence: Medium
 - [ ] ID-10
Reentrancy in [FP_Vault.distributeSlashing(uint256)](../contracts/iebs_Faillapop_vault.sol#L210-L218):
	External calls:
	- [(data) = nftContract.call(abi.encodeWithSignature(totalPowersellers()))](../contracts/iebs_Faillapop_vault.sol#L213)
	State variables written after the call(s):
	- [max_claimable_amount = newMax](../contracts/iebs_Faillapop_vault.sol#L217)

../contracts/iebs_Faillapop_vault.sol#L210-L218


 - [ ] ID-11
Reentrancy in [FP_DAO.endDispute(uint256)](../contracts/iebs_Faillapop_DAO.sol#L196-L214):
	External calls:
	- [sellerWins(itemId)](../contracts/iebs_Faillapop_DAO.sol#L207)
		- [shopContract.endDispute(itemId)](../contracts/iebs_Faillapop_DAO.sol#L282)
	State variables written after the call(s):
	- [disputeResult[disputeId] = 2](../contracts/iebs_Faillapop_DAO.sol#L208)

../contracts/iebs_Faillapop_DAO.sol#L196-L214


 - [ ] ID-12
Reentrancy in [FP_DAO.endDispute(uint256)](../contracts/iebs_Faillapop_DAO.sol#L196-L214):
	External calls:
	- [buyerWins(itemId)](../contracts/iebs_Faillapop_DAO.sol#L204)
		- [shopContract.returnItem(itemId)](../contracts/iebs_Faillapop_DAO.sol#L274)
	State variables written after the call(s):
	- [disputeResult[disputeId] = 1](../contracts/iebs_Faillapop_DAO.sol#L205)

../contracts/iebs_Faillapop_DAO.sol#L196-L214


 - [ ] ID-13
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
 - [ ] ID-14
Reentrancy in [FP_DAO.endDispute(uint256)](../contracts/iebs_Faillapop_DAO.sol#L196-L214):
	External calls:
	- [buyerWins(itemId)](../contracts/iebs_Faillapop_DAO.sol#L204)
		- [shopContract.returnItem(itemId)](../contracts/iebs_Faillapop_DAO.sol#L274)
	- [sellerWins(itemId)](../contracts/iebs_Faillapop_DAO.sol#L207)
		- [shopContract.endDispute(itemId)](../contracts/iebs_Faillapop_DAO.sol#L282)
	Event emitted after the call(s):
	- [EndDispute(disputeId,itemId)](../contracts/iebs_Faillapop_DAO.sol#L213)

../contracts/iebs_Faillapop_DAO.sol#L196-L214


 - [ ] ID-15
Reentrancy in [FP_Vault.doSlash(address)](../contracts/iebs_Faillapop_vault.sol#L154-L163):
	External calls:
	- [distributeSlashing(amount)](../contracts/iebs_Faillapop_vault.sol#L160)
		- [(data) = nftContract.call(abi.encodeWithSignature(totalPowersellers()))](../contracts/iebs_Faillapop_vault.sol#L213)
	Event emitted after the call(s):
	- [Slashed(badUser,amount)](../contracts/iebs_Faillapop_vault.sol#L162)

../contracts/iebs_Faillapop_vault.sol#L154-L163


 - [ ] ID-16
Reentrancy in [FP_Vault.doUnstake(uint256)](../contracts/iebs_Faillapop_vault.sol#L115-L124):
	External calls:
	- [(success) = address(msg.sender).call{value: amount}()](../contracts/iebs_Faillapop_vault.sol#L120)
	Event emitted after the call(s):
	- [Unstake(msg.sender,amount)](../contracts/iebs_Faillapop_vault.sol#L123)

../contracts/iebs_Faillapop_vault.sol#L115-L124


 - [ ] ID-17
Reentrancy in [VulnerableBank.distributeBenefits(uint256)](../contracts/VulnerableBank.sol#L93-L113):
	External calls:
	- [returnRewards(percentage)](../contracts/VulnerableBank.sol#L95)
		- [(success) = address(msg.sender).call{value: reward}()](../contracts/VulnerableBank.sol#L49)
	Event emitted after the call(s):
	- [Benefits(amount)](../contracts/VulnerableBank.sol#L112)

../contracts/VulnerableBank.sol#L93-L113


 - [ ] ID-18
Reentrancy in [FP_Vault.claimRewards()](../contracts/iebs_Faillapop_vault.sol#L184-L204):
	External calls:
	- [nftContract.call(abi.encodeWithSignature(checkPrivilege(address),msg.sender))](../contracts/iebs_Faillapop_vault.sol#L186-L191)
	- [(success) = address(msg.sender).call{value: amount}()](../contracts/iebs_Faillapop_vault.sol#L198)
	External calls sending eth:
	- [(success) = address(msg.sender).call{value: amount}()](../contracts/iebs_Faillapop_vault.sol#L198)
	Event emitted after the call(s):
	- [RewardsClaimed(msg.sender,amount)](../contracts/iebs_Faillapop_vault.sol#L203)

../contracts/iebs_Faillapop_vault.sol#L184-L204


 - [ ] ID-19
Reentrancy in [FP_DAO.lotteryNFT(address)](../contracts/iebs_Faillapop_DAO.sol#L251-L267):
	External calls:
	- [nftContract.mintCoolNFT(user)](../contracts/iebs_Faillapop_DAO.sol#L262)
	Event emitted after the call(s):
	- [AwardNFT(user)](../contracts/iebs_Faillapop_DAO.sol#L266)

../contracts/iebs_Faillapop_DAO.sol#L251-L267


## timestamp
Impact: Low
Confidence: Medium
 - [ ] ID-20
[FP_DAO.lotteryNFT(address)](../contracts/iebs_Faillapop_DAO.sol#L251-L267) uses timestamp for comparisons
	Dangerous comparisons:
	- [randomNumber < THRESHOLD](../contracts/iebs_Faillapop_DAO.sol#L261)

../contracts/iebs_Faillapop_DAO.sol#L251-L267


## dead-code
Impact: Informational
Confidence: Medium
 - [ ] ID-21
[FP_Token._beforeTokenTransfer(address,address,uint256)](../contracts/iebs_Faillapop_ERC20.sol#L36-L43) is never used and should be removed

../contracts/iebs_Faillapop_ERC20.sol#L36-L43


## solc-version
Impact: Informational
Confidence: High
 - [ ] ID-22
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/token/ERC20/IERC20.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/token/ERC20/IERC20.sol#L4


 - [ ] ID-23
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol#L4


 - [ ] ID-24
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/interfaces/draft-IERC6093.sol#L3) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/interfaces/draft-IERC6093.sol#L3


 - [ ] ID-25
Pragma version[^0.8.20](../contracts/iebs_Faillapop_vault.sol#L2) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../contracts/iebs_Faillapop_vault.sol#L2


 - [ ] ID-26
Pragma version[^0.8.13](../contracts/interfaces/IFP_DAO.sol#L3) allows old versions

../contracts/interfaces/IFP_DAO.sol#L3


 - [ ] ID-27
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/utils/Pausable.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/utils/Pausable.sol#L4


 - [ ] ID-28
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol#L4


 - [ ] ID-29
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/access/AccessControl.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/access/AccessControl.sol#L4


 - [ ] ID-30
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/access/IAccessControl.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/access/IAccessControl.sol#L4


 - [ ] ID-31
solc-0.8.20 is not recommended for deployment

 - [ ] ID-32
Pragma version[^0.8.13](../contracts/interfaces/IFP_Shop.sol#L3) allows old versions

../contracts/interfaces/IFP_Shop.sol#L3


 - [ ] ID-33
Pragma version[^0.8.13](../contracts/VulnerableBank.sol#L2) allows old versions

../contracts/VulnerableBank.sol#L2


 - [ ] ID-34
Pragma version[^0.8.13](../contracts/interfaces/IFP_Vault.sol#L3) allows old versions

../contracts/interfaces/IFP_Vault.sol#L3


 - [ ] ID-35
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol#L4


 - [ ] ID-36
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/utils/introspection/IERC165.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/utils/introspection/IERC165.sol#L4


 - [ ] ID-37
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/utils/Context.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/utils/Context.sol#L4


 - [ ] ID-38
Pragma version[^0.8.20](../contracts/iebs_Faillapop_ERC20.sol#L2) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../contracts/iebs_Faillapop_ERC20.sol#L2


 - [ ] ID-39
Pragma version[^0.8.20](../contracts/iebs_Faillapop_DAO.sol#L2) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../contracts/iebs_Faillapop_DAO.sol#L2


 - [ ] ID-40
Pragma version[^0.8.20](../node_modules/@openzeppelin/contracts/utils/introspection/ERC165.sol#L4) necessitates a version too recent to be trusted. Consider deploying with 0.8.18.

../node_modules/@openzeppelin/contracts/utils/introspection/ERC165.sol#L4


 - [ ] ID-41
Pragma version[^0.8.13](../contracts/interfaces/IFP_NFT.sol#L3) allows old versions

../contracts/interfaces/IFP_NFT.sol#L3


## low-level-calls
Impact: Informational
Confidence: High
 - [ ] ID-42
Low level call in [FP_Vault.claimRewards()](../contracts/iebs_Faillapop_vault.sol#L184-L204):
	- [nftContract.call(abi.encodeWithSignature(checkPrivilege(address),msg.sender))](../contracts/iebs_Faillapop_vault.sol#L186-L191)
	- [(success) = address(msg.sender).call{value: amount}()](../contracts/iebs_Faillapop_vault.sol#L198)

../contracts/iebs_Faillapop_vault.sol#L184-L204


 - [ ] ID-43
Low level call in [FP_Vault.doUnstake(uint256)](../contracts/iebs_Faillapop_vault.sol#L115-L124):
	- [(success) = address(msg.sender).call{value: amount}()](../contracts/iebs_Faillapop_vault.sol#L120)

../contracts/iebs_Faillapop_vault.sol#L115-L124


 - [ ] ID-44
Low level call in [FP_Vault.distributeSlashing(uint256)](../contracts/iebs_Faillapop_vault.sol#L210-L218):
	- [(data) = nftContract.call(abi.encodeWithSignature(totalPowersellers()))](../contracts/iebs_Faillapop_vault.sol#L213)

../contracts/iebs_Faillapop_vault.sol#L210-L218


## missing-inheritance
Impact: Informational
Confidence: High
 - [ ] ID-45
[FP_Vault](../contracts/iebs_Faillapop_vault.sol#L18-L244) should inherit from [IFP_Vault](../contracts/interfaces/IFP_Vault.sol#L14-L56)

../contracts/iebs_Faillapop_vault.sol#L18-L244


## naming-convention
Impact: Informational
Confidence: High
 - [ ] ID-46
Function [FP_DAO.query_dispute(uint256)](../contracts/iebs_Faillapop_DAO.sol#L298-L300) is not in mixedCase

../contracts/iebs_Faillapop_DAO.sol#L298-L300


 - [ ] ID-47
Variable [FP_Vault.max_claimable_amount](../contracts/iebs_Faillapop_vault.sol#L43) is not in mixedCase

../contracts/iebs_Faillapop_vault.sol#L43


 - [ ] ID-48
Contract [IFP_Shop](../contracts/interfaces/IFP_Shop.sol#L14-L90) is not in CapWords

../contracts/interfaces/IFP_Shop.sol#L14-L90


 - [ ] ID-49
Contract [IFP_NFT](../contracts/interfaces/IFP_NFT.sol#L5-L15) is not in CapWords

../contracts/interfaces/IFP_NFT.sol#L5-L15


 - [ ] ID-50
Contract [IFP_DAO](../contracts/interfaces/IFP_DAO.sol#L12-L63) is not in CapWords

../contracts/interfaces/IFP_DAO.sol#L12-L63


 - [ ] ID-51
Variable [FP_DAO.shop_addr](../contracts/iebs_Faillapop_DAO.sol#L55) is not in mixedCase

../contracts/iebs_Faillapop_DAO.sol#L55


 - [ ] ID-52
Variable [VulnerableBank.distribute_period](../contracts/VulnerableBank.sol#L26) is not in mixedCase

../contracts/VulnerableBank.sol#L26


 - [ ] ID-53
Contract [IFP_Vault](../contracts/interfaces/IFP_Vault.sol#L14-L56) is not in CapWords

../contracts/interfaces/IFP_Vault.sol#L14-L56


 - [ ] ID-54
Contract [FP_Vault](../contracts/iebs_Faillapop_vault.sol#L18-L244) is not in CapWords

../contracts/iebs_Faillapop_vault.sol#L18-L244


 - [ ] ID-55
Contract [FP_Token](../contracts/iebs_Faillapop_ERC20.sol#L13-L45) is not in CapWords

../contracts/iebs_Faillapop_ERC20.sol#L13-L45


 - [ ] ID-56
Variable [VulnerableBank.total_invested](../contracts/VulnerableBank.sol#L20) is not in mixedCase

../contracts/VulnerableBank.sol#L20


 - [ ] ID-57
Contract [FP_DAO](../contracts/iebs_Faillapop_DAO.sol#L17-L304) is not in CapWords

../contracts/iebs_Faillapop_DAO.sol#L17-L304


 - [ ] ID-58
Variable [VulnerableBank.latest_distribution](../contracts/VulnerableBank.sol#L28) is not in mixedCase

../contracts/VulnerableBank.sol#L28


 - [ ] ID-59
Parameter [VulnerableBank.updateConfig(uint256).n_blocks](../contracts/VulnerableBank.sol#L75) is not in mixedCase

../contracts/VulnerableBank.sol#L75


## similar-names
Impact: Informational
Confidence: Medium
 - [ ] ID-60
Variable [FP_DAO.fptContract](../contracts/iebs_Faillapop_DAO.sol#L61) is too similar to [FP_DAO.nftContract](../contracts/iebs_Faillapop_DAO.sol#L59)

../contracts/iebs_Faillapop_DAO.sol#L61


## too-many-digits
Impact: Informational
Confidence: Medium
 - [ ] ID-61
[FP_Token.constructor()](../contracts/iebs_Faillapop_ERC20.sol#L17-L22) uses literals with too many digits:
	- [_mint(msg.sender,1000000 * 10 ** decimals())](../contracts/iebs_Faillapop_ERC20.sol#L20)

../contracts/iebs_Faillapop_ERC20.sol#L17-L22


## unused-state
Impact: Informational
Confidence: High
 - [ ] ID-62
[FP_DAO.DEFAULT_QUORUM](../contracts/iebs_Faillapop_DAO.sol#L23) is never used in [FP_DAO](../contracts/iebs_Faillapop_DAO.sol#L17-L304)

../contracts/iebs_Faillapop_DAO.sol#L23


## constable-states
Impact: Optimization
Confidence: High
 - [ ] ID-63
[FP_DAO.quorum](../contracts/iebs_Faillapop_DAO.sol#L63) should be constant 

../contracts/iebs_Faillapop_DAO.sol#L63


## immutable-states
Impact: Optimization
Confidence: High
 - [ ] ID-64
[FP_DAO.fptContract](../contracts/iebs_Faillapop_DAO.sol#L61) should be immutable 

../contracts/iebs_Faillapop_DAO.sol#L61


 - [ ] ID-65
[VulnerableBank.admin](../contracts/VulnerableBank.sol#L22) should be immutable 

../contracts/VulnerableBank.sol#L22


