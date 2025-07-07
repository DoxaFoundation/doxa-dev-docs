---
title: "Integration Guide"
linkTitle: "Integration Guide"
weight: 1
description: "Guide for integrating with the Doxa Protocol"
---

This guide provides a comprehensive overview of integrating with the Doxa Protocol.

# Doxa Protocol Integration Guide

This document is intended for developers who want to understand and integrate with the Doxa Protocol. Before reading this document, you need:

* Knowledge of Internet Computer blockchain
* Development environment capable of running IC canisters
* Understanding of DeFi concepts
* Motoko/Rust programming experience

## Navigation

* [DUSD Token Integration](#dusd-token-integration)
* [Staking Integration](#staking-integration)
* [Pool Operations](#pool-operations)
* [Fee Collection](#fee-collection)
* [Transaction Monitoring](#transaction-monitoring)

## Core Canister IDs

| Component | Principal ID |
|-----------|-------------|
| DUSD Token | irorr-5aaaa-aaaak-qddsq-cai |
| DUSD Index | modmy-byaaa-aaaag-qndgq-cai |
| Staking | mhahe-xqaaa-aaaag-qndha-cai |
| ckUSDC | xevnm-gaaaa-aaaar-qafnq-cai |

## DUSD Token Integration

### ICRC-1 Standard Methods
```motoko
// Check balance
icrc1_balance_of : shared query (Account) -> async Nat

// Transfer tokens
icrc1_transfer : shared (TransferArg) -> async TransferResult

// Get metadata
icrc1_metadata : shared query () -> async [(Text, MetadataValue)]
```

### Token Specifications
- **Decimals**: 6 (1 DUSD = 1,000,000 units)
- **Symbol**: DUSD
- **Name**: Doxa USD
- **Standard**: ICRC-1 compliant

## Staking Integration

### Initialize Stake
```motoko
public shared ({ caller }) func stake(
    amount: Nat
) : async Result<StakeId, Text> {
    // Minimum stake: 10 DUSD (10,000,000 units)
    // Lock duration: 30 days
    // APY: 20% base rate
}
```

### Configuration Parameters
```motoko
MIN_LOCK_DURATION = 30 days
MIN_STAKE_AMOUNT = 10_000_000 // 10 DUSD
BASE_APY = 20 // 20%
```

### Harvest Rewards
```motoko
public shared ({ caller }) func harvestRewards(
    stakeId: StakeId
) : async Result<Nat, Text>
```

## Pool Operations

### AMM Swaps
```motoko
public shared func swap(
    tokenIn: Text,
    tokenOut: Text,
    amountIn: Nat
) : async Result<Nat, Text>
```

### Price Queries
```motoko
public query func getPrice(
    token0: Text,
    token1: Text
) : async Float
```

## Minting DUSD

### Notify Mint Function
```motoko
public shared ({ caller }) func notify_mint_with_ckusdc({
    ckusdc_block_index : Nat;
    minting_token : Text;
}) : async Result<Nat, NotifyError>
```

### Process Flow
1. Deposit ckUSDC to reserve account
2. Call notify_mint_with_ckusdc with block index
3. System validates ckUSDC transaction
4. Mints equivalent DUSD tokens (1:1 ratio)

## Error Handling

### Common Error Types
```motoko
type TransferError = variant {
    BadFee : { expected_fee : Tokens };
    InsufficientFunds : { balance : Tokens };
    TooOld;
    CreatedInFuture : { ledger_time : Timestamp };
    TemporarilyUnavailable;
    Duplicate : { duplicate_of : BlockIndex };
};

type NotifyError = variant {
    InvalidTransaction : Text;
    TransactionTooOld : Nat;
    UnexpectedError : Text;
};
```

## Best Practices

### 1. Transaction Safety
- Always validate amounts before operations
- Handle all error variants properly
- Check minimum thresholds

### 2. Gas Management
- Monitor cycle costs
- Implement retry mechanisms
- Use appropriate timeouts

### 3. State Management
- Cache frequently accessed data
- Implement proper error recovery
- Handle async call failures

## Integration Examples

### Basic DUSD Transfer
```motoko
let transferArg : TransferArg = {
    amount = 1_000_000; // 1 DUSD
    to = recipientAccount;
    fee = ?10_000; // Standard fee
    memo = null;
    from_subaccount = null;
    created_at_time = ?Nat64.fromIntWrap(Time.now());
};

let result = await DUSD.icrc1_transfer(transferArg);
```

### Stake DUSD Tokens
```motoko
// First approve the staking canister
let approveArg : ApproveArgs = {
    amount = 50_000_000; // 50 DUSD
    spender = { owner = STAKING_CANISTER; subaccount = null };
    fee = ?10_000;
    memo = null;
    from_subaccount = null;
    created_at_time = ?Nat64.fromIntWrap(Time.now());
    expires_at = null;
};

await DUSD.icrc2_approve(approveArg);

// Then stake the tokens
let stakeResult = await StakingCanister.stake(50_000_000);
```

## Rate Limits & Quotas

- **Transfer Operations**: No explicit limits
- **Query Calls**: 500 per second recommended
- **Update Calls**: Consider cycle costs
- **Staking Operations**: Once per lock period

## Testing Guidelines

### Local Development
1. Deploy canisters locally using dfx
2. Use test tokens for integration testing
3. Validate all error scenarios

### Mainnet Integration
1. Start with small amounts
2. Monitor transaction results
3. Implement proper logging
4. Have rollback procedures ready 