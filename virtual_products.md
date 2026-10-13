# Virtual products
Virtual products are defined by some flexible (e. g. prosumer specific) and some fixed (e. g. given by market) characteristics. Currently there is a single product, the rent of storage capacity for one week.

## Market
* Auction: one auction per week, open for one hour. Only the next week is tradeable.
* Delivery: starts 12 hours after the market closes.
* Clearing: uniform price per location; the accepted bids are matched with counterparties (see the [battery market flow](./seq_diagrams/battery_market.md)).

**Fixed characteristics:**
* Timeframe (e. g. time one can rent a storage for): set to one week
* Efficiency: 95%. Buyer of storage has to account for losses (e. g. only receiving 95% of stored energy back)
* Fill level: Storage will be received AND returned with 50% state of charge.
* C-Rate: Charge- and discharge power in relation to the volume (e. g. volume: 10 kWh + c-rate: 2 == 20 kW, volume: 50 kWh + c-rate: 0.5 == 25 kW). Currently only a c-rate of 1 is allowed.
* Schedule: the buyer sends its schedule for the rented storage to the BRP in 15-minute steps.

**Flexible characteristics:**
* Price: 0.01 €/kWh steps, minimum 0 €/kWh
* Capacity: 0.1 kWh steps

**Required with every bid:**
* Blockchain address of the bidder

## Differences to the implementation
The current implementation ([MAS4TE/assume](https://github.com/MAS4TE/assume), branch `mas4te`, and [MAS4TE/market-clearing](https://github.com/MAS4TE/market-clearing)) differs from the definition above:

* Efficiency and fill level are not checked by the market. In the simulation the buyers' storage has an efficiency of 100%.
* The steps of price and capacity are not enforced.
* The market config uses `kW` as volume unit.
* The clearing already supports several locations and mutually exclusive bids on different locations (fields `location` and `exclusive_id`), which are not used yet.
