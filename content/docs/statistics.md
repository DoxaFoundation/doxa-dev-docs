---
title: "Statistics & Data"
linkTitle: "Statistics & Data"
weight: 7
description: "Guide for accessing protocol statistics and data"
---

This guide explains how to access basic statistics in the Doxa Protocol.

## Overview

The Doxa Protocol provides basic statistics through canister query functions for monitoring protocol health and activity.

## Basic Protocol Statistics

### DUSD Token Stats
```motoko
// Get DUSD token metadata and supply info
let DUSD = actor("irorr-5aaaa-aaaak-qddsq-cai") : {
    icrc1_total_supply : () -> async Nat;
    icrc1_metadata : () -> async [(Text, MetadataValue)];
    icrc1_fee : () -> async Nat;
};

// Example usage
let totalSupply = await DUSD.icrc1_total_supply();
let metadata = await DUSD.icrc1_metadata();
```

### Staking Statistics
```motoko
// Get staking pool information
let STAKING = actor("mhahe-xqaaa-aaaag-qndha-cai") : {
    getPoolInfo : () -> async PoolInfo;
    getTotalStaked : () -> async Nat;
    getActiveStakers : () -> async Nat;
};

type PoolInfo = {
    apy : Nat;                    // Current APY (20)
    totalStaked : Nat;            // Total DUSD staked
    minimumStake : Nat;           // Minimum stake amount
    lockDuration : Nat;           // Lock duration in nanoseconds
};
```

## Account-Level Statistics

### User Balance Queries
```motoko
// Check user's DUSD balance
public func getUserBalance(account : Account) : async Nat {
    await DUSD.icrc1_balance_of(account);
}

// Get user's staking positions
public func getUserStakes(user : Principal) : async [StakeInfo] {
    await STAKING.getUserStakes(user);
}
```

### Portfolio Overview
```motoko
type UserStats = {
    dusdBalance : Nat;
    totalStaked : Nat;
    totalRewards : Nat;
    activeStakes : Nat;
};

public func getUserStats(user : Principal) : async UserStats {
    let account = { owner = user; subaccount = null };
    let balance = await DUSD.icrc1_balance_of(account);
    let stakes = await STAKING.getUserStakes(user);
    
    let totalStaked = Array.foldLeft<StakeInfo, Nat>(
        stakes, 0, func(acc, stake) = acc + stake.amount
    );
    
    {
        dusdBalance = balance;
        totalStaked = totalStaked;
        totalRewards = 0; // Calculate from stakes
        activeStakes = stakes.size();
    };
}
```

## System Health Metrics

### Basic Health Check
```motoko
public query func getSystemHealth() : async {
    dusdSupply : Nat;
    stakingActive : Bool;
    lastUpdated : Int;
} {
    {
        dusdSupply = await DUSD.icrc1_total_supply();
        stakingActive = true; // Check staking canister status
        lastUpdated = Time.now();
    };
}
```

### Collateralization Ratio
```motoko
// Monitor backing ratio (should be 1:1 with ckUSDC)
public func getCollateralizationRatio() : async Float {
    let dusdSupply = await DUSD.icrc1_total_supply();
    let ckusdcReserves = await getCkUsdcReserves();
    
    if (dusdSupply == 0) { 0.0 }
    else { Float.fromInt(ckusdcReserves) / Float.fromInt(dusdSupply) };
}
```

## Transaction Volume

### Daily Activity
```motoko
public func getDailyVolume() : async {
    transfers : Nat;
    mints : Nat;
    stakes : Nat;
} {
    // Query recent transactions from ledger
    let recent = await DUSD.get_transactions({
        start = 0;
        length = 1000;
    });
    
    // Count different transaction types
    var transfers = 0;
    var mints = 0;
    
    for (tx in recent.transactions.vals()) {
        switch (tx.transfer) {
            case (?_) { transfers += 1; };
            case null {};
        };
        switch (tx.mint) {
            case (?_) { mints += 1; };
            case null {};
        };
    };
    
    {
        transfers = transfers;
        mints = mints;
        stakes = 0; // Get from staking canister
    };
}
```

## Price Information

### Basic Price Queries
```motoko
// Simple price calculation (if AMM is available)
public func getDUSDPrice() : async ?Float {
    // Query from swap canister if available
    // For now, DUSD should maintain 1:1 with USD
    ?1.0;
}
```

## Data Export

### CSV-Style Data
```motoko
public func exportBasicStats() : async Text {
    let supply = await DUSD.icrc1_total_supply();
    let poolInfo = await STAKING.getPoolInfo();
    
    "DUSD Supply," # Nat.toText(supply) # "\n" #
    "Total Staked," # Nat.toText(poolInfo.totalStaked) # "\n" #
    "Current APY," # Nat.toText(poolInfo.apy) # "%\n";
}
```

## Best Practices

### 1. Efficient Querying
- Use query calls when possible (no cycle cost)
- Cache frequently accessed data
- Implement reasonable polling intervals

### 2. Error Handling
```motoko
public func getSafeStats() : async ?PoolInfo {
    try {
        ?(await STAKING.getPoolInfo());
    } catch (e) {
        null; // Handle canister unavailability
    };
}
```

### 3. Rate Limiting
- Don't overwhelm canisters with requests
- Use appropriate delays between calls
- Monitor your own cycle consumption

## Integration Examples

### Simple Dashboard Data
```motoko
type DashboardData = {
    totalDUSD : Nat;
    totalStaked : Nat;
    currentAPY : Nat;
    activeUsers : Nat;
};

public func getDashboardData() : async DashboardData {
    let supply = await DUSD.icrc1_total_supply();
    let poolInfo = await STAKING.getPoolInfo();
    
    {
        totalDUSD = supply;
        totalStaked = poolInfo.totalStaked;
        currentAPY = poolInfo.apy;
        activeUsers = 0; // Would need to track separately
    };
}
```

### User Portfolio Summary
```motoko
public func getPortfolioSummary(user : Principal) : async {
    balance : Nat;
    staked : Nat;
    earned : Nat;
} {
    let account = { owner = user; subaccount = null };
    let balance = await DUSD.icrc1_balance_of(account);
    let stakes = await STAKING.getUserStakes(user);
    
    let totalStaked = Array.foldLeft<StakeInfo, Nat>(
        stakes, 0, func(acc, stake) = acc + stake.amount
    );
    
    {
        balance = balance;
        staked = totalStaked;
        earned = 0; // Calculate from stake rewards
    };
}
```

## Limitations

- Limited historical data retention
- No complex analytics or charting
- Basic aggregation capabilities only
- Dependent on individual canister query limits

## Future Analytics

- Enhanced data aggregation
- Historical trend analysis  
- Advanced portfolio analytics
- Real-time dashboards 