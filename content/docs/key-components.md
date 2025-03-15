---
title: "Key Components"
linkTitle: "Key Components"
weight: 1
description: "Overview of core components in the Doxa Protocol"
---

The Doxa Protocol consists of several fundamental building blocks that work together to create a robust stablecoin system. This section provides a detailed explanation of these core components and their interactions.

## Core System Architecture

```motoko
// Core System Components
module DoxaStablecoin {
  // Canister Configuration
  public let DOXA_CANISTER_ID : Text = "xeka7-7iaaa-...";
  public let CYCLE_FEE : Nat = 10_000; // 0.00001 XDR
  
  // Stablecoin Architecture
  public type StablecoinConfig = {
    // Tokenomics Parameters
    collateralRatio: Nat = 150; // 150% collateralization
    minimumMint: Nat = 100; // Minimum mint amount
    liquidationThreshold: Nat = 120; // 120% threshold
    
    // Stability Mechanism
    stabilityFee: Nat = 1_000; // 0.1% fee
    liquidationPenalty: Nat = 13_000; // 13% penalty
    
    // Price Oracle Integration
    oracleCanisterId: Text = "abc123...";
    updateFrequency: Nat = 300; // 5 minutes
  };

  // Multi-Collateral Support
  public type CollateralType = {
    #BTC;
    #ETH;
    #ICP;
    #Cycles;
  };

  // Vault Management
  public type Vault = {
    owner: Principal;
    collateralType: CollateralType;
    collateralAmount: Nat;
    debtAmount: Nat;
    lastUpdate: Time;
  };

  // Chain Fusion Integration
  public type ChainConfig = {
    btcAdapter: Text = "btc-adapter-canister-id";
    ethAdapter: Text = "eth-adapter-canister-id";
    cyclesMinting: Text = "cycles-minting-canister-id";
  };
};
```

## Key Features

### 1. Stablecoin Architecture
- **Multi-Collateral System**: Support for BTC, ETH, ICP and Cycles as collateral
- **Overcollateralization**: 150% minimum collateral ratio for stability
- **Dynamic Liquidation**: Automatic liquidation at 120% threshold
- **Price Oracle Integration**: Real-time price feeds with 5-minute updates

### 2. Stability Mechanism
- **Stability Fee**: 0.1% fee on minting operations
- **Liquidation Penalty**: 13% penalty on forced liquidations
- **Algorithmic Adjustments**: Dynamic parameters based on market conditions

### 3. Chain Fusion Features
- **Cross-Chain Integration**: Seamless interaction with Bitcoin and Ethereum
- **Cycles Management**: Efficient handling of computation costs
- **Canister Coordination**: Automated multi-canister orchestration

### 4. Security Features
- **Internet Identity Authentication**: Secure user access control
- **Chain Key Cryptography**: Leveraging IC's cryptographic capabilities
- **Threshold Signatures**: Secure cross-chain transaction validation

## Component Interactions

### Vault Management
The vault system is the core of Doxa's collateralized debt positions:
- Users can create vaults with different collateral types
- Each vault maintains its collateralization ratio
- Automatic liquidation triggers protect system stability

### Price Oracle System
Real-time price feeds ensure accurate collateral valuation:
- 5-minute update frequency
- Multi-source price aggregation
- Chainlink-style decentralized oracle network

### Chain Fusion Integration
Seamless interaction with multiple blockchains:
- Bitcoin integration via threshold ECDSA
- Ethereum bridge via Chain Key TX
- Native IC cycles management

## Technical Specifications

### Collateral Parameters
| Asset | Min Ratio | Liquidation Threshold | Stability Fee |
|-------|-----------|----------------------|---------------|
| BTC   | 150%      | 120%                | 0.1%         |
| ETH   | 150%      | 120%                | 0.1%         |
| ICP   | 175%      | 130%                | 0.1%         |
| Cycles| 200%      | 150%                | 0.1%         |

### System Constraints
- Minimum mint amount: 100 USDx
- Maximum vault size: Based on collateral type
- Price update frequency: 5 minutes
- Transaction finality: ~2 seconds 