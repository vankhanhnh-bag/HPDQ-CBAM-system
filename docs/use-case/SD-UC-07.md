# UC-07: Estimate & Analyze CBAM exposure for shipment

| Field | Value |
|---|---|
| Use case | UC-07 Estimate & Analyze CBAM exposure for shipment |
| Version | v1 |
| FRs | FRs	FR-02-01, FR-02-02, FR-02-03, FR-02-04, FR-02-05, FR-02-06, FR-02-07, FR-02-08, FR-02-09 |

## Diagram
```mermaid
sequenceDiagram
    autonumber
    actor User as Export Sales / Procurement
    participant UI as CBAM estimation UI
    participant Svc as CBAM service
    participant Engine as Compliance calculation engine
    participant Records as Compliance records
    participant SAP as SAP S/4HANA
    participant ETS as EU ETS market data

    User->>UI: Select shipment & request CBAM estimate
    UI->>Svc: Request CBAM estimate
    Svc->>SAP: Retrieve shipment & product data
    SAP-->>Svc: Shipment, product, weight, destination, data
    Svc->>Engine: Validate eligibility & inputs

    alt Missing / Invalid data
        Svc-->>UI: Return validation error
        UI-->>User: Display validation error message
    else Valid data
        Svc->>Engine: Get verified embedded emissions
        Engine->>Engine: Verified embedded emissions
        Engine->>Engine: Determine exempt emissions
        Engine->>Engine: Emissions subject to CBAM
        Engine->>ETS: Retrieve EU ETS certificate price
        ETS-->>Engine: Applicable certificate price (€/ton CO2)
        Engine->>Engine: Recognize origin carbon-price deduction
        Engine->>Engine: Calculate estimated CBAM cost
        Engine->>Engine: Generate breakdown & assumptions
        Engine->>Records: Store estimate & details
        Engine->>Svc: CBAM cost + breakdown
        Svc-->>UI: Completed estimate
        UI-->>User: Display CBAM estimate and breakdown
    end
```
