# Battery market flow

Market process and schedule handling of the battery market. For the (not yet implemented) energy only market see [here](./EOM_schedule.md).

Components:

- **ASSUME** ([MAS4TE/assume](https://github.com/MAS4TE/assume), branch `mas4te`): runs the weekly battery market. The clearing is [MAS4TE/market-clearing](https://github.com/MAS4TE/market-clearing).
- **BRP** ([MAS4TE/brp-party](https://github.com/MAS4TE/brp-party)): stores the market results, assigns the buyers' schedules to the matched sellers and validates the driven schedules.
- **AI agents** (buyer and seller): create the bids and talk to ASSUME via MQTT, see [AI agent](../ai_agent.md).

The blockchain address comes from the AI agent, the market in ASSUME reports the awarded
orders to the BRP, the BRP assigns the buyers' schedules to the matched sellers, the seller
(storage provider) polls its schedules from the BRP via HTTP.

```mermaid
sequenceDiagram
    autonumber
    participant BU as Buyer<br/>(AI agent)
    participant AS as ASSUME<br/>(battery market)
    participant BRP as BRP<br/>(brp-party)
    participant SE as Seller / storage provider<br/>(AI agent + battery)

    Note over AS: Market opening (weekly, product = 1 week of storage)
    AS->>BU: [MQTT] market_open (products, limits)
    AS->>SE: [MQTT] market_open (products, limits)
    BU->>AS: [MQTT] bids [{bid_id, price, quantity, blockchain_address}]
    SE->>AS: [MQTT] bids [{bid_id, price, quantity, blockchain_address}]

    Note over AS: Market closing: clearing per location,<br/>uniform price, matching of counterparties
    AS->>BRP: [HTTP] POST /market_results<br/>accepted orders + counterparties + addresses
    BRP-->>AS: [HTTP] 200 {market_id, stored_orders}
    AS->>BU: [MQTT] market result: accepted orders with matches<br/>(counterparty bid_id, address, volume, price)
    AS->>SE: [MQTT] market result: accepted orders with matches<br/>(counterparty bid_id, address, volume, price)

    BU->>BRP: [HTTP] POST /power_request<br/>{blockchain_address, time_steps, series} (15-min steps)
    BRP->>BRP: validate, aggregate, assign to the matched sellers<br/>(share by matched volume from market_results), store
    BRP-->>BU: [HTTP] 200 {request_id}, or 422 if no awarded storage
    loop every ~30 s
        SE->>BRP: [HTTP] GET /power_request?blockchain_address=...
        BRP-->>SE: [HTTP] 200 open schedules (own share)<br/>[{request_id, time_steps, power_rate_w}]
    end
    SE->>BRP: [HTTP] POST /actual_schedule<br/>{request_id, blockchain_address, time_steps, power_rate_w}
    BRP->>BRP: compare with own share of the request, missing_energy_wh, store
    BRP-->>SE: [HTTP] 200 {valid, errors, missing_energy_wh}

    SE->>SE: [MQTT] own-use schedule to its battery
```

## Authentication

- `POST /market_results`: HTTP Basic Auth, username `assume`, password `MARKET_RESULTS_PASSWORD` (only ASSUME).
- `POST /power_request`, `GET /power_request`, `POST /actual_schedule`: HTTP Basic Auth with `POWER_REQUEST_PASSWORD` (all agents).

## Not implemented yet

- The communications agent does not evaluate `matches` yet and does not send a
  `blockchain_address` with its bids (ASSUME skips bids without one).
- The `blockchain_address` in the BRP requests is not verified (no signature), so anyone
  with the `POWER_REQUEST_PASSWORD` can act as any buyer or seller.
- Bids and clearing results are not stored on a blockchain.
