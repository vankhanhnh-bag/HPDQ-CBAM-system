# Use Case Diagram: HPDQ

| Field | Value |
|---|---|
| Actors | Procurement, Sustainability & ESG, Export Sales & Marketing, Customers; Time (starts UC-01) |

## Diagram
```mermaid
%%{init: {'theme': 'base', 'flowchart': {'nodeSpacing': 40, 'rankSpacing': 50}}}%%
graph LR
    subgraph EcoTrace["System Boundary: EcoTrace"]
        direction TB
        
        subgraph F1["F-01 Carbon Traceability & Product Carbon<br/>Footprint Management Operations"]
            UC01["UC-01 Submit supplier &<br/>raw material carbon data"]
            UC02["UC-02 Manage GHG methodology<br/>& emission factors"]
            UC03["UC-03 Access batch carbon intensity"]
            UC04["UC-04 Calculate shipment-level PCF"]
            UC05["UC-05 Perform Scope 3 assessment"]
            UC06["UC-06 View carbon traceability record"]
        end

        subgraph F2["F-02 CBAM Compliance & Carbon Cost"]
            UC07["UC-07 Estimate CBAM cost for shipment"]
            UC08["UC-08 Configure CBAM regulatory parameters"]
        end

        subgraph F3["F-03 Green Steel Comparison & Recommendation"]
            UC09["UC-09 Compare and recommend green steel options"]
        end

        subgraph F4["F-04 Platform, Access & Operations"]
            UC10["UC-10 Authenticate / sign in"]
            UC11["UC-11 Configure RBAC & authorization"]
            UC12["UC-12 Monitor / export audit logs"]
        end
    end

    %% Các liên kết Actor theo đúng yêu cầu đã xác nhận
    Procurement["Procurement"] --- UC01
    Sustainability["Sustainability & ESG"] --- UC02
    Production["Production"] --- UC03
    ExportSales["Export Sales & Marketing"] --- UC04
    Procurement --- UC05
    Sustainability --- UC06

    ExportSalesRight["Export Sales & Marketing"] --- UC07
    ProcurementTeam["Procurement team"] --- UC07
    ComplianceTeam["Compliance team"] --- UC08
    Customers["Customers"] --- UC09
    AllUsers["All users"] --- UC10
    SystemAdmin["System admin"] --- UC11
```
## Sequence diagrams

One sequence diagram per use case, at analysis level: the lifelines are the elements of the
[physical design](../architecture/architecture.md).

| Use case | Sequence diagram | Status |
|---|---|---|
| UC-01 Submit supplier & raw material carbon data | [SD-UC-01](SD-UC-01.md) | Done (split from "Booking flow" after Lesson 7) |
| UC-02 Manage GHG methodology & emission factors |  [SD-UC-02](SD-UC-02.md)  | Open: still to be drawn (Inputs check row 4, Owner A) |
| UC-03 Access batch carbon intensity | [SD-UC-03](SD-UC-03.md) | Done |
| UC-04 Calculate shipment-level PCF | [SD-UC-04](SD-UC-04.md) | Done (split from "Booking flow" after Lesson 7) |
| UC-05 Perform Scope 3 assessment |  [SD-UC-05](SD-UC-05.md)  | Open: still to be drawn (Inputs check row 4, Owner A) |
| UC-06 View carbon traceability record | [SD-UC-06](SD-UC-06.md) | Done |
| UC-07 Estimate & analyze CBAM exposure for shipment | [SD-UC-07](SD-UC-07.md) | Done (split from "Booking flow" after Lesson 7) |
| UC-08 Manage CBAM regulatory rules & parameters |  [SD-UC-08](SD-UC-08.md)  | Open: still to be drawn (Inputs check row 4, Owner A) |
| UC-09 Compare and recommend green steel options | [SD-UC-09](SD-UC-09.md) | Done |
| UC-10 Authenticate and sign in to the platform | [SD-UC-10](SD-UC-10.md) | Done (split from "Booking flow" after Lesson 7) |
| UC-11 Configure Role-Based Access Control (RBAC) and system authorization |  [SD-UC-11](SD-UC-11.md)  | Open: still to be drawn (Inputs check row 4, Owner A) |
| UC-12 Monitor, track, and export system audit logs | [SD-UC-12](SD-UC-12.md) | Done |

## Change log

| Version | When | Change | Why |
|---|---|---|---|



