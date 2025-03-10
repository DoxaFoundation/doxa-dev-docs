---
title: "Troubleshooting"
description: "Common issues and their solutions"
---

# Troubleshooting Guide

This section covers common issues and their solutions when developing on Doxa.

## Common Errors

### Cycle Management

#### Error: Insufficient Cycles
**Symptom**: `IC0301` error when deploying or calling canisters

**Solution**:
1. Check cycle balance:
```bash
dfx canister call my_canister get_cycle_balance
```

2. Transfer cycles:
```bash
dfx ledger transfer --amount 0.1 --memo 0 --to <CANISTER_ID>
```

### Authentication

#### Error: Authentication Failed
**Symptom**: `AUTH004` error when trying to authenticate

**Solution**:
1. Clear browser cache
2. Reconnect wallet
3. Check NFID permissions

### Swap Operations

#### Error: Slippage Too High
**Symptom**: `SWAP409` error during swap

**Solution**:
1. Adjust slippage tolerance
2. Check current market rates
3. Try with smaller amount

## Development Environment

### DFX Issues

#### DFX Not Starting
**Symptom**: `dfx start` fails

**Solution**:
1. Kill existing processes:
```bash
pkill dfx
```

2. Clear cache:
```bash
rm -rf .dfx
```

3. Restart DFX:
```bash
dfx start --clean
```

### Canister Deployment

#### Deployment Fails
**Symptom**: `dfx deploy` fails

**Solution**:
1. Check network:
```bash
dfx network use local
```

2. Verify canister ID:
```bash
dfx canister id my_canister
```

3. Check logs:
```bash
dfx canister call my_canister get_logs
```

## Performance Issues

### Slow Response Times

**Symptom**: Canister calls taking too long

**Solution**:
1. Check cycle balance
2. Monitor network status
3. Optimize code:
```motoko
// Before
for (i in Iter.range(0, 1000)) {
    // expensive operation
};

// After
let batch_size = 100;
for (i in Iter.range(0, 1000, batch_size)) {
    // process in batches
};
```

## Debugging Tips

### Logging

1. Add debug prints:
```motoko
import Debug "mo:base/Debug";

Debug.print("Debug: " # debug_show(value));
```

2. View logs:
```bash
dfx canister call my_canister get_logs
```

### State Inspection

1. Check canister state:
```motoko
public query func get_state() : async Text {
    debug_show(state)
};
```

2. View in Candid UI:
```bash
dfx canister --network local call my_canister get_state
```

## Getting Help

If you're still experiencing issues:

1. Check our [GitHub Issues](https://github.com/doxa-fi/core/issues)
2. Join our [Discord](https://discord.gg/doxa)
3. Contact support: support@doxa.org 