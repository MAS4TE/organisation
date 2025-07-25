This diagram is only for the schedule of batteries. For the battery market process please [see here](./battery_market.md).
```mermaid
sequenceDiagram
    Participant CA as Consumer Agent
    Participant CS as Consumer Supplier / <br> Balancing Operator
    Participant BatMO as Battery Market <br> Operator
    Participant PA as Provider Agent / FZJ
    Participant PS as Provider Supplier / <br> Balancing Operator
    Participant FZJ as FZJ FiWare

    CA->>BatMO: Send battery schedule
    BatMO->>CS: Send battery schedule
    BatMO->>PA: Send battery schedule
    BatMO->>PS: Send battery schedule

    PA->>FZJ: Send battery schedule

    FZJ->>FZJ: Process via FIWARE
```
