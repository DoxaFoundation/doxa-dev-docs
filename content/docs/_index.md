---
title: "Documentation"
linkTitle: "Docs"
weight: 20
description: "Welcome to the Doxa Protocol Documentation"
---

# Welcome to Doxa Protocol Documentation

The Doxa Protocol is a stablecoin system built on the Internet Computer that provides DUSD (Doxa USD), a 1:1 ckUSDC-backed stablecoin with integrated staking and trading capabilities.

## Core Protocol Components

### 🪙 [DUSD Token Integration](dusd-token-integration/)
Learn about DUSD, our ICRC-1 compliant stablecoin backed 1:1 by ckUSDC with 6 decimal precision.

### 🏗️ [Key Components](key-components/)
Understand the core architecture including the minter system, staking mechanism, and AMM trading.

### 🏛️ [Architecture Overview](architecture-overview/)
Deep dive into the technical architecture and inter-canister communication patterns.

### 💰 [Staking Integration](staking-integration/)
Explore the 20% APY staking system with 30-day lock periods and bootstrap rewards.

### 🔄 [Pool Operations](pool-operations/)
Learn about AMM-based token swaps and liquidity provision.

### 💸 [Fee Collection](fee-collection/)
Understanding the fee structure and distribution mechanisms.

## Getting Started

### For Developers
1. **[Integration Guide](integration-guide/)** - Complete guide for integrating with Doxa Protocol
2. **[Users vs Developers Guide](user-developer-guide/)** - Understanding different access levels and capabilities

### Monitoring & Analytics
- **[Transaction Monitoring](transaction-monitoring/)** - Track DUSD transactions and activities
- **[Statistics & Data](statistics/)** - Access protocol statistics and user analytics

### Support & Security
- **[Security Guide](security-guide/)** - Best practices and security implementation
- **[Troubleshooting](troubleshooting/)** - Common issues and solutions

## Quick Reference

### Core Canister IDs
- **DUSD Token**: `irorr-5aaaa-aaaak-qddsq-cai`
- **Staking**: `mhahe-xqaaa-aaaag-qndha-cai`
- **ckUSDC**: `xevnm-gaaaa-aaaar-qafnq-cai`

### Key Parameters
- **Minting Ratio**: 1:1 (1 ckUSDC = 1 DUSD)
- **Minimum Deposit**: 1 ckUSDC
- **Staking APY**: 20% base rate
- **Lock Period**: 30 days minimum
- **Minimum Stake**: 10 DUSD

## Additional Resources

- **[Internet Computer Documentation](https://internetcomputer.org/docs/current/home)**
- **[Motoko Programming Language Book](https://internetcomputer.org/docs/current/motoko/main/motoko)**
- **[ICRC-1 Token Standard](https://internetcomputer.org/docs/current/developer-docs/integrations/icrc-1/)** 