---
title: "Key Components"
linkTitle: "Key Components"
weight: 1
description: "Overview of core components in the Doxa Protocol"
---

The Doxa Protocol consists of several fundamental building blocks that work together to create a robust stablecoin system. This section provides a detailed explanation of these core components and their interactions.

## Core System Components

The Doxa Protocol implements DUSD (Doxa USD), a stablecoin that maintains a 1:1 peg with USD through ckUSDC collateralization.

### 1. DUSD Token (irorr-5aaaa-aaaak-qddsq-cai)
- **Standard**: ICRC-1 compliant stablecoin
- **Decimals**: 6 (1 DUSD = 1,000,000 units)
- **Backing**: 1:1 collateralized by ckUSDC
- **Supply**: Backed by verified ckUSDC deposits

### 2. Stablecoin Minter System
- **Collateral**: ckUSDC (Circle USD on Internet Computer)
- **Minting Ratio**: 1:1 (1 ckUSDC = 1 DUSD)
- **Minimum Deposit**: 1 ckUSDC
- **Reserve Management**: Automated collateral verification

### 3. Staking System (mhahe-xqaaa-aaaag-qndha-cai)
- **Base APY**: 20%
- **Lock Duration**: 30 days minimum
- **Minimum Stake**: 10 DUSD
- **Reward Token**: DUSD
- **Bootstrap Phase**: Enhanced early staker rewards

### 4. AMM Trading System
- **Swap Functionality**: Automated market making
- **Price Discovery**: Dynamic pricing based on liquidity
- **Fee Structure**: 0.3% standard trading fees
- **Supported Pairs**: DUSD/ckUSDC and other IC tokens

## Key Features

### Minting Process
1. User deposits ckUSDC to reserve account
2. System validates transaction on ckUSDC ledger
3. Equivalent DUSD tokens are minted 1:1
4. DUSD transferred to user's account

### Staking Mechanism
- Dynamic staking pool with 20% base APY
- 30-day minimum lock period for rewards
- Bootstrap phase for enhanced early rewards
- Compound interest calculations

### Trading Features
- AMM-based token swaps
- Liquidity provision capabilities
- Real-time price calculations
- Multi-token support

## Technical Architecture

### Canister Network
- **DUSD Token**: Core stablecoin implementation
- **Minter**: Handles ckUSDC deposits and DUSD minting
- **Staking**: Manages stake positions and rewards
- **Swap**: Facilitates token trading

### Inter-Canister Communication
```motoko
// Example: Minting DUSD
let transferArg : Icrc.TransferArg = {
    amount = mintAmount;
    to = userAccount;
    // ... other parameters
};
let result = await DUSD.icrc1_transfer(transferArg);
```

### Security Features
- **Collateral Validation**: Every mint backed by verified ckUSDC
- **Reserve Accounts**: Secure custody of collateral
- **Transaction Verification**: On-chain validation of all deposits
- **Access Controls**: Role-based permissions

## Integration Points

### For Developers
- ICRC-1 standard compatibility
- Query functions for balances and metadata
- Transfer and approval mechanisms
- Staking position management

### For DApps
- Token integration via standard interfaces
- Liquidity pool interactions
- Staking reward calculations
- Real-time price feeds

## System Constraints

- **Minimum Operations**: 1 ckUSDC deposits, 10 DUSD stakes
- **Collateralization**: 100% backed (1:1 ratio)
- **Lock Periods**: 30-day minimum for staking rewards
- **Fee Structure**: 0.3% trading fees, no minting fees 