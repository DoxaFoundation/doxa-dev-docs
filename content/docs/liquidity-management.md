---
title: "Liquidity Management"
linkTitle: "Liquidity Management"
weight: 2
description: "Guide for managing liquidity positions in the Doxa Protocol"
---

This guide explains how to manage liquidity positions in the Doxa Protocol.

## Overview

Doxa Protocol uses a concentrated liquidity model that allows liquidity providers (LPs) to specify custom price ranges for their positions.

## Liquidity Concepts

### Price Ranges
```motoko
type PriceRange = {
    tickLower: Int;
    tickUpper: Int;
    currentTick: Int;
    price0: Float;
    price1: Float;
};
```

### Position Types
```motoko
type Position = {
    id: Nat;
    owner: Principal;
    poolId: Principal;
    token0: Principal;
    token1: Principal;
    liquidity: Nat;
    range: PriceRange;
    feesEarned: {
        token0: Nat;
        token1: Nat;
    };
};
```

## Managing Positions

### Create Position
```motoko
public shared func createPosition(
    params: {
        poolId: Principal;
        tickLower: Int;
        tickUpper: Int;
        amount0Desired: Nat;
        amount1Desired: Nat;
        amount0Min: Nat;
        amount1Min: Nat;
        recipient: Principal;
        deadline: Int;
    }
) : async Result<PositionId, Text>
```

### Increase Liquidity
```motoko
public shared func increaseLiquidity(
    params: {
        positionId: Nat;
        amount0Desired: Nat;
        amount1Desired: Nat;
        amount0Min: Nat;
        amount1Min: Nat;
        deadline: Int;
    }
) : async Result<{
    liquidity: Nat;
    amount0: Nat;
    amount1: Nat;
}, Text>
```

### Decrease Liquidity
```motoko
public shared func decreaseLiquidity(
    params: {
        positionId: Nat;
        liquidity: Nat;
        amount0Min: Nat;
        amount1Min: Nat;
        deadline: Int;
    }
) : async Result<{
    amount0: Nat;
    amount1: Nat;
}, Text>
```

## Fee Management

### Collect Fees
```motoko
public shared func collectFees(
    positionId: Nat
) : async Result<{
    token0: Nat;
    token1: Nat;
}, Text>
```

### Fee Growth Tracking
```motoko
type FeeGrowth = {
    feeGrowthInside0LastX128: Nat;
    feeGrowthInside1LastX128: Nat;
    tokensOwed0: Nat;
    tokensOwed1: Nat;
};
```

## Position Analytics

### Get Position Details
```motoko
public query func getPosition(
    positionId: Nat
) : async ?PositionDetails

type PositionDetails = {
    position: Position;
    valueUSD: Float;
    apr: Float;
    healthFactor: Float;
};
```

### Calculate Returns
```motoko
public query func calculateReturns(
    positionId: Nat
) : async {
    feesEarned: Float;
    impermanentLoss: Float;
    netReturn: Float;
}
```

## Risk Management

### Health Monitoring
```motoko
public query func getPositionHealth(
    positionId: Nat
) : async {
    healthFactor: Float;
    liquidationPrice: Float;
    warningLevel: HealthWarning;
}
```

### Auto-Rebalancing
```motoko
public shared func setAutoRebalance(
    params: {
        positionId: Nat;
        enabled: Bool;
        targetRange: {lower: Float; upper: Float};
    }
) : async Result<(), Text>
```

## Best Practices

### 1. Position Sizing
- Consider market volatility
- Maintain balanced exposure
- Set appropriate ranges

### 2. Risk Management
- Monitor position health
- Set stop-loss levels
- Diversify across pools

### 3. Fee Optimization
- Choose optimal fee tiers
- Regular fee collection
- Track earnings

### 4. Range Management
- Analyze price trends
- Adjust ranges proactively
- Consider rebalancing costs

## Common Strategies

### 1. Passive LP
- Wide price ranges
- Lower maintenance
- Stable fee income

### 2. Active Management
- Narrow ranges
- Higher potential returns
- More frequent rebalancing

### 3. Hedged Positions
- Multiple positions
- Risk mitigation
- Balanced exposure

## Integration Examples

### Basic Position Creation
```motoko
// Create a new position
let result = await liquidityManager.createPosition({
    poolId = poolId;
    tickLower = calculateTick(lowerPrice);
    tickUpper = calculateTick(upperPrice);
    amount0Desired = 1_000_000;
    amount1Desired = 1_000_000;
    amount0Min = 990_000;
    amount1Min = 990_000;
    recipient = Principal.fromText("...");
    deadline = Time.now() + 15 * 60 * 1_000_000_000;
});
```

### Auto-Compound Strategy
```motoko
// Set up auto-compounding
let strategy = await liquidityManager.setAutoCompound({
    positionId = positionId;
    frequency = #Daily;
    minReward = 100_000;
});
``` 