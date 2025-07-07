---
title: "Pool Operations"
linkTitle: "Pool Operations"
weight: 5
description: "Guide for pool operations in the Doxa Protocol"
---

# Pool Operations

## Overview

Doxa Protocol implements AMM-style trading pools for token swaps. The system supports DUSD trading pairs with automatic price calculation.

## Supported Tokens

Based on the actual implementation:
- **DUSD**: Primary stablecoin (6 decimals)
- **ckUSDC**: Collateral token (6 decimals)
- **ICP**: Internet Computer token
- **Other ICRC-1 tokens**: As configured

## Basic Swap Operations

### Get Pool Price

```motoko
// Get current price between tokens
let price = await swapCanister.getInitialPrice(token0Id, token1Id);

// Check token decimals
let decimals0 = await swapCanister.getDecimals(token0Id);
let decimals1 = await swapCanister.getDecimals(token1Id);
```

### Execute Swap

```motoko
// Basic swap function
let swapResult = await swapCanister.swap({
    tokenIn = "irorr-5aaaa-aaaak-qddsq-cai"; // DUSD
    tokenOut = "xevnm-gaaaa-aaaar-qafnq-cai"; // ckUSDC
    amountIn = 1_000_000; // 1 DUSD
    minimumAmountOut = 950_000; // Minimum 0.95 ckUSDC
    deadline = Time.now() + 300_000_000_000; // 5 minutes
});
```

## Pool Information

### Pool Data Structure

```motoko
type PoolData = {
    token0: Text;
    token1: Text;
    reserve0: Nat;
    reserve1: Nat;
    totalSupply: Nat;
    fee: Nat; // Fee in basis points
};
```

### Get Pool Details

```motoko
// Get pool information
let poolInfo = await swapCanister.getCreatedPoolData(poolId);

// Check pool exists and is active
let poolExists = await swapCanister.poolExists(token0Id, token1Id);
```

## Fee Structure

- **Standard Fee**: 0.3% (30 basis points)
- **Stable Pairs**: 0.05% (5 basis points)  
- **Exotic Pairs**: 1% (100 basis points)

## Price Calculation

The swap uses automated market maker formulas:

```motoko
// Constant product formula: x * y = k
// Price impact based on reserve ratios
// Slippage protection through minimum output amounts
```

## Error Handling

```motoko
type SwapError = variant {
    #InsufficientBalance;
    #InsufficientLiquidity;
    #ExcessiveSlippage;
    #DeadlineExceeded;
    #InvalidToken;
    #PoolNotFound;
};
```

## Best Practices

1. **Slippage Protection**: Always set reasonable minimum output amounts
2. **Deadline Management**: Use appropriate transaction deadlines
3. **Token Validation**: Verify token addresses before swapping
4. **Balance Checks**: Ensure sufficient balance before attempting swaps
5. **Error Handling**: Implement proper error handling for all scenarios

## Integration Example

```motoko
// Complete swap integration
let user_balance = await DUSD.icrc1_balance_of({
    owner = userPrincipal;
    subaccount = null;
});

if (user_balance >= swapAmount) {
    let swapResult = await performSwap(
        tokenIn,
        tokenOut, 
        swapAmount,
        minimumOut,
        deadline
    );
    
    switch (swapResult) {
        case (#ok(outputAmount)) {
            Debug.print("Swap successful: " # Nat.toText(outputAmount));
        };
        case (#err(error)) {
            Debug.print("Swap failed: " # debug_show(error));
        };
    };
};
``` 