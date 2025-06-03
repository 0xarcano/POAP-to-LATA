# POAP-to-LATA Automation Project
## Technical Design and Collaborative Development Plan

---

## 📋 Executive Summary

This project implements an automated system for claiming LATA governance tokens based on verification of specific POAP ownership. The solution uses custom Aragon OSX plugins to manage the list of eligible POAPs and automate the token claiming process.

---

## 🏗️ System Architecture

### Main Components

```
📦 EnceboDAO Automation System
├── 🏛️ Aragon OSX DAO (Base)
├── 🪙 LATA Token Contract (ERC-20Votes)
├── 🔌 POAP Manager Plugin
├── 🔌 POAP Claim Plugin
├── 🌐 Frontend Interface
└── 📊 Monitoring & Analytics
```

### Technology Stack

- **Blockchain**: Arbitrum (Mainnet & Sepolia for testing)
- **DAO Framework**: Aragon OSX
- **Smart Contracts**: Solidity ^0.8.19
- **Frontend**: React + TypeScript + Vite
- **Testing**: Hardhat + Chai + Mocha
- **Tools**: OpenZeppelin, Aragon SDK

---

## 🔧 Detailed Technical Specifications

### 1. POAP Manager Plugin

**Purpose**: Manage token claiming system configuration.

**Main Functionalities**:
```solidity
interface IPOAPManager {
    function addEligiblePOAP(uint256 poapId) external;
    function removeEligiblePOAP(uint256 poapId) external;
    function setRequiredThreshold(uint256 threshold) external;
    function setTokensPerPOAP(uint256 amount) external;
    function getEligiblePOAPs() external view returns (uint256[] memory);
    function getRequiredThreshold() external view returns (uint256);
}
```

**Technical Features**:
- Inherits from Aragon OSX `PluginUUPSUpgradeable`
- Access control based on Aragon permissions
- Events for all configuration modifications
- Robust input validations

### 2. POAP Claim Plugin

**Purpose**: Automate LATA token claiming based on POAPs.

**Main Functionalities**:
```solidity
interface IPOAPClaim {
    function claimTokens() external;
    function batchClaimTokens(address[] calldata users) external;
    function getClaimableAmount(address user) external view returns (uint256);
    function hasClaimedPOAP(address user, uint256 poapId) external view returns (bool);
}
```

**Technical Features**:
- Automatic POAP ownership verification
- Prevention of double claiming per POAP
- Gas optimization for batch claims
- Detailed event system for traceability

### 3. LATA Token Contract

**Specifications**:
```solidity
contract LATAToken is ERC20Votes, AccessControl {
    bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");
    
    function mint(address to, uint256 amount) external onlyRole(MINTER_ROLE);
    function burn(address from, uint256 amount) external onlyRole(MINTER_ROLE);
}
```

**Features**:
- Implements ERC-20Votes for Aragon compatibility
- Role system for issuance control
- Controlled mint/burn functions
- Native integration with delegation system

---
