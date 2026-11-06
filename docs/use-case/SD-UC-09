```mermaid
sequenceDiagram
    autonumber
    actor Customers as Customers
    participant UI as EcoTrace web UI
    participant BE as EcoTrace BE
    participant PCDB as Product & Carbon DB
    participant DB as EcoTrace Database

    Customers->>UI: 1. Input comparison criteria
    UI->>BE: 2. Submit filters
    
    rect rgb(245, 245, 247)
        Note over UI,DB: EcoTrace internal system
        
        BE->>BE: 3. Validate parameters
        BE->>PCDB: 4. Query product profiles
        PCDB-->>BE: 5. Return product profiles
        PCDB->>DB: 6. Store retrieved data
        DB-->>BE: 7. Stored data
        BE->>BE: 8. Calculate weighted score
        BE->>DB: 9. Store comparison results
        DB-->>BE: 10. Stored results
        BE->>BE: 11. Sort and assign primary/alternative flags
        BE->>DB: 12. Store recommendation
        DB-->>BE: 13. Stored recommendation
        BE-->>UI: 14. Return recommended options
    end

    UI-->>Customers: 15. Display options
