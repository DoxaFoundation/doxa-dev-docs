---
title: "USDx Token Integration"
linkTitle: "USDx Token Integration"
weight: 4
description: "Guide for integrating with the USDx stablecoin"
---

This guide explains how to integrate with the USDx stablecoin in the Doxa Protocol.

## Overview

USDx is an ICRC-1 and ICRC-2 compliant stablecoin built on the Internet Computer. It maintains a stable 1:1 peg with USD through a multi-collateral backing system.

## Token Standards

### ICRC-1 Standard Implementation
The USDx token implements the ICRC-1 standard for fungible tokens:

```motoko
// Check balance
icrc1_balance_of : shared query (Account) -> async Nat

// Transfer tokens
icrc1_transfer : shared (TransferArg) -> async TransferResult

// Get metadata
icrc1_metadata : shared query () -> async [(Text, MetadataValue)]
```

### ICRC-2 Approval Standard
Extended functionality for allowances and approvals:

```motoko
// Approve spending
icrc2_approve : shared (ApproveArgs) -> async ApproveResult

// Check allowance
icrc2_allowance : shared query (AllowanceArgs) -> async Allowance
```

## Integration Examples

### 1. Basic Transfer
```motoko
// Transfer 100 USDx
let transferArgs : TransferArg = {
    to = recipient;
    amount = 100_000_000; // 100 USDx (8 decimals)
    fee = ?10_000;
    memo = null;
    created_at_time = null;
};

let result = await token.icrc1_transfer(transferArgs);
```

### 2. Approval Flow
```motoko
// Approve spending
let approvalArgs : ApproveArgs = {
    spender = spenderPrincipal;
    amount = 1_000_000_000; // 1000 USDx
    expires_at = ?(Time.now() + 24 * 60 * 60 * 1_000_000_000); // 24 hours
};

let approval = await token.icrc2_approve(approvalArgs);
```

## Token Specifications

### Technical Details
```motoko
type TokenSpec = {
    name : Text = "USDx Stablecoin";
    symbol : Text = "USDx";
    decimals : Nat8 = 8;
    fee : Nat = 10_000; // 0.0001 USDx
};
```

### Minting & Burning
```motoko
// Mint new tokens (restricted to authorized minters)
public shared ({ caller }) func mint(
    to: Account,
    amount: Nat
) : async Result<Nat, Text>

// Burn tokens
public shared ({ caller }) func burn(
    amount: Nat
) : async Result<Nat, Text>
```

## Price Feed Integration

### Get Current Price
```motoko
public query func getPrice() : async Nat64
```

### Price Update Mechanism
```motoko
public shared ({ caller }) func updatePrice(
    newPrice: Nat64,
    timestamp: Int
) : async Result<(), Text>
```

## Error Handling

Common token operation errors:
```motoko
type TokenError = variant {
    InsufficientBalance;
    InsufficientAllowance;
    Unauthorized;
    InvalidAmount;
    TransferFailed;
};
```

## Best Practices

1. **Transaction Management**
   - Always check transaction results
   - Handle all error cases
   - Implement proper timeouts
   - Use appropriate gas limits

2. **Security Considerations**
   - Validate input amounts
   - Check allowances before transfers
   - Implement rate limiting
   - Monitor for suspicious activity

3. **Integration Tips**
   - Use the testnet first
   - Start with small amounts
   - Implement proper logging
   - Monitor transaction status

4. **Performance Optimization**
   - Batch transactions when possible
   - Cache balances locally
   - Optimize approval amounts
   - Monitor gas usage 