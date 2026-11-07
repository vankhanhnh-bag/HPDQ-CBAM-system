# CVCA: EcoTrace

> HPDQ Carbon Management & Reporting System (EcoTrace) — DBIZ II.

The Customer Value Chain Analysis (CVCA) describes the stakeholders around EcoTrace and the value exchanged between them, including information, approvals, services, and assurance. It is a business-level view used to identify problems and opportunities before defining detailed system behavior.

| Field | Value |
|---|---|
| Project | HPDQ Carbon Management & Reporting System (EcoTrace) |
| Version | v2 |
| Source | DBIZ I CVCA |
| Next | [Context Diagram](docs/context/context_diagram.md) |

## 1. Value chain (with the new system)

```mermaid
flowchart LR
  subgraph CUSTOMER["CUSTOMER — HÒA PHÁT DUNG QUẤT"]
    direction TB
    EX["HPDQ Executive<br/>Board"]
    ESG["Sustainability &<br/>ESG Department"]
    TECH["Technology & Digital<br/>Transformation Dept"]
    PROD["Production Divisions<br/>(Iron - Steel - Rolling)"]
    FIN["Finance &<br/>Accounting"]
    SALES["Export Sales &<br/>Marketing"]
    PROC["Procurement &<br/>Raw Materials"]
  end

  SYS["OUR<br/>SYSTEM"]

  subgraph PARTNERS["PARTNERS"]
    direction TB
    BSI["BSI<br/>(Verification / Assurance)"]
    VINIT["VinIT<br/>(Data & AI Platform)"]
    VCB["Vietcombank<br/>(Green Finance)"]
    LOG["Hoa Phat Logistics<br/>(Consignment Data)"]
  end

  EU["EU Importers<br/>(under CBAM)"]
  DOM["Domestic End-users<br/>(Construction Devs)"]

  SYS -->|"proposal"| EX
  EX -->|"capital, approval, mandate"| SYS

  ESG -->|"GHG Method, Emission Factors"| SYS
  SYS -->|"Verified Inventory, PCF"| ESG

  TECH -->|"SAP / ERP data, IT Request"| SYS
  SYS -->|"System Integration, IT Assurance"| TECH

  PROD -->|"Production-batch & Energy Data"| SYS
  SYS -->|"Batch Cacbon Intensity"| PROD

  FIN -->|"Budget, capex approval"| SYS
  SYS -->|"ROI, Green-finance MRV"| FIN

  SALES -->|"Customer Orders, Shipment data"| SYS
  SYS -->|"Shipment-level PCF, CBAM report"| SALES

  PROC -->|"Supplier carbon documents"| SYS
  SYS -->|"Scope 3 Gaps, Supplier Scoring"| PROC

  EU -->|"orders, feedback"| SYS
  SYS -->|"shipment PCF, CBAM report"| EU

  DOM -->|"orders, feedback"| SYS
  SYS -->|"ESG data report, verification"| DOM

  SYS -->|"Audit, Verification"| BSI
  BSI -->|"ISO assurance, EPD"| SYS

  SYS -->|"Requirement, Payment"| VINIT
  VINIT -->|"Data integration, AI"| SYS

  SYS -->|"MRV Reporting"| VCB
  VCB -->|"Green Credit, Capital"| SYS

  SYS -->|"Shipment query"| LOG
  LOG -->|"Batch-shipment link"| SYS

  classDef company fill:#ffffff,stroke:#111111,stroke-width:1.5px,color:#111111;
  classDef system fill:#ffffff,stroke:#063989,stroke-width:2px,color:#063989,font-size:21px,font-weight:bold;
  classDef partner fill:#ffffff,stroke:#063989,stroke-width:1.8px,color:#063989;
  classDef actor fill:#ffffff,stroke:#111111,stroke-width:1.5px,color:#111111;

  class EX,ESG,TECH,PROD,FIN,SALES,PROC company;
  class SYS system;
  class BSI,VINIT,VCB,LOG partner;
  class EU,DOM actor;

  style CUSTOMER fill:#ffffff,stroke:#111111,stroke-width:1.5px,color:#111111
  style PARTNERS fill:#ffffff,stroke:#ffffff,color:#063989

  linkStyle default stroke:#063989,stroke-width:2px;
```

The diagram represents the value chain **with EcoTrace**, without a separate Before view. It retains the finalized v7 stakeholders and exchanges, and restores VinIT and Vietcombank with their original DBIZ I arrow labels. SAP/ERP data remain represented by the Technology & Digital Transformation Department; SAP S/4HANA is not a separate partner. Partner exchanges express business relationships, not necessarily functions or integrations implemented in DBIZ II.

## 2. Problems and opportunities

The following are business problems and opportunities inferred from the value exchanges in the CVCA, rather than confirmed findings from stakeholder interviews. Partner-related opportunities do not by themselves establish that the related capabilities are included in the DBIZ II implementation.

| # | Stakeholder | Problem or opportunity |
|---|---|---|
| P-1 | Sustainability & ESG; Production Divisions | GHG methods, emission factors, and production/energy records come from different sources. There is an opportunity to consolidate them into consistent carbon inventory and PCF information. |
| P-2 | Production Divisions | Batch-level carbon intensity needs to be associated with production batches and their energy data to make the results useful to operations. |
| P-3 | Procurement & Raw Materials | Supplier carbon documents need to be consolidated so that Scope 3 data gaps can be identified and supplier information can be assessed. |
| P-4 | Export Sales & Marketing; Hoa Phat Logistics | Shipment information and batch-shipment links must be brought together to support shipment-level PCF and CBAM reporting. |
| P-5 | EU Importers; Domestic End-users | Steel buyers need accessible carbon information, including shipment PCF/CBAM information for EU importers and ESG/verification information for domestic customers. |
| P-6 | BSI; Sustainability & ESG | Carbon outputs need an independent assurance route so that verification findings can support the credibility of the information provided. |
| P-7 | Technology & Digital Transformation Dept | EcoTrace needs usable SAP/ERP information, system integration, and IT assurance to support reliable data exchange. |
| P-8 | HPDQ Executive Board; Finance & Accounting | Investment proposals, approvals, budgets, and performance/ROI information need to be linked to the system's expected business value. |
| P-9 | VinIT; Technology & Digital Transformation Dept | External data integration and AI services are a potential support opportunity, but their requirements, cost, and value need to be defined before engagement. |
| P-10 | Vietcombank; Finance & Accounting | MRV reporting may support access to green credit or capital, provided it is suitable for the partner's assessment and financing criteria. |

## 3. Stakeholder requirements

Written as **"<stakeholder> shall be able to ..."**, following the requirement style of Prof. 

| ID | Stakeholder requirement | From |
|---|---|---|
| StR-01 | The Sustainability & ESG Department shall be able to provide GHG methods and emission factors for carbon assessment. | P-1 |
| StR-02 | The Sustainability & ESG Department shall be able to obtain carbon inventory and PCF information from EcoTrace. | P-1 |
| StR-03 | Production Divisions shall be able to provide production-batch and energy data and obtain batch carbon intensity information. | P-2 |
| StR-04 | Procurement & Raw Materials shall be able to provide supplier carbon documents and identify Scope 3 gaps and supplier assessment information. | P-3 |
| StR-05 | Export Sales & Marketing shall be able to supply customer order and shipment information and obtain shipment-level PCF and CBAM reports. | P-4 |
| StR-06 | Hoa Phat Logistics shall be able to provide batch-shipment links in response to shipment queries. | P-4 |
| StR-07 | EU Importers shall be able to obtain shipment PCF and CBAM information and provide orders or feedback. | P-5 |
| StR-08 | Domestic End-users shall be able to obtain ESG reporting and verification information and provide orders or feedback. | P-5 |
| StR-09 | BSI shall be able to assess EcoTrace outputs for audit/verification and provide independent assurance findings. | P-6 |
| StR-10 | The Technology & Digital Transformation Department shall be able to coordinate SAP/ERP data exchange, system integration, and IT assurance. | P-7 |
| StR-11 | The HPDQ Executive Board shall be able to review system proposals and provide the required approval and mandate. | P-8 |
| StR-12 | Finance & Accounting shall be able to review relevant budget/capex information and obtain ROI and green-finance MRV information. | P-8 |
| StR-13 | VinIT shall be able to receive the system's requirements and agreed payment and provide data integration and AI services as a partner. | P-9 |
| StR-14 | Vietcombank shall be able to review MRV reporting to assess potential green credit or capital support. | P-10 |
