```mermaid
sequenceDiagram
    autonumber
    actor User as Sustainability & ESG user
    participant UI as EcoTrace web UI
    participant BE as EcoTrace BE
    participant LIB as GHG methodology & emission factor library
    participant LOG as Audit log

    User->>UI: 1. Enter methodology version, factor value, unit, effective date (FR-01-02-FE)
    UI->>BE: 2. Submit configuration details
    
    rect rgb(245, 245, 247)
        Note over BE,LOG: EcoTrace internal system processing
        
        BE->>BE: 3. Validate parameters (FR-01-02-BE)
        
        alt Invalid Parameters
            BE-->>UI: 4a. Return validation errors
            UI-->>User: 5a. Show validation errors
        else Valid Parameters
            BE->>LIB: 4b. Check if factor version already exists
            LIB-->>BE: 5b. Return existence status
            
            alt Existing Factor / New Version
                BE-->>UI: 6. Request Change Justification
                UI-->>User: 7. Prompt for justification
                User->>UI: 8. Provide Change Justification
                UI->>BE: 9. Submit justification
            end
            
            BE->>BE: 10. Process & store record (Set Created/Updated By, At & Justification) (FR-01-02-BE)
            BE->>LIB: 11. Create new version / update library & keep previous version (FR-01-02-BE)
            LIB-->>BE: 12. Confirm storage success
            
            BE->>LOG: 13. Write audit log entry (Who, when, old to new, reason) (FR-01-02-BE)
            LOG-->>BE: 14. Confirm audit logged
            
            BE-->>UI: 15. Return saved confirmation (FR-01-02-FE)
        end
    end

    UI-->>User: 16. Show saved confirmation
