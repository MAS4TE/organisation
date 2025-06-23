```mermaid
sequenceDiagram
    participant ProviderAgent as Provider Agent
    participant ConsumerAgent as Consumer Agent
    participant LLM_Provider as LLM (Provider Side)
    participant LLM_Consumer as LLM (Consumer Side)
    participant OptFramework as Optimization Framework
    participant Market as Battery Market
    participant Blockchain
    participant FZJ as FZJ FiWare

    LLM_Provider->>OptFramework: Query price suggestion (supply side)
    OptFramework-->>LLM_Provider: Return price suggestion

    LLM_Consumer->>OptFramework: Query price suggestion (demand side)
    OptFramework-->>LLM_Consumer: Return price suggestion

    ProviderAgent->>LLM_Provider: Request bid recommendation
    LLM_Provider-->>ProviderAgent: Provide bid recommendation

    ConsumerAgent->>LLM_Consumer: Request bid recommendation
    LLM_Consumer-->>ConsumerAgent: Provide bid recommendation

    ProviderAgent->>Market: Submit supply bid
    ConsumerAgent->>Market: Submit demand bid

    Market->>Blockchain: Store bids and clearing result
    Market-->>ProviderAgent: Return clearing result
    Market-->>ConsumerAgent: Return clearing result

    ConsumerAgent->>ProviderAgent: Coordinate schedule / storage usage

    ProviderAgent->>FZJ: Send operational data
    FZJ-->>FZJ: Process via FIWARE
    
    linkStyle 3 stroke:#ff3,stroke-width:4px,color:red;
```