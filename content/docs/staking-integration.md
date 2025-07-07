---
title: "Staking Integration"
linkTitle: "Staking Integration"
weight: 3
description: "Guide for integrating with Doxa Protocol's staking system"
---

# DUSD Staking Integration

## Overview

Doxa Protocol offers DUSD staking with 20% base APY and a 30-day minimum lock period. The system includes bootstrap phase rewards for early participants.

## Staking Parameters

```motoko
// Core staking constants
MIN_LOCK_DURATION: 2_592_000_000_000_000 nanoseconds (30 days)
MIN_STAKE_AMOUNT: 10_000_000 units (10 DUSD)
MAX_STAKE_PER_ADDRESS: 20_000_000_000 units (20,000 DUSD)
BASE_APY: 20%
TOKEN: DUSD (irorr-5aaaa-aaaak-qddsq-cai)
```

## Basic Staking

### Stake DUSD Tokens

```motoko
// Stake 50 DUSD for 30 days
let stakeResult = await doxaStaking.stakeTokens({
    amount = 50_000_000; // 50 DUSD (6 decimals)
    lockDuration = 2_592_000_000_000_000; // 30 days minimum
});

switch (stakeResult) {
    case (#ok(stakeId)) {
        Debug.print("Staked successfully, ID: " # stakeId);
    };
    case (#err(error)) {
        Debug.print("Staking failed: " # error);
    };
};
```

### Check Staking Status

```motoko
// Get stake information
let stakeInfo = await doxaStaking.getStakeInfo(stakeId);

// Check available rewards
let rewards = await doxaStaking.getRewards(userPrincipal);

// Get pool information
let poolInfo = await doxaStaking.getPoolInfo();
```

### Claim Rewards

```motoko
// Claim accumulated rewards
let claimResult = await doxaStaking.claimRewards();

switch (claimResult) {
    case (#ok(amount)) {
        Debug.print("Claimed " # Nat.toText(amount) # " DUSD rewards");
    };
    case (#err(error)) {
        Debug.print("Claim failed: " # error);
    };
};
```

## Unstaking

```motoko
// Unstake after lock period expires
let unstakeResult = await doxaStaking.unstake(stakeId);

switch (unstakeResult) {
    case (#ok(amount)) {
        Debug.print("Unstaked " # Nat.toText(amount) # " DUSD");
    };
    case (#err(error)) {
        Debug.print("Unstaking failed: " # error);
    };
};
```

## Pool Configuration

The staking pool has the following settings:

```motoko
Pool Details:
- Name: "Doxa Dynamic Staking"
- Staking Token: DUSD
- Reward Token: DUSD
- APY: 20%
- Min Stake: 10 DUSD
- Max Stake per Address: 20,000 DUSD
- Lock Duration: 30 days minimum
- Bootstrap Phase: Enhanced rewards for early stakers
```

## Error Handling

```motoko
type StakingError = variant {
    #InsufficientBalance;
    #AmountTooLow;
    #AmountTooHigh;
    #StakeNotFound;
    #StakeLocked;
    #Unauthorized;
    #PoolNotActive;
};
```

## Best Practices

1. **Minimum Requirements**: Always stake at least 10 DUSD
2. **Lock Period**: Plan for 30-day minimum lock duration
3. **Reward Claims**: Claim rewards regularly to compound returns
4. **Pool Limits**: Respect the 20,000 DUSD maximum per address
5. **Bootstrap Phase**: Stake early for enhanced rewards 