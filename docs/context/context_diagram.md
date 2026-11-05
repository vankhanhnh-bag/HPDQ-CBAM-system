# Context Diagram: HPDQ Carbon Management \& Reporting System

|Field|Value|
|-|-|
|Diagram ID|`CD-01`|
|System Name|`HPDQ Carbon Management \& Reporting System`|
|Primary Purpose|Map system boundaries, external actors, and high-level data flows across internal departments and external partners.|
|Version / status|v0.1 · Draft|
|Author|Team Sample|

## Diagram

```
graph TD
    SYS["<b>HPDQ Carbon Management & Reporting system</b>"]

    subgraph INT_DEPS["Internal Departments"]
        PROD["Production"]
        PROC["Procurement"]
        SALES["Export Sales<br>& Marketing"]
        FIN_ACC["Finance<br>& Accounting"]
        ESG["Sustainability<br>& ESG"]
        TECH["Technology & Digital<br>Transformation"]
    end

    BSI["Verification & Assurance Body<br>(BSI)"]
    AI["Data & AI partner"]
    LOG["Logistics Partner<br>(Hoa Phat Logistics)"]
    CUST["Customers / EU Importers<br>(Under CBAM)"]
    FIN_PART["Green Finance Partner"]

    ESG -->|"GHG Method,<br>Emission Factors"| SYS
    SYS -->|"Verified Inventory,<br>PCF"| ESG

    TECH -->|"SAP / ERP data,<br>IT Request"| SYS
    SYS -->|"System Integration,<br>IT Assurance"| TECH

    PROD -->|"Production-batch<br>& Energy Data"| SYS
    SYS -->|"Batch Carbon<br>Intensity"| PROD

    PROC -->|"Supplier carbon<br>documents"| SYS
    SYS -->|"Scope 3 Gaps,<br>Supplier Scoring"| PROC

    SALES -->|"Customer Orders,<br>Shipment data"| SYS
    SYS -->|"Shipment-level PCF,<br>CBAM report"| SALES

    FIN_ACC -->|"Budget,<br>capex approval"| SYS
    SYS -->|"ROI,<br>Green-finance MRV"| FIN_ACC

    SYS -->|"Emissions & PCF Reports,<br>Supporting Evidence"| BSI
    BSI -->|"Audit Results,<br>Verification /<br>Assurance"| SYS

    SYS -->|"Data<br>Requirements"| AI
    AI -->|"Data Integration, AI Models,<br>System Support"| SYS

    SYS -->|"Batch - Shipment Link,<br>Delivery Status"| LOG
    LOG -->|"Shipment /<br>Consignment Information"| SYS

    SYS -->|"PCF, CBAM Reports<br>Information"| CUST
    CUST -->|"Customer Orders, Requirements,<br>Feedback"| SYS

    FIN_PART -->|"MRV Reports, ESG Data,<br>PCF Results"| SYS
    SYS -->|"Green credit, financing<br>terms & feedback"| FIN_PART

    %% Styling & Palette
    style SYS fill:#e1f5fe,stroke:#01579b,stroke-width:3px,shape:circle
    style INT_DEPS fill:#ffd1dc,stroke:#ff69b4,stroke-width:2px
    style PROD fill:#ffffff,stroke:#333
    style PROC fill:#ffffff,stroke:#333
    style SALES fill:#ffffff,stroke:#333
    style FIN_ACC fill:#ffffff,stroke:#333
    style ESG fill:#ffffff,stroke:#333
    style TECH fill:#ffffff,stroke:#333
    style BSI fill:#e8eaf6,stroke:#3f51b5,stroke-width:1.5px
    style AI fill:#ede7f6,stroke:#673ab7,stroke-width:1.5px
    style LOG fill:#e0f7fa,stroke:#00acc1,stroke-width:1.5px
    style CUST fill:#fff8e1,stroke:#ffb300,stroke-width:1.5px
    style FIN_PART fill:#e8f5e9,stroke:#4caf50,stroke-width:1.5px

```



