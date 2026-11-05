# Sequence Diagram: SD-UC-06 View carbon traceability record (Must-only)   

This sequence models the background traceability-linkage behavior required by FR-01-07. Whenever a supplier, production, or shipment record is created, EcoTrace processes the new record and maintains the applicable relationships connecting supplier/raw-material sources, production batches, and shipped batches.

| Field | Value |
|---|---|
| Use case | UC-06 View carbon traceability record |
| Version | v1 |
| FRs | FR-01-08 |

```mermaid
sequenceDiagram
    autonumber
    participant SUP as Supplier / Procurement Source
    participant PRD as Production Source
    participant SHP as Shipment / Logistics Source
    participant ING as EcoTrace Record Ingestion
    participant LNK as Traceability Linkage Engine
    participant RDB as Operational Records Store
    participant TGB as Traceability Graph Store

    alt Supplier record is created
        SUP->>ING: SupplierRecordCreated(recordId, materialLotRefs)
    else Production record is created
        PRD->>ING: ProductionRecordCreated(recordId, batchId, consumedMaterialRefs)
    else Shipment record is created
        SHP->>ING: ShipmentRecordCreated(recordId, shipmentId, shippedBatchRefs)
    end

    ING->>LNK: ProcessCreatedRecord(recordType, recordId, lineageRefs)
    LNK->>RDB: Retrieve new record and referenced lineage records
    RDB-->>LNK: Record details and available lineage references
    LNK->>LNK: Resolve applicable upstream raw-material sources
    LNK->>LNK: Resolve applicable downstream shipped batches
    LNK->>LNK: Build/update supplier-to-material-to-production-to-shipment relationships
    LNK->>TGB: Upsert traceability nodes and relationship edges
    TGB-->>LNK: Traceability linkage persisted
    LNK-->>ING: Linkage processing completed
```

