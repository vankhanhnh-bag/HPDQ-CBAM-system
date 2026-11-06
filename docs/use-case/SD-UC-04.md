# UC-04: Calculate shipment-level PCF

| Field | Value |
|---|---|
| Use case | UC-04 Calculate shipment-level PCF |
| Version | v1 |
| FRs | FR-01-05 (core calculation). Supporting data: FR-01-03 (batch carbon intensity), FR-01-07 (traceability linkage) |

## Diagram
```mermaid
sequenceDiagram
    autonumber
    actor User as Export Sales & Marketing
    participant UI as Shipment PCF UI
    participant Svc as PCF service
    participant CDT as Carbon data / traceability service
    participant Records as PCF Records
    participant SAP as SAP S/4HANA
    participant LOG as Hoa Phat Logistics

    User->>UI: Select shipment
    UI->>Svc: Request shipment PCF
    Svc->>SAP: Retrieve shipment/order context
    SAP-->>Svc: Shipment + product/batch identifiers
    Svc->>CDT: Retrieve constituent-batch linkage
    CDT->>Records: Retrieve batch carbon intensities
    Records-->>CDT: Batch carbon intensities
    CDT-->>Svc: Constituent-batch linkage + batch carbon intensities
    Svc->>LOG: Retrieve logistics emissions
    LOG-->>Svc: Logistics emissions data
    Svc->>Svc: Aggregate batch intensity + logistics
    Note over Svc: FR-01-05 (Must)
    Svc->>Records: Store shipment-level PCF
    Svc-->>UI: PCF result
    UI-->>User: Display shipment-level PCF
```
