# Cardano Explorer Stats

Statistics about transactions and utxo, collected by full explorer scan.

## Files explained

1. `cardano_explorer_latest_stats_*.json` - short file with only latest stat snapshot

1. `cardano_explorer_historical_stats_*.json` - long file containing historical snapshots
(latest, each year, each month, each epoch, each day)

## Schema

```
{
  "calculated_at": "timestamp of the total stats snapshot creation (string)",
  "last_date": "last blockchain date taken into account in the snapshot (string)",
  "last_page": "last explorer API page used in the snapshot (int)",
  "last_epoch": "what epoch last accounted block was from (int)",
  "last_slot": "what slot (whithin epoch) last accounted block was from (int)",
  "stats": {
    "<period_type>": [
      [
        "<period_key>",
        {
          "is_open": "flag, indicating that this period is still open and may change in later snapshots (bool)",
          "txs": "number of total transactions during this period (int)",
          "txsTotalSize": "total size of all transactions in that period in bytes (int)",
          "feesTotal": "total amount of fees during that period in lovelaces (int)",
          "txsAvgSize": "average transaction size during this period in bytes (int)",
          "feesAvg": "average fee per transaction in lovelaces (int)",
          "utxo_distribution": {
            "count": "total unique addresses at the end of the period or at the time of the snapshot (int)",
            "balance": "total positive balance on all addresses in lovelaces (int)",
            "logs": [
              {
                "log": "log10(balance) of addresses - what max power of 10 is still less than the balance in lovelaces (int)",
                "num": "number of addresses with this power of balances (int)",
                "share": "share of the number of addresses among all addresses (float.2)"
              }
            ],
            "tops": [
              {
                "num": "number of top richest addresses (int)",
                "coins": "total balance of these top addresses in lovelaces (int)",
                "share": "share of this addresses' common balance among the total balance of all addresses (float.2)"
              }
            ]
          }
        }
      ]
    ]
  }
}
```

#### Where

1. `<period_type>` is one of: `['total', 'year', 'month', 'epoch', 'day']`
2. `<period_key>`:
    1. For type `total` - single value is always `total`
    2. For type `year` - value is the first day of the year date like `"2018-01-01"`
    3. For type `month` - value is the first day of the month date like `"2018-07-01"`
    4. For type `epoch` - value is the number of the epoch like `58`
    5. For type `day` - value is the date like `"2018-07-13"`
    
## "Logs" distribution explained

"Logs" means "logarithmic".

- Path: `stats.<period_type>.<period_key>.utxo_distribution.logs`
- Type: collection of objects
- Example of an object:

```
{
  "log": 7,
  "num": 19928,
  "share": 7.69
}
```

- `log` - logarithm base 10 from the balance in lovelaces
(in this case it means all balances between 10 and 100 ADA)
- `num` - number of such balances
- `share` - percentage of such balances among all other balances

### Logarithms are in lovelaces!

For example, `log=0` means all balances between 0 and 10 **lovelaces**.
`log=1` means all balances between 10 and 100 **lovelaces**.
`log=6` means all balances between 1 and 10 **ADA**.
`log=15` mean all balances between 1 and 10 **billion ADA**  

## "Tops" distribution explained

Distribution of coins among top N accounts.

- Path: `stats.<period_type>.<period_key>.utxo_distribution.tops`
- Type: collection of objects
- Example of an object:

```
{
  "num": 1000,
  "coins": 18042051191672586,
  "share": 59.16
}
```

- `num` - number of top addresses considered
- `coins` - total balance of those addresses in lovelaces
- `share` - percentage of this balance among the total balance of all addresses