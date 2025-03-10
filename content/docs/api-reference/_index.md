---
title: "API Reference"
description: "Complete API documentation for Doxa"
---

# API Reference

This section provides detailed documentation for all Doxa APIs.

## Swap Canister

### `swap_api`

Creates a new swap transaction.

**Parameters**
```candid
record {
    token_in: principal;
    amount_in: nat;
    token_out: principal;
}
```

**Returns**
```candid
record {
    amount_out: nat;
    cycles_used: nat;
}
```

**Example**
```bash
dfx canister call swap_canister swap_api '(
  record {
    token_in = principal "xeka7-7iaaa-..."; // USDC
    amount_in = 100_000_000; // 100 USDC
    token_out = principal "ryjl3-tyaaa-..."; // DOXA
  }
)'
```

### `get_pool_info`

Retrieves information about a specific pool.

**Parameters**
```candid
record {
    pool_id: principal;
}
```

**Returns**
```candid
record {
    token0: principal;
    token1: principal;
    reserve0: nat;
    reserve1: nat;
    fee: nat;
}
```

## Cycle Management

### `get_cycle_balance`

Gets the current cycle balance of a canister.

**Parameters**
None

**Returns**
```candid
nat
```

**Example**
```motoko
public query func get_cycle_balance() : async Nat {
    Cycles.balance()
};
```

### `transfer_cycles`

Transfers cycles to another canister.

**Parameters**
```candid
record {
    to: principal;
    amount: nat;
}
```

**Returns**
```candid
variant {
    Ok: nat;
    Err: text;
}
```

## Authentication

### `authenticate`

Authenticates a user with NFID.

**Parameters**
```candid
record {
    principal: principal;
    signature: vec nat8;
}
```

**Returns**
```candid
variant {
    Ok: text; // JWT token
    Err: text;
}
```

## Error Codes

| Code    | Description                    | Solution                    |
|---------|--------------------------------|-----------------------------|
| `IC0301`| Insufficient cycles           | Top up canister cycles      |
| `AUTH004`| Authentication failed        | Reconnect wallet           |
| `SWAP409`| Slippage too high           | Adjust parameters          |
| `POOL404`| Pool not found              | Check pool ID              | 