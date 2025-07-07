---
title: "Architecture Overview"
linkTitle: "Architecture Overview"
weight: 2
description: "Detailed system architecture of the Doxa Protocol"
---

This document provides a comprehensive overview of the Doxa Protocol's system architecture.

## System Components

### Core Components

The Doxa Protocol implements a simple yet robust stablecoin system with the following components:

1. **DUSD Token System**
   - ICRC-1 compliant stablecoin
   - 1:1 backed by ckUSDC collateral
   - 6 decimal precision (1 DUSD = 1,000,000 units)

2. **Stablecoin Minter**
   - Handles ckUSDC deposits
   - Validates collateral transactions
   - Mints DUSD tokens 1:1 ratio

3. **Staking System**
   - 20% base APY staking rewards
   - 30-day minimum lock period
   - Bootstrap phase for early adopters

4. **AMM Trading System**
   - Token swaps and price discovery
   - Liquidity provision capabilities
   - Multi-token support

### Canister Architecture

```motoko
// Core Canisters
DUSD_TOKEN: "irorr-5aaaa-aaaak-qddsq-cai"     // ICRC-1 token
STAKING:    "mhahe-xqaaa-aaaag-qndha-cai"     // Staking rewards
MINTER:     "[stablecoin-minter-canister]"    // ckUSDC -> DUSD
SWAP:       "[swap-canister]"                 // AMM functionality
```

## Data Flow

### Minting Process
1. User deposits ckUSDC to reserve account
2. Minter validates ckUSDC transaction on ledger
3. Equivalent DUSD tokens minted 1:1 ratio
4. DUSD transferred to user account

### Staking Process
1. User approves DUSD transfer to staking canister
2. Stake position created with 30-day lock
3. Rewards calculated at 20% base APY
4. Bootstrap bonuses applied during early phase

### Trading Process
1. User initiates swap via AMM
2. Price calculated based on liquidity pools
3. Tokens exchanged with 0.3% trading fee
4. Updated balances reflected on-chain

## Inter-Canister Communication

```motoko
// Example: Minting DUSD
actor StablecoinMinter {
    let DUSD : Icrc.Self = actor("irorr-5aaaa-aaaak-qddsq-cai");
    let ckUSDC : Icrc.Self = actor("xevnm-gaaaa-aaaar-qafnq-cai");
    
    public shared func notify_mint_with_ckusdc({
        ckusdc_block_index : Nat;
        minting_token : Text;
    }) : async Result<Nat, NotifyError> {
        // Validate ckUSDC transaction
        let validation = await validateCkUsdcTransaction(ckusdc_block_index);
        
        // Mint equivalent DUSD
        let result = await DUSD.icrc1_transfer({
            amount = validation.amount;
            to = validation.account;
            // ... other parameters
        });
        
        result;
    };
}
```

## State Management

### Stable Storage
```motoko
// Staking canister state
private stable var stakingPool = {
    apy = 20;
    totalStaked = 0;
    minimumStake = 10_000_000; // 10 DUSD
    lockDuration = 2_592_000_000_000_000; // 30 days in nanoseconds
};

// Minter canister state  
private stable var reserveAccount : Account = {
    owner = Principal.fromText("ckusdc-reserve-account");
    subaccount = null;
};
```

### Memory Management
- Efficient state serialization
- Garbage collection optimization
- Canister upgrade compatibility

## Security Architecture

### Collateral Validation
```motoko
private func validateCkUsdcBlockForMint(
    blockIndex : Nat,
    caller : Principal,
    token : Text
) : async Result<(Nat, Account), NotifyError> {
    // Get transaction from ckUSDC ledger
    let transaction = await ckUSDC.get_transactions({
        start = blockIndex;
        length = 1;
    });
    
    // Validate minimum amount (1 ckUSDC)
    if (transfer.amount < 1_000_000) {
        return #err(#InvalidTransaction("Below minimum deposit"));
    };
    
    // Verify destination is reserve account
    if (transfer.to != reserveAccount) {
        return #err(#InvalidTransaction("Invalid destination"));
    };
    
    #ok(transfer.amount, transfer.from);
};
```

### Access Control
- Role-based permissions for admin functions
- User authentication via Internet Identity
- Time-locked operations for security

## Performance Characteristics

### Transaction Throughput
- **Token Transfers**: ~2 second finality
- **Staking Operations**: Single block confirmation
- **Minting**: Depends on ckUSDC ledger validation
- **Swaps**: Real-time price calculation

### Scalability Design
- Independent canister scaling
- Optimized query functions
- Efficient state management

## Integration Points

### ICRC-1 Compatibility
```motoko
// Standard token interface
public query func icrc1_balance_of(account : Account) : async Nat
public shared func icrc1_transfer(args : TransferArg) : async TransferResult
public query func icrc1_metadata() : async [(Text, MetadataValue)]
```

### DApp Integration
- Standard token interfaces
- Query-based price feeds
- Event notification system

## Monitoring & Health

### System Metrics
```motoko
public query func getSystemStats() : async {
    totalDUSDSupply: Nat;
    totalCkUSDCReserves: Nat;
    activeStakes: Nat;
    totalValueLocked: Nat;
}
```

### Health Checks
- Collateralization ratio monitoring
- Canister cycle tracking
- Transaction success rates

## Future Architecture

### Planned Enhancements
1. Multi-collateral support expansion
2. Advanced staking reward mechanisms
3. Cross-chain bridge integrations
4. Enhanced AMM features

### Scalability Considerations
- Subnet deployment strategies
- Load balancing mechanisms
- State sharding possibilities 