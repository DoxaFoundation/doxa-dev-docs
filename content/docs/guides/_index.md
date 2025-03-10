---
title: "Guides"
description: "Step-by-step guides for Doxa development"
---

# Development Guides

This section contains detailed guides for common development tasks.

## Canister Development

### Creating a New Canister

1. Create a new Motoko file:

```motoko
actor {
    private var state = 0;
    
    public func increment() : async Nat {
        state += 1;
        state
    };
}
```

2. Add to `dfx.json`:

```json
{
  "canisters": {
    "my_canister": {
      "main": "src/my_canister.mo",
      "type": "motoko"
    }
  }
}
```

3. Deploy:

```bash
dfx deploy my_canister
```

### Managing Cycles

1. Check balance:

```bash
dfx canister call my_canister get_cycle_balance
```

2. Transfer cycles:

```bash
dfx ledger transfer --amount 0.1 --memo 0 --to <CANISTER_ID>
```

## Authentication

### Implementing NFID Auth

1. Add dependencies:

```bash
npm install @dfinity/agent @dfinity/auth-client
```

2. Initialize auth:

```typescript
import { AuthClient } from '@dfinity/auth-client';

const client = await AuthClient.create();
const identity = client.getIdentity();
```

3. Make authenticated calls:

```typescript
const agent = new HttpAgent({
    identity,
    host: 'https://ic0.app'
});
```

## Error Handling

### Best Practices

1. Use Result type:

```motoko
type Result<T, E> = variant {
    Ok: T;
    Err: E;
};

public func handle_error(err : SwapError) : Text {
    switch(err) {
        case (#InsufficientLiquidity) "Add more tokens to pool";
        case (#ExpiredTransaction) "Retry with updated rate";
        case (#AuthFailed) "Reconnect wallet";
    }
};
```

2. Log errors:

```motoko
Debug.print("Error: " # debug_show(err));
```

## Testing

### Unit Tests

1. Create test file:

```motoko
import Debug "mo:base/Debug";
import Principal "mo:base/Principal";

actor {
    public func test_create_pool() : async Bool {
        let result = await create_pool(
            Principal.fromText("xeka7-7iaaa-..."),
            Principal.fromText("ryjl3-tyaaa-...")
        );
        switch(result) {
            case (#Ok(_)) true;
            case (#Err(_)) false;
        }
    };
}
```

2. Run tests:

```bash
dfx test
```

## Deployment

### Mainnet Deployment

1. Set network:

```bash
dfx network use ic
```

2. Deploy:

```bash
dfx deploy --network ic --wallet $(dfx identity get-wallet)
```

3. Verify:

```bash
dfx canister --network ic status <CANISTER_ID>
``` 