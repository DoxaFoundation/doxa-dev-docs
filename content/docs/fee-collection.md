---
title: "Fee Collection"
linkTitle: "Fee Collection"
weight: 5
description: "Guide for fee structure and collection in the Doxa Protocol"
---

This guide explains the fee structure and collection process in the Doxa Protocol.

## Overview

Doxa Protocol implements various fees to maintain system stability and incentivize participants. All fees are collected in USDx and distributed among stakeholders.

## Fee Structure

### Transaction Fees
```motoko
// Base transaction fee
public let BASE_FEE : Nat = 10_000; // 0.0001 USDx

// Pool creation fee
public let POOL_CREATION_FEE : Nat = 100_000_000; // 1 USDx

// Approval fee
public let APPROVAL_FEE : Nat = 10_000; // 0.0001 USDx
```

### Protocol Fees

| Operation | Fee Amount | Description |
|-----------|------------|-------------|
| Pool Creation | 1 USDx | One-time fee for creating a new pool |
| Swap | 0.3% | Fee on each swap transaction |
| Mint | 0.1% | Fee for minting new USDx |
| Burn | 0% | No fee for burning USDx |
| Liquidation | 13% | Penalty fee on liquidated positions |

## Fee Collection Mechanism

### Swap Fees
```motoko
public shared func collectSwapFee(
    poolId: Principal,
    amount: Nat
) : async Result<(), Text> {
    // Collection logic
}
```

### Distribution Model
```motoko
type FeeDistribution = {
    treasury: Nat = 50; // 50% to treasury
    stakers: Nat = 30;  // 30% to stakers
    lps: Nat = 20;      // 20% to liquidity providers
};
```

## Fee Management

### Treasury Management
```motoko
public shared ({ caller }) func withdrawTreasuryFees(
    amount: Nat
) : async Result<(), Text>
```

### Staker Rewards
```motoko
public func calculateStakerRewards(
    stakeId: StakeId
) : async Nat
```

### LP Fee Claims
```motoko
public shared ({ caller }) func claimLPFees(
    poolId: Principal
) : async Result<Nat, Text>
```

## Weekly Distribution

### Schedule
- Weekly fee distribution every Monday at 00:00 UTC
- Automatic distribution to eligible participants
- Manual claim required for LP fees

### Distribution Flow
1. Fees collected during the week
2. Distribution calculation on Sunday
3. Treasury allocation
4. Staker rewards calculation
5. LP share computation
6. Distribution execution

## Monitoring & Reporting

### Fee Statistics
```motoko
public query func getFeeStats() : async {
    totalCollected: Nat;
    weeklyVolume: Nat;
    distributionPending: Nat;
}
```

### Historical Data
```motoko
public query func getFeeHistory(
    startTime: Time,
    endTime: Time
) : async [FeeRecord]
```

## Best Practices

### For Users
1. **Transaction Planning**
   - Batch transactions to minimize fees
   - Consider fee tiers for different operations
   - Monitor gas prices for optimal timing

2. **Fee Optimization**
   - Use approved operators to save on approval fees
   - Participate in fee sharing programs
   - Stake tokens for fee discounts

### For Developers
1. **Integration Tips**
   - Always include fee calculations
   - Handle fee collection failures
   - Implement proper fee accounting

2. **Security Considerations**
   - Verify fee calculations
   - Implement fee limits
   - Monitor for unusual patterns 