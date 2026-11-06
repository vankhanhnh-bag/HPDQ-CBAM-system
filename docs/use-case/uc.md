# Use Case Diagram: LabDesk

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
    Auditor["Auditor"] --- UC12
