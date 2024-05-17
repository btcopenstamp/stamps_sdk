# SRC-101 Standard

`SRC-101` is a unique non-fungible token standard designed for the Bitcoin Name
Service, based on BTC Stamps. It is inherited from [SRC-20](./src20specs.md) and
maintains compatibility with it. In SRC-101, tokens are bound to owner accounts,
preventing them from being mistakenly spent with UTXOs, which safeguards the
assets from loss. Additionally, SRC-101 enables multiple transfer operations
within a single transaction, significantly reducing network fees.

# Specifications

A normal SRC-101 transaction must conform to these fields. Otherwise a Bitcoin
Stamp Number will not be created, the transaction will not be considered a valid
SRC-101 transaction and they will not appear in the Bitcoin Stamps Protocol
index / API.

### DEPLOY

```JSON
{
  "p": "src-101", //(string)protocol standard name for bitname service
  "op": "deploy", //(string)function name
  "name": "Bit Name Service", //(string)collection name
  "max": "0", //(uint64)0 means unlimited
  "lim": "10", //(uint64)limit 10 mint op in each transaction, if there are more than 10 mint op in 1 transaction, only the first 10 will be handled.
  "dec": "0", //(uint8)indivisible NFT
  "owner": "bc1q34eaj4rz9yxupzxwza2epvt3qv2nvcc0ujqqpl", //(string)owner address
  "rec": "bc1q7rwd4cgdvcmrxm27xfy6504jwkllge3dda04ww", //(string)recipient address to receive mint fees
  "tick": "BNS", //(string)
  "pri": "30769", //(uint64)price in sats, must pay to "rec"
  "desc": "Bitname Service powered by BTC stamp.", //(string)description for the collection.
  "mintstart": "1706866958", // Unix timestamps in Milliseconds. Mint is available from this time.
  "mintend": "18446744073709551615", // Maximum Unix timestamps
  "wlf": true, //(bool)whitelist flag
  "wll": "https://xxxxx",
  "disc": "50" //(uint8) 50% discount for whitelist address
}
```

The `DEPLOY` transaction signer must be the same as "owner", otherwise it will
not be considered as a valid SRC-101 transaction.

### MINT

```JSON
{
  "p": "src-101", //(string)protocol standard name
  "op": "mint", //(string)function name
  "hash": "0x38091b803f794e50dcc10a9091becaf4f65d35d3ef9e71cfa90c7936af50757e", //(hash256)txid of bns deploy transaction
  "toaddress": "bc1q7rwd4cgdvcmrxm27xfy6504jwkllge3dda04ww", // recipient address of this mint, can be different from signer address.
  "tokenid": "7375706572626f79", //(string)UTF8-> hexstring: superboy->7375706572626f79.
  "dua": "1" //(uint8)years of duration. Expire date = current + dua
}
```

### TRANSFER

```JSON
{
  "p": "src-101", //(string)protocol standard name
  "op": "transfer", //(string)function name
  "hash": "0x38091b803f794e50dcc10a9091becaf4f65d35d3ef9e71cfa90c7936af50757e", //(hash256)txid of the deploy transaction, only this txid will be considered as valid in bitname service.
  "toaddress": "bc1q7rwd4cgdvcmrxm27xfy6504jwkllge3dda04ww", // new owner address of this token..Support any existed type of bitcoin addresses
  "tokenid": "7375706572626f79" //
}
```

If the bitname NFT specified to be transferred not in transaction sender's
address (which would be determined by the latest state of an Indexer), then the
transfer will be deemed invalid.

### SETRECORD

```JSON
{
  "p": "src-101", //(string)protocol standard name
  "op": "setrecord", //(string)function name
  "hash": "0x38091b803f794e50dcc10a9091becaf4f65d35d3ef9e71cfa90c7936af50757e", //(hash256)txid of the deploy transaction
  "type": "address", //(string)Currently two kinds of record types are supported, txt and address
  "data": "bc1q7epcly9u55yut5k7ykmlcyrp87knt8gxd7knnt" //(string)record data
}
```

The `SETRECORD` transaction signer must be the same as "owner", otherwise it
will not be considered as a valid SRC-101 transaction. Multi record could exist
for different addresses. If the record for setting is existed, it will be
overwrote.

### RENEW

```JSON
{
  "p": "src-101", //(string)protocol standard for non-fungible token
  "op": "renew", //(string)function name
  "hash": "0x38091b803f794e50dcc10a9091becaf4f65d35d3ef9e71cfa90c7936af50757e", //(hash256)txid of the deploy transaction
  "address": "bc1q7epcly9u55yut5k7ykmlcyrp87knt8gxd7knnt", //(string)owner address
  "tokenid": "7375706572626f79", //(string)UTF8-> hexstring
  "dua": "2" //(uint8)years of duration. Expire date = current + dua
}
```

### TRANSFEROWNERSHIP

```JSON
{
  "p": "src-101", //(string)protocol standard for non-fungible token
  "op": "transferownership", //(string)function name
  "hash": "0x38091b803f794e50dcc10a9091becaf4f65d35d3ef9e71cfa90c7936af50757e", //(hash256)txid of the deploy transaction
  "newowner": "bc1qag3cemd7988sgtx2huscdf6qmvgexnsx393ayc" //(string)new owner address.Support any existed type of bitcoin addresses
}
```

This allows SRC-101 admin transferring ownership to another. The
`TRANSFEROWNERSHIP` transaction signer must be the same as "owner", otherwise it
will not be considered as a valid SRC-101 transaction.