# AI agents

<img width="1920" height="1080" alt="AI_Agents_1" src="https://github.com/user-attachments/assets/6bfeb480-293b-4208-a71d-14b452e16193" />

https://www.canva.com/design/DAGyptuNCTg/5m9gBuTWbCBhh3NP3jGk9A/edit?utm_content=DAGyptuNCTg&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton

## Interface to ASSUME

The AI agents (communications agents) run outside of ASSUME. ASSUME does not call an LLM
itself: the bidding strategy `LLMStrategy` (`mas4te/mas4te_bidding_strategy.py` in
[MAS4TE/assume](https://github.com/MAS4TE/assume), branch `mas4te`) only passes messages
between the market and the agent via MQTT (broker on `localhost:1883`).

| Topic | Direction | Content |
|-------|-----------|---------|
| `mas4te/market/status_agent<unit_id>` | ASSUME → agent | `market_open` with market id, limits, units, `additional_fields`, market products and the products to bid on (`start_time`, `end_time`) |
| `mas4te/bids/agent<unit_id>` | agent → ASSUME | one bid or a list of bids |
| `mas4te/results/agent<unit_id>` | ASSUME → agent | acknowledgement `bids_received`, after the clearing the `market result` with the orderbook of the unit |

`<unit_id>` is the id of the unit in ASSUME, e.g. `B_01` (buyer) or `S_01` (seller).

Bid format:

```json
[{"bid_id": 1, "price": 0.09, "quantity": 12.0, "blockchain_address": "0x..."}]
```

- `quantity` is always positive, ASSUME sets the sign by the role of the unit (`volume` is accepted as fallback).
- Bids without `blockchain_address` are skipped.
- ASSUME waits up to 10 minutes for the bids; without bids the unit does not take part in that auction.

In the market result, every accepted order carries `matches`, the list of counterparties
(`bid_id`, `blockchain_address`, `volume`, `price`). The agents need it for the schedule
handling with the BRP, see the [battery market flow](./seq_diagrams/battery_market.md).

## History (state of September 2025)

Before the MQTT interface, the LLM was called from within ASSUME. This code is no longer part
of the ASSUME fork; the points below describe the approach of that time.

- The class `LLMModelAPI` loaded the model (Mistral API with credits, or locally via LM Studio) and ran the prompts of the LLM agent.
- The LLM agent got a prompt and the output of the [Battery Utility Calculator (BUC)](https://github.com/NOWUM/battery-utility-calculator), back then called optimization framework, which calculated the monetary worth of storage volumes for the unit from forecasts of price, demand and solar generation.
- The LLM chose price and volume from it and answered with a JSON object with `volume`, `price` and `reasoning`; a default value was used if the output was invalid.
- Known issue: the LLM interpreted the worth of the storage as profit instead of costs.

Ideas noted at that time:

- Incorporate tools (toolbox) to combine all input signals into one larger system that can reason and act.
- Incorporate a short history of the market, to see how the AI agent reasons about it.
