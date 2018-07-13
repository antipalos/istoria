# Cardano UTxO snapshot

Snapshot of all addresses with a balance (at least one transaction) 

## Files explained

- `utxo_<snapshot_date>.json`

## Schema

```
{
  "date": "blockchain date of the snapshot (string)",
  "epoch": "what epoch last accounted block was from (int)",
  "slot": "what slot (whithin epoch) last accounted block was from (int)",
  "count": "total count of used addresses",
  "balance": "total sum of positive address balances",
  "utxo": [
    [
        "<address>",
        {
          "inpTxNum": "number of input transactions (int)",
          "outTxNum": "number of output transactions (int)",
          "balance": "address balance in lovelaces (int)"
        }
    ]
  ]
}
```

## Comments

- All addresses are sorted by the descending balance
- Some addresses have negative balance, those are "genesis redemption addresses"
 and they have form of `Ae2tdPwUPEZKQuZh2UndEoTKEakMYHGNjJVYmNZgJk2qqgHouxDsA5oT83n`