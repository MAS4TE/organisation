This diagram is only for the schedule of the energy only market (EOM). For the battery market process please [see here](./seq_diagrams/battery_market.md), for the scheduling process of the battery market please [see here](./seq_diagrams/battery_schedule_seq.md) and for the balancing process please [see here](./seq_diagrams/balancing.md).

```mermaid
sequenceDiagram
    Participant CA as Consumer Agent
    Participant CS as Consumer Supplier / <br> Balancing Operator
    Participant EOMO as local Energy only Market <br> Operator
    Participant PA as Provider Agent / FZJ
    Participant PS as Provider Supplier / <br> Balancing Operator
    Participant FZJ as FZJ FiWare

    CA->>EOMO: Send local EOM schedule
    EOMO->>CS: Send local EOM schedule
    EOMO->>PA: Send local EOM schedule
    EOMO->>PS: Send local EOM schedule

    PA->>FZJ: Send local EOM schedule

    FZJ->>FZJ: Process via FIWARE
```
