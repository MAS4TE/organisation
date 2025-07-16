This diagram is only for the schedule of the energy only market (EOM). For the battery market process please [see here](./seq_diagrams/battery_market.md), for the scheduling process of the battery market please [see here](./seq_diagrams/battery_schedule_seq.md) and for the balancing process please [see here](./seq_diagrams/balancing.md).

```mermaid
sequenceDiagram
    participant ProviderAgent as Provider Agent
    participant ProviderSupplier as Provider Supplier
    participant ConsumerAgent as Consumer Agent
    participant ConsumerSupplier as Consumer Supplier
    participant PricingFW as Pricing Framework
    participant Market as Local Energy only Market
    participant Blockchain

    ConsumerAgent->>PricingFW: Query energy worth for 24 hours (demand side)
    PricingFW-->>ConsumerAgent: Return energy worth for 24 hours


    ConsumerAgent->>Market: Submit demand bid

    ProviderAgent->>PricingFW: Query energy worth for 24 hours (supply side)
    PricingFW-->>ProviderAgent: Return energy worth for 24 hours

    ProviderAgent->>Market: Submit supply bid

    Market->>Blockchain: Store bids and clearing result
    Market-->>ProviderAgent: Return clearing result
    Market-->>ConsumerAgent: Return clearing result

    Market->>ConsumerSupplier: Send accepted bids
    Market->>ProviderSupplier: Send accepted bids
```
