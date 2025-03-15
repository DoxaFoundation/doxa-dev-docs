---
title: "Users vs Developers"
linkTitle: "Users vs Developers"
weight: 2
description: "Comparison of capabilities between users and developers in the Doxa Protocol"
---

# Users vs Developers in Doxa Protocol

## System Access & Control

### For Developers
```motoko
// Full system access with canister control
actor class DoxaCanister {
    // Ability to modify core protocol logic
    // Access to system configurations
    // Custom implementation capabilities
}
```

**Capabilities:**
- Full canister access and modification rights
- Custom pool creation and management
- System parameter configuration
- Protocol-level customizations

### For Users
```typescript
// Interface-level access only
const userActions = {
    swap: (tokenA, tokenB, amount) => {},
    stake: (amount, duration) => {},
    addLiquidity: (pool, amountA, amountB) => {}
}
```

**Capabilities:**
- Web interface interactions
- Predefined trading operations
- Standard portfolio management
- Basic liquidity operations

## Setup & Requirements

### Developer Setup
```bash
# Required tools and environment
npm install           # Package management
dfx start            # IC development environment
dfx deploy           # Canister deployment
```

**Requirements:**
- Full development environment
- Node.js and npm
- Internet Computer SDK (dfx)
- Development tools and IDEs

### User Setup
**Simple Requirements:**
- Web browser
- Internet Computer wallet
- Internet connection

## Risk Management

### Developer Controls
```motoko
public func manageSystemRisk() {
    // Implementation of safety measures
    // Custom risk parameters
    // Emergency controls
}
```

**Responsibilities:**
- System-level risk management
- Safety parameter configuration
- Emergency shutdown capabilities
- Protocol upgrade management

### User Protections
```typescript
// Built-in safety mechanisms
const safetyLimits = {
    maxTransactionSize: 1000000,
    minStakeAmount: 100,
    maxDailyVolume: 500000
}
```

**Safeguards:**
- Predefined transaction limits
- Automated risk checks
- Built-in slippage protection
- Clear warning systems

## Interface Access

### Developer Tools
- ✓ Direct canister interaction
- ✓ Backend code modification
- ✓ System configuration access
- ✓ Test environment usage
- ✓ Deployment controls

### User Interface
- ✓ Web interface access
- ✓ Portfolio management
- ✓ Trading operations
- ✓ Liquidity provision
- ✓ Reward tracking

## Getting Started

### For Developers
Visit our [Developer Documentation](/docs/developer-guide) for:
- Development environment setup
- API documentation
- Testing guidelines
- Deployment procedures

### For Users
Check our [User Guide](/docs/user-guide) for:
- Account setup
- Trading instructions
- Portfolio management
- Liquidity provision guide

Need help? Visit our [GitHub repository](https://github.com/DoxaFoundation/doxa-v3) or join our community channels. 