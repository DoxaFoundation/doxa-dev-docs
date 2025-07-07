---
title: "DUSD Token Integration"
linkTitle: "DUSD Token Integration"
weight: 4
description: "Guide for integrating with the DUSD stablecoin"
---

# DUSD Token Integration

## Overview

DUSD (Doxa USD) is an ICRC-1 compliant stablecoin on the Internet Computer, backed 1:1 by ckUSDC collateral.

**Token Details:**
- **Symbol**: DUSD
- **Name**: Doxa USD
- **Standard**: ICRC-1
- **Decimals**: 6
- **Canister ID**: `irorr-5aaaa-aaaak-qddsq-cai`

## Basic Integration

### Minting DUSD

```motoko
// 1. Get reserve account
let reserveAccount = await stablecoinMinter.get_ckusdc_reserve_account_of({
    token = #DUSD
});

// 2. Transfer ckUSDC to reserve
let transferResult = await ckUSDC.icrc1_transfer({
    to = reserveAccount;
    amount = 1_000_000; // 1 ckUSDC minimum
    fee = ?10_000;
    memo = null;
    from_subaccount = null;
    created_at_time = ?Nat64.fromIntWrap(Time.now());
});

// 3. Notify minter
switch (transferResult) {
    case (#Ok(blockIndex)) {
        await stablecoinMinter.notify_mint_with_ckusdc({
            ckusdc_block_index = blockIndex;
            minting_token = #DUSD;
        });
    };
    case (#Err(error)) { /* Handle error */ };
};
```

### Token Operations

```motoko
// Check balance
let balance = await DUSD.icrc1_balance_of({
    owner = userPrincipal;
    subaccount = null;
});

// Transfer tokens
let transferResult = await DUSD.icrc1_transfer({
    to = { owner = recipientPrincipal; subaccount = null };
    amount = 10_000_000; // 10 DUSD
    fee = null;
    memo = null;
    from_subaccount = null;
    created_at_time = ?Nat64.fromIntWrap(Time.now());
});
```

## Requirements

- **Minimum mint**: 1 ckUSDC (1,000,000 units)
- **Collateral**: ckUSDC only
- **Ratio**: 1:1 (1 ckUSDC = 1 DUSD)
- **Network**: Internet Computer mainnet

## Error Handling

```motoko
type MintError = {
    #AlreadyProcessed : { blockIndex : Nat };
    #InvalidTransaction : Text;
    #Other : { error_message : Text; error_code : Nat64 };
};
```

Common errors:
- `InvalidTransaction`: Insufficient amount or wrong destination
- `AlreadyProcessed`: Transaction already used for minting
- `Other`: Transfer or system errors 