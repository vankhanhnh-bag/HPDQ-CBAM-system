# Context Diagram (DFD Level 0): HPDQ Carbon Management \& Reporting System

|Field|Value|
|-|-|
|Diagram ID|`CD-01`|
|System Name|`HPDQ Carbon Management \& Reporting System`|
|Primary Purpose|Map system boundaries, external actors, and high-level data flows across internal departments and external partners.|
|Version / status|v0.4 · Draft|
|Author|Team Sample|

## 1\. Diagram

```
graph TD

&#x20;   SYS\["<b>HPDQ Carbon Management \& Reporting system</b>"]



&#x20;   subgraph INT\_DEPS\["Internal Departments"]

&#x20;       PROD\["Production"]

&#x20;       PROC\["Procurement"]

&#x20;       SALES\["Export Sales<br>\& Marketing"]

&#x20;       FIN\_ACC\["Finance<br>\& Accounting"]

&#x20;       ESG\["Sustainability<br>\& ESG"]

&#x20;       TECH\["Technology \& Digital<br>Transformation"]

&#x20;   end



&#x20;   BSI\["Verification \& Assurance Body<br>(BSI)"]

&#x20;   AI\["Data \& AI partner"]

&#x20;   LOG\["Logistics Partner<br>(Hoa Phat Logistics)"]

&#x20;   CUST\["Customers / EU Importers<br>(Under CBAM)"]

&#x20;   FIN\_PART\["Green Finance Partner"]



&#x20;   ESG -->|"GHG Method,<br>Emission Factors"| SYS

&#x20;   SYS -->|"Verified Inventory,<br>PCF"| ESG



&#x20;   TECH -->|"SAP / ERP data,<br>IT Request"| SYS

&#x20;   SYS -->|"System Integration,<br>IT Assurance"| TECH



&#x20;   PROD -->|"Production-batch<br>\& Energy Data"| SYS

&#x20;   SYS -->|"Batch Carbon<br>Intensity"| PROD



&#x20;   PROC -->|"Supplier carbon<br>documents"| SYS

&#x20;   SYS -->|"Scope 3 Gaps,<br>Supplier Scoring"| PROC



&#x20;   SALES -->|"Customer Orders,<br>Shipment data"| SYS

&#x20;   SYS -->|"Shipment-level PCF,<br>CBAM report"| SALES



&#x20;   FIN\_ACC -->|"Budget,<br>capex approval"| SYS

&#x20;   SYS -->|"ROI,<br>Green-finance MRV"| FIN\_ACC



&#x20;   SYS -->|"Emissions \& PCF Reports,<br>Supporting Evidence"| BSI

&#x20;   BSI -->|"Audit Results,<br>Verification /<br>Assurance"| SYS



&#x20;   SYS -->|"Data<br>Requirements"| AI

&#x20;   AI -->|"Data Integration, AI Models,<br>System Support"| SYS



&#x20;   SYS -->|"Batch - Shipment Link,<br>Delivery Status"| LOG

&#x20;   LOG -->|"Shipment /<br>Consignment Information"| SYS



&#x20;   SYS -->|"PCF, CBAM Reports<br>Information"| CUST

&#x20;   CUST -->|"Customer Orders, Requirements,<br>Feedback"| SYS



&#x20;   FIN\_PART -->|"MRV Reports, ESG Data,<br>PCF Results"| SYS

&#x20;   SYS -->|"Green credit, financing<br>terms \& feedback"| FIN\_PART



&#x20;   %% Styling \& Palette

&#x20;   style SYS fill:#e1f5fe,stroke:#01579b,stroke-width:3px,shape:circle

&#x20;   style INT\_DEPS fill:#ffd1dc,stroke:#ff69b4,stroke-width:2px

&#x20;   style PROD fill:#ffffff,stroke:#333

&#x20;   style PROC fill:#ffffff,stroke:#333

&#x20;   style SALES fill:#ffffff,stroke:#333

&#x20;   style FIN\_ACC fill:#ffffff,stroke:#333

&#x20;   style ESG fill:#ffffff,stroke:#333

&#x20;   style TECH fill:#ffffff,stroke:#333

&#x20;   style BSI fill:#e8eaf6,stroke:#3f51b5,stroke-width:1.5px

&#x20;   style AI fill:#ede7f6,stroke:#673ab7,stroke-width:1.5px

&#x20;   style LOG fill:#e0f7fa,stroke:#00acc1,stroke-width:1.5px

&#x20;   style CUST fill:#fff8e1,stroke:#ffb300,stroke-width:1.5px

&#x20;   style FIN\_PART fill:#e8f5e9,stroke:#4caf50,stroke-width:1.5px


```

## 2\. External Entities Summary

|Entity Name|Type|Key Information Flow Exchanged|
|-|-|-|
|**Internal Departments**|Internal Actor|GHG methods, SAP/ERP data, production batch data, scope 3 gaps, budgets, and capex approvals.|
|**Verification \& Assurance Body (BSI)**|External Auditor|Receives emissions reports/supporting evidence; returns audit results and assurance status.|
|**Green Finance Partner**|External Financial Partner|Receives MRV reports and ESG data; provides green credit financing terms.|
|**Data \& AI Partner**|External Technology Partner|Exchanges system support, data requirements, data integration, and AI model inputs.|
|**Logistics Partner (Hoa Phat Logistics)**|External Partner|Exchanges shipment tracking data, consignment status, and delivery milestones.|
|**Customers / EU Importers**|External Customer|Sends orders and feedback; receives PCF and CBAM compliance reports.|

## 3\. Verification Log

|Check|Result|By|Date|
|-|-|-|-|
|Renders clearly on mermaid.live|Pass|Team|2026-11-06|
|Colored external entity boxes added|Pass|Team|2026-11-06|



