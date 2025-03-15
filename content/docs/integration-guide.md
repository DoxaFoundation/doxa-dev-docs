# Doxa Protocol Integration Guide

This document is intended for developers who want to understand and integrate with the Doxa Protocol. Before reading this document, you need:

* Knowledge of Internet Computer blockchain
* Development environment capable of running IC canisters
* Understanding of DeFi concepts
* Motoko/Rust programming experience

## Navigation

* [Pool Operations](#pool-operations)
* [Staking Integration](#staking-integration)
* [USDx Token Integration](#usdx-token-integration)
* [Liquidity Management](#liquidity-management)
* [Fee Collection](#fee-collection)
* [Transaction Monitoring](#transaction-monitoring)
* [Statistics and Data](#statistics-and-data)

## Testing Environment

### Canister IDs

| Component | Principal ID |
|-----------|-------------|
| USDx Token | irorr-5aaaa-aaaak-qddsq-cai |
| USDx Index | modmy-byaaa-aaaag-qndgq-cai |
| Staking | mhahe-xqaaa-aaaag-qndha-cai |
| CKUSDC Pool | iyn2n-liaaa-aaaak-qddta-cai |
| Fee Collector | ieja4-4iaaa-aaaak-qddra-cai |

## Core Features

### 1. Pool Operations

#### Creating a Pool
```motoko
public shared func create(
    firstTokenId: Text, 
    secondTokenId: Text
) : async Result<SwapFactory.PoolData, Text>
```

Example:
```motoko
let poolArgs : CreatePoolArgs = {
    fee = 3_000; // 0.3% fee
    sqrtPriceX96 = Int.toText(sqrtPrice);
    subnet = null;
    token0 = { address = token0Id; standard = "ICRC2" };
    token1 = { address = token1Id; standard = "ICRC2" };
};
```

#### Adding Liquidity
```motoko
public shared func addLiquidity(
    poolId: Principal,
    amount0Desired: Nat,
    amount1Desired: Nat
) : async Result<Position, Text>
```

### 2. Staking Integration

#### Initialize Stake
```motoko
// Stake tokens
public shared ({ caller }) func stake(
    amount: Nat,
    duration: Nat
) : async Result<StakeId, Text>
```

Configuration Parameters:
```motoko
MIN_LOCK_DURATION = 30 days
MAX_LOCK_DURATION = 365 days
MIN_STAKE_AMOUNT = 100 tokens
```

#### Harvest Rewards
```motoko
public shared ({ caller }) func harvestRewards(
    stakeId: StakeId
) : async Result<Nat, Text>
```

### 3. USDx Token Integration

#### ICRC-1 Standard Methods
```motoko
// Check balance
icrc1_balance_of : shared query (Account) -> async Nat

// Transfer tokens
icrc1_transfer : shared (TransferArg) -> async TransferResult

// Get metadata
icrc1_metadata : shared query () -> async [(Text, MetadataValue)]
```

#### ICRC-2 Approval Methods
```motoko
// Approve spending
icrc2_approve : shared (ApproveArgs) -> async ApproveResult

// Check allowance
icrc2_allowance : shared query (AllowanceArgs) -> async Allowance
```

### 4. Fee Collection

#### Fee Structure
```motoko
// Pool creation fee
poolCreationFee : Nat = 100_000_000; // 1 ICP

// Transaction approval fee
approvalFee : Nat = 10_000; // 0.0001 ICP
```

#### Weekly Reward Distribution
```motoko
public shared ({ caller }) func weekly_reward_approval(
    {
        memo;
        created_at_time;
        amount;
        expires_at
    } : RewardApprovalArg
) : async Result<Nat, RewardApprovalErr>
```

### 5. Transaction Monitoring

#### Get Transaction History
```motoko
public query func get_account_transactions(
    args: GetAccountTransactionsArgs
) : async GetTransactionsResult
```

Response Structure:
```motoko
type GetTransactions = {
    balance : Tokens;
    transactions : [TransactionWithId];
    oldest_tx_id : ?BlockIndex;
};
```

### 6. Statistics and Data

#### Pool Statistics
```motoko
public query func getPoolPrices() : async [{
    name : Text;
    price : Float;
}]
```

#### Position Information
```motoko
public func getPositionAmount() : async [{
    name : Text;
    amount0 : Int;
    amount1 : Int;
}]
```

## Error Handling

Common error types and their meanings:

```motoko
type TransferError = variant {
    BadFee : { expected_fee : Tokens };
    InsufficientFunds : { balance : Tokens };
    TooOld;
    CreatedInFuture : { ledger_time : Timestamp };
    TemporarilyUnavailable;
    Duplicate : { duplicate_of : BlockIndex };
};
```

## Best Practices

### 1. Transaction Safety
- Always check return values
- Handle all error variants
- Implement proper timeouts

### 2. Gas Optimization
- Batch transactions when possible
- Use appropriate fee settings
- Monitor gas consumption

### 3. Security
- Validate all inputs
- Implement proper access control
- Use secure approval flows

### 4. Rate Limiting
- Implement cooldown periods
- Monitor transaction frequency
- Handle congestion gracefully 