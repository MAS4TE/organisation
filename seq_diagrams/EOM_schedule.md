> **Status: planned, not implemented.** The local EOM (`CommunityEOM`) is commented out in
> `mas4te/simulation.py` of [MAS4TE/assume](https://github.com/MAS4TE/assume). The agents value
> the energy with the [Battery Utility Calculator (BUC)](https://github.com/NOWUM/battery-utility-calculator).

This diagram is only for the schedule of the energy only market (EOM). For the battery market process and its schedules please [see here](./battery_market.md).

```mermaid
sequenceDiagram
    participant ProviderAgent as Provider Agent
    participant ProviderSupplier as Provider Supplier
    participant ConsumerAgent as Consumer Agent
    participant ConsumerSupplier as Consumer Supplier
    participant BUC as Battery Utility Calculator (BUC)
    participant Market as Local Energy only Market
    participant Blockchain

    ConsumerAgent->>BUC: Query energy worth for 24 hours (demand side)
    BUC-->>ConsumerAgent: Return energy worth for 24 hours


    ConsumerAgent->>Market: Submit demand bid

    ProviderAgent->>BUC: Query energy worth for 24 hours (supply side)
    BUC-->>ProviderAgent: Return energy worth for 24 hours

    ProviderAgent->>Market: Submit supply bid

    Market->>Blockchain: Store bids and clearing result
    Market-->>ProviderAgent: Return clearing result
    Market-->>ConsumerAgent: Return clearing result

    Market->>ConsumerSupplier: Send accepted bids
    Market->>ProviderSupplier: Send accepted bids
```
