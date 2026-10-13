# MAS4TE project

This repository contains concepts and diagrams of the MAS4TE project.

## Overview

Components and responsibilities of the project partners. The schedules are exchanged via the BRP, see the [battery market flow](./seq_diagrams/battery_market.md).

```mermaid
flowchart TB
    subgraph FHAC["FHAC (ASSUME toolbox)"]
        Market["Battery Market<br/>(ASSUME + market-clearing)"]
        MQTT(["MQTT"])
        Supplier["Battery Supplier Agent"]
        Demand["Battery Demand Agent"]
        BRP["BRP<br/>(brp-party)"]
    end
    subgraph FZJ
        FZJInterface["FZJ Interface"]
        FiWare["FZJ FiWare"]
    end
    subgraph OU
        LLM["LLM"]
        UserInput["User Input"]
        BUC["Battery Utility Calculator (BUC)"]
    end
    GridSim["Grid simulation<br/>(planned, after trading)"]
    Blockchain["Blockchain"]
    ClimateCities["Climate Cities"]
    BoostingAlpha["BoostingAlpha"]

    Market <-->|allocation| Supplier
    Market <-->|allocation| Demand
    Market -->|market results| BRP
    Demand -->|schedule| BRP
    BRP -->|schedule| Supplier
    Market -.->|blockchain addresses| Blockchain
    Supplier <--> FZJInterface
    FZJInterface --> FiWare
    Supplier <--> LLM
    Demand <--> LLM
    LLM <--> UserInput
    LLM <--> BUC
    BRP -.->|schedules| GridSim
```

## Schedule

| Workshop | Content |
|----------|---------|
| [1. July](./2025-07-01-workshop/README.md)  | Concepts & Programming  |

## Sequence Diagrams
[The battery market sequence diagram](./seq_diagrams/battery_market.md) shows the market process and the schedule handling via the BRP. The interface between ASSUME and the AI agents is described [here](./ai_agent.md).


## Product definitions
[See here](./virtual_products.md)

## Software stack and next steps
Green: already implemented, orange: next steps, blue: for a later part. ASSUME and the agents communicate via MQTT, see [AI agent](./ai_agent.md).

```mermaid
flowchart LR
    subgraph Legend
        direction LR
        LDone["already implemented"]:::done
        LNext["next steps"]:::next
        LLater["for later part"]:::later
    end

    ASSUME["ASSUME Market"]:::done
    ADB[("ASSUME DB")]:::optional
    BC["Blockchain transactions"]:::next
    BRP["BRP<br/>(brp-party)"]:::done
    CA["Communications agent"]:::done
    DB[("Comm-agents database<br/>demand forecasts, price forecasts, bids")]
    BUC["Battery Utility Calculator (BUC)"]:::done
    BAT["Batteries model"]
    PF["Price forecaster"]:::later
    GS["Grid simulation"]:::later
    U["User"]

    ASSUME -->|"market_open, market results (MQTT)"| CA
    CA -->|"bids (MQTT)"| ASSUME
    ASSUME -->|"market results (HTTP)"| BRP
    ASSUME -.-> ADB
    ADB --> BC
    CA -->|communication bids| DB
    DB -->|market results| CA
    CA -->|"forecasts (prices, solar generation, demand, ...)"| BUC
    BUC -->|"bidding curve, storage charge + discharge time series"| CA
    CA -->|"buyer: schedule"| BRP
    BRP -->|"seller: schedules to fulfil"| CA
    CA -->|sends schedule| BAT
    BAT -->|"simulation results, SOC, ..."| CA
    PF -->|price forecasts| DB
    DB -->|"historic market results / price signals<br/>(from external, weather forecast, ...)"| PF
    CA -->|"explain bid creation, market process"| U
    U -->|"changes in behaviour (vacation, ...)"| CA
    BRP -->|"schedules (after trading)"| GS

    classDef done stroke:#2e7d32,stroke-width:2px
    classDef next stroke:#ef6c00,stroke-width:2px
    classDef later stroke:#1565c0,stroke-width:2px
    classDef optional stroke-dasharray:5 5
    linkStyle 0,1,2 stroke:#2e7d32,stroke-width:2px
    linkStyle 4,5,6,7,8,9,10 stroke:#ef6c00,stroke-width:2px
    linkStyle 13,14,15,16,17 stroke:#1565c0,stroke-width:2px
```

The state of the agent side (comm-agents database, BUC and BRP connection of the agents) is taken from February 2026; the code of the agents is not part of these repositories.

## Tools

- Git/GitHub for Version Control
- [Excalidraw](https://draw.nowum.fh-aachen.de/)
- [Diagrams.net](https://app.diagrams.net/)
- [Mermaid.js](https://mermaid.live/)
