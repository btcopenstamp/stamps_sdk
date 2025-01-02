## SRC20 UTXO Model Specification (Draft)

Currently, SRC20 uses an account model. To enable Atomic Swaps, a UTXO model will be introduced for SRC20 accounts, while the account model will be retained. Adding the UTXO model will introduce the following new SRC20 operations:

### New Operations

#### Attach

Attaches the amount in the account model to the first non-OP_RETURN UTXO. If no non-OP_RETURN output is available, the asset will remain in the original account, and the transaction will be invalid.

```xml
{
  "p": "src-20",
  "op": "attach",
  "tick": "STAMP",
  "amt": "100"
}
```

#### Detach 

Detaches all assets from inputs, transferring them to the account model associated with the first non-OP_RETURN output.
If no valid output exists, the assets are either destroyed or returned to the originating accounts (TBD).

```xml
{
  "p": "src-20",
  "op": "detach"
}
```

#### UTXO Transfer 

Transfers all assets attached to all inputs to the first non-OP_RETURN output. If no non-OP_RETURN output exists, the assets are either destroyed or returned to the originating accounts(TBD).
### Existing Operations

#### Deploy

```xml
{
  "p": "src-20",
  "op": "deploy",
  "tick": "STAMP",
  "max": "100000",
  "lim": "100",
  "dec": "18" // [optional]
}
```

#### Mint

```xml
{
  "p": "src-20",
  "op": "mint",
  "tick": "STAMP",
  "amt": "100"
}
```

Tokens minted will be stored in the account model.

#### Transfer - Account Model

```xml
{
  "p": "src-20",
  "op": "transfer",
  "tick": "STAMP",
  "amt": "100"
}
```

### Notes
1. Indexer prioritizes P2WSH-encoded operations, followed by UTXO asset transfers.
