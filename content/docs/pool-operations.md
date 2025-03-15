---
title: "Pool Operations"
linkTitle: "Pool Operations"
weight: 1
description: "Guide for interacting with liquidity pools in the Doxa Protocol"
---

This guide explains how to interact with liquidity pools in the Doxa Protocol.

## Overview

Doxa Protocol uses an Automated Market Maker (AMM) model similar to Uniswap v3, with concentrated liquidity positions and multiple fee tiers.

## Pool Types

### Standard Pools
```motoko
type PoolType = {
    #Standard: {
        fee: Fee;
        tickSpacing: Nat;
    };
    #Stable: {
        fee: Fee;
        amplifier: Nat;
    };
};
```

### Fee Tiers
| Fee Tier | Description | Best For |
|----------|-------------|----------|
| 0.01% | Ultra low fee | Stable pairs |
| 0.05% | Low fee | Correlated pairs |
| 0.3% | Medium fee | Most pairs |
| 1% | High fee | Exotic pairs |

## Pool Creation

### Create New Pool
```motoko
public shared func createPool(
    token0: Principal,
    token1: Principal,
    fee: Fee,
    sqrtPriceX96: Nat,
    tickSpacing: Nat
) : async Result<Principal, Text>
```

### Initialize Pool
```motoko
public shared func initializePool(
    poolId: Principal,
    sqrtPriceX96: Nat
) : async Result<(), Text>
```

## Liquidity Management

### Add Liquidity
```motoko
public shared func addLiquidity(
    params: {
        poolId: Principal;
        recipient: Principal;
        tickLower: Int;
        tickUpper: Int;
        amount0Desired: Nat;
        amount1Desired: Nat;
        amount0Min: Nat;
        amount1Min: Nat;
        deadline: Int;
    }
) : async Result<AddLiquidityResult, Text>
```

### Remove Liquidity
```motoko
public shared func removeLiquidity(
    params: {
        poolId: Principal;
        positionId: Nat;
        amount0Min: Nat;
        amount1Min: Nat;
        deadline: Int;
    }
) : async Result<RemoveLiquidityResult, Text>
```

## Trading Operations

### Swap Tokens
```motoko
public shared func swap(
    params: {
        poolId: Principal;
        recipient: Principal;
        zeroForOne: Bool;
        amountSpecified: Int;
        sqrtPriceLimitX96: Nat;
        deadline: Int;
    }
) : async Result<SwapResult, Text>
```

### Quote Price
```motoko
public query func quotePrice(
    poolId: Principal,
    amountIn: Nat,
    zeroForOne: Bool
) : async Result<QuoteResult, Text>
```

## Position Management

### Get Position Info
```motoko
public query func getPosition(
    positionId: Nat
) : async ?Position

type Position = {
    owner: Principal;
    poolId: Principal;
    tickLower: Int;
    tickUpper: Int;
    liquidity: Nat;
    feeGrowthInside0LastX128: Nat;
    feeGrowthInside1LastX128: Nat;
    tokensOwed0: Nat;
    tokensOwed1: Nat;
};
```

### Collect Fees
```motoko
public shared func collectFees(
    positionId: Nat
) : async Result<{
    amount0: Nat;
    amount1: Nat;
}, Text>
```

## Pool Analytics

### Get Pool State
```motoko
public query func getPoolState(
    poolId: Principal
) : async PoolState

type PoolState = {
    liquidity: Nat;
    sqrtPriceX96: Nat;
    tick: Int;
    observationIndex: Nat;
    observationCardinality: Nat;
    feeProtocol: Nat;
    unlocked: Bool;
};
```

### Get Pool Statistics
```motoko
public query func getPoolStats(
    poolId: Principal
) : async PoolStats

type PoolStats = {
    tvl: Nat;
    volume24h: Nat;
    fees24h: Nat;
    apy: Float;
};
```

## Oracle Functions

### Get Time-Weighted Average Price (TWAP)
```motoko
public query func getTWAP(
    poolId: Principal,
    secondsAgo: Nat
) : async Result<Nat, Text>
```

### Get Price Observation
```motoko
public query func getObservation(
    poolId: Principal,
    index: Nat
) : async ?Observation
```

## Error Handling

Common pool operation errors:
```motoko
type PoolError = variant {
    InsufficientLiquidity;
    InvalidPrice;
    InvalidTick;
    LiquidityOverflow;
    NotFound;
    PriceLimitReached;
    SlippageExceeded;
    TickOutOfRange;
    Unauthorized;
};
```

## Best Practices

### 1. Price Impact
- Calculate price impact before trades
- Set appropriate slippage tolerance
- Use price limits for large trades

### 2. Gas Optimization
- Batch similar operations
- Choose optimal fee tiers
- Consider tick spacing

### 3. Risk Management
- Monitor position health
- Set reasonable price ranges
- Understand impermanent loss

### 4. Integration Tips
- Handle all error cases
- Implement proper timeouts
- Monitor pool state changes

## Common Use Cases

### 1. Market Making
- Provide concentrated liquidity
- Manage active ranges
- Rebalance positions

### 2. Trading
- Execute swaps efficiently
- Monitor price movements
- Optimize trade paths

### 3. Portfolio Management
- Track positions
- Collect fees regularly
- Adjust ranges as needed 