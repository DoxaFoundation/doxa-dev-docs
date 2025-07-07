---
title: "Transaction Monitoring"
linkTitle: "Transaction Monitoring"
weight: 6
description: "Guide for monitoring transactions in the Doxa Protocol"
---

This guide explains how to monitor and track transactions in the Doxa Protocol.

## Overview

The Doxa Protocol provides transaction monitoring capabilities through standard ICRC-1 ledger interfaces and index canisters for tracking DUSD token activities.

## Transaction Types

### Core Transactions
```motoko
type TransactionType = {
    #Transfer;     // DUSD transfers
    #Mint;         // DUSD minting from ckUSDC
    #Burn;         // DUSD burning (if supported)
    #Approve;      // Token approvals
    #Stake;        // Staking operations
    #Unstake;      // Unstaking operations
};
```

## Monitoring APIs

### DUSD Ledger Queries
```motoko
// Get transaction history from DUSD ledger
let DUSD_LEDGER = actor("irorr-5aaaa-aaaak-qddsq-cai") : {
    get_transactions : ({start: Nat; length: Nat}) -> async GetTransactionsResponse;
};

type GetTransactionsResponse = {
    first_index : Nat;
    log_length : Nat;
    transactions : [Transaction];
    archived_transactions : [{
        start : Nat;
        length : Nat;
        callback : shared query GetTransactionsRequest -> async TransactionRange;
    }];
};
```

### Account Transaction History
```motoko
// Using DUSD Index canister for account-specific queries
let DUSD_INDEX = actor("modmy-byaaa-aaaag-qndgq-cai") : {
    get_account_transactions : ({
        account : Account;
        start : ?Nat;
        max_results : Nat;
    }) -> async GetAccountTransactionsResponse;
};
```

## Basic Monitoring Examples

### Check Account Balance
```motoko
let balance = await DUSD_LEDGER.icrc1_balance_of({
    owner = userPrincipal;
    subaccount = null;
});
```

### Get Recent Transactions
```motoko
let recentTxs = await DUSD_LEDGER.get_transactions({
    start = 0;
    length = 100;
});
```

### Monitor Specific Account
```motoko
let accountTxs = await DUSD_INDEX.get_account_transactions({
    account = { owner = userPrincipal; subaccount = null };
    start = null;
    max_results = 50;
});
```

## Transaction Analysis

### Parse Transaction Data
```motoko
type Transaction = {
    burn : ?{
        amount : Nat;
        from : Account;
        spender : ?Account;
        memo : ?Blob;
        created_at_time : ?Nat64;
    };
    kind : Text;
    mint : ?{
        amount : Nat;
        to : Account;
        memo : ?Blob;
        created_at_time : ?Nat64;
    };
    approve : ?{
        fee : ?Nat;
        from : Account;
        memo : ?Blob;
        created_at_time : ?Nat64;
        amount : Nat;
        expected_allowance : ?Nat;
        expires_at : ?Nat64;
        spender : Account;
    };
    timestamp : Nat64;
    transfer : ?{
        amount : Nat;
        fee : ?Nat;
        from : Account;
        memo : ?Blob;
        created_at_time : ?Nat64;
        to : Account;
        spender : ?Account;
    };
};
```

## Staking Transaction Monitoring

### Check Staking Status
```motoko
let STAKING_CANISTER = actor("mhahe-xqaaa-aaaag-qndha-cai") : {
    getUserStakes : (Principal) -> async [StakeInfo];
};

type StakeInfo = {
    id : Nat;
    amount : Nat;
    lockTime : Int;
    rewardEarned : Nat;
    lastClaimed : Int;
};
```

## Best Practices

### 1. Efficient Querying
- Use pagination for large transaction sets
- Query specific time ranges when possible
- Cache frequently accessed data

### 2. Error Handling
```motoko
switch (await DUSD_LEDGER.get_transactions(request)) {
    case (#Ok(response)) {
        // Process transactions
    };
    case (#Err(error)) {
        // Handle error cases
    };
};
```

### 3. Rate Limiting
- Implement appropriate delays between queries
- Use query calls when possible (no cycles cost)
- Monitor canister cycle usage

## Integration Examples

### Basic Transaction Monitor
```motoko
actor TransactionMonitor {
    private var lastCheckedBlock : Nat = 0;
    
    public func checkNewTransactions() : async [Transaction] {
        let response = await DUSD_LEDGER.get_transactions({
            start = lastCheckedBlock;
            length = 100;
        });
        
        lastCheckedBlock += response.transactions.size();
        response.transactions;
    };
}
```

### Account Activity Tracker
```motoko
public func getAccountActivity(account : Account) : async [Transaction] {
    let result = await DUSD_INDEX.get_account_transactions({
        account = account;
        start = null;
        max_results = 100;
    });
    
    result.transactions;
}
```

## Common Use Cases

### 1. Portfolio Tracking
- Monitor DUSD balance changes
- Track staking rewards accumulation
- Calculate total value locked

### 2. Compliance Monitoring
- Transaction history for auditing
- Large transaction alerts
- Activity pattern analysis

### 3. DApp Integration
- Real-time balance updates
- Transaction confirmation
- User activity feeds

## Limitations

- Historical data depends on ledger retention
- Query limits based on canister capacity
- No real-time push notifications (polling required)
- Limited to on-chain transaction data

## Future Enhancements

- Enhanced query capabilities
- Real-time event subscriptions
- Advanced filtering options
- Cross-canister transaction correlation 