---
title: "Fee Collection"
linkTitle: "Fee Collection"
weight: 6
description: "Guide for fee structure and collection in the Doxa Protocol"
---

# Fee Collection

## Overview

Doxa Protocol implements a simple fee structure for swaps and protocol operations.

## Fee Structure

### Trading Fees
- **Standard Swaps**: 0.3% (30 basis points)
- **Stable Pairs**: 0.05% (5 basis points)
- **Exotic Pairs**: 1% (100 basis points)

### Protocol Fees
- **Minting DUSD**: No additional fee (1:1 with ckUSDC)
- **Staking**: No deposit/withdrawal fees
- **Token Transfers**: Standard ICRC-1 fees

## Fee Collection Process

### Swap Fees
```motoko
// Fees automatically collected during swaps
let swapAmount = 1_000_000; // 1 DUSD
let feeAmount = swapAmount * 30 / 10000; // 0.3% fee
let outputAmount = calculateSwapOutput(swapAmount - feeAmount);
```

### Staking Fees
```motoko
// No fees for staking operations
// Rewards distributed from protocol treasury
let stakingReward = calculateReward(stakedAmount, duration, apy);
```

## Fee Distribution

1. **Protocol Treasury**: 60% of collected fees
2. **Liquidity Providers**: 30% of collected fees  
3. **Staking Rewards**: 10% of collected fees

## Integration

### Calculate Trading Fees
```motoko
public func calculateTradingFee(amount: Nat, feeRate: Nat) : Nat {
    amount * feeRate / 10000
};

// Example usage
let tradeFee = calculateTradingFee(1_000_000, 30); // 0.3% of 1 DUSD
```

### Fee Queries
```motoko
// Get current fee rates
public query func getFeeRates() : async {
    standardSwap: Nat;
    stablePair: Nat;
    exoticPair: Nat;
} {
    {
        standardSwap = 30; // 0.3%
        stablePair = 5;    // 0.05%
        exoticPair = 100;  // 1%
    }
};
```

## Error Handling

```motoko
type FeeError = variant {
    #InsufficientAmount;
    #InvalidFeeRate;
    #CollectionFailed;
};
```

## Best Practices

1. **Fee Awareness**: Always account for fees in trade calculations
2. **Slippage Tolerance**: Set appropriate slippage to cover fees
3. **Route Optimization**: Choose optimal trading routes to minimize fees
4. **Balance Management**: Maintain sufficient balance to cover fees 