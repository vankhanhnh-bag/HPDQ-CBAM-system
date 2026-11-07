# Usage Flow: EcoTrace

| Field | Value |
|---|---|
| Product | EcoTrace (HPDQ Carbon Management & Reporting) |
| Version | v0.2 (actor-organized integration draft) |
| Date | 2026-11-07 |
| Sources | `docs/function-list/W0_Scope.md`; Use Case Diagram; `docs/activity/`; `docs/external-design/screen-list.md` |

> Usage flows are grouped **by actor**, then **by distinct actor goal**. Each Mermaid box represents a Use Case, not a click or screen. The original group flows are retained; F-02 (UC-07 and UC-08) is added under the actors already present, rather than under a separate feature heading. Sign-in (UC-10) is a shared F-04 use case and only applies when the user needs to authenticate; an existing session proceeds to the destination UC.

## All Users

### Sign in to access EcoTrace

```mermaid
flowchart LR
  A(["User needs to access EcoTrace"])
  U1["UC-10: Authenticate and Sign In"]
  Z(["Authenticated session established; user proceeds to the requested EcoTrace destination"])
  A --> U1 --> Z
```

|Step|Use case|Activity diagram|Screens (fill in after Block 2)|
|-|-|-|-|
|1|UC-10 Authenticate and Sign In|`docs/activity/AD-UC-10.md`|`S-F00-01`|

## Procurement

### Record supplier and raw material carbon data

```mermaid
flowchart LR
  A(["Procurement needs to record carbon information for a supplier delivery"])
  U1["UC-01: Submit supplier & raw material carbon data"]
  Z(["Supplier delivery carbon record stored in EcoTrace"])
  A --> U1 --> Z
```

|Step|Use case|Activity diagram|Screens (fill in after Block 2)|
|-|-|-|-|
|1|UC-01 Submit supplier & raw material carbon data|`docs/activity/AD-UC-01.md`|-|

### Assess Scope 3 emissions

```mermaid
flowchart LR
  A(["Procurement needs to assess Scope 3 emissions for a supplier or material category"])
  U1["UC-05: Perform Scope 3 assessment"]
  Z(["Scope 3 assessment summary available for review"])
  A --> U1 --> Z
```

|Step|Use case|Activity diagram|Screens (fill in after Block 2)|
|-|-|-|-|
|1|UC-05 Perform Scope 3 assessment|`docs/activity/AD-UC-05.md` **(not present in current activity-diagram set)**|-|

### Estimate CBAM exposure for procurement decisions (Procurement & Raw Materials)

```mermaid
flowchart LR
  A(["Procurement & Raw Materials needs to assess CBAM cost exposure for a shipment"])
  U1["UC-10: Authenticate and Sign In"]
  U2["UC-07: Estimate & analyze CBAM exposure for shipment"]
  Z(["Shipment CBAM estimate and supporting breakdown available for procurement review"])
  A --> U1 --> U2 --> Z
```

|Step|Use case|Activity diagram|Screens (fill in after Block 2)|
|-|-|-|-|
|1|UC-10 Authenticate and Sign In|`docs/activity/AD-UC-10.md`|`S-F00-01`|
|2|UC-07 Estimate & analyze CBAM exposure for shipment|`docs/activity/AD-UC-07.md`|`S-F02-01`|

## Sustainability & ESG

### Manage GHG methodology and emission factors

```mermaid
flowchart LR
  A(["Sustainability & ESG needs to create or update GHG methodology or emission factors"])
  U1["UC-02: Manage GHG methodology & emission factors"]
  Z(["GHG methodology or emission-factor configuration saved"])
  A --> U1 --> Z
```

|Step|Use case|Activity diagram|Screens (fill in after Block 2)|
|-|-|-|-|
|1|UC-02 Manage GHG methodology & emission factors|`docs/activity/AD-UC-02.md` **(not present in current activity-diagram set)**|-|

### View carbon traceability record

```mermaid
flowchart LR
  A(["Sustainability & ESG needs to trace a shipped product batch"])
  U1["UC-06: View carbon traceability record"]
  Z(["Carbon traceability record displayed for review"])
  A --> U1 --> Z
```

|Step|Use case|Activity diagram|Screens (fill in after Block 2)|
|-|-|-|-|
|1|UC-06 View carbon traceability record|`docs/activity/AD-UC-06.md`|-|

## Production

### Access batch carbon intensity

```mermaid
flowchart LR
  A(["Production needs the carbon intensity of a production batch"])
  U1["UC-03: Access batch carbon intensity"]
  Z(["Batch carbon intensity available for review"])
  A --> U1 --> Z
```

|Step|Use case|Activity diagram|Screens (fill in after Block 2)|
|-|-|-|-|
|1|UC-03 Access batch carbon intensity|`docs/activity/AD-UC-03.md` **(not present in current activity-diagram set)**|-|

## Export Sales & Marketing

### Calculate shipment-level PCF

```mermaid
flowchart LR
  A(["Export Sales & Marketing needs the PCF of a shipment"])
  U1["UC-04: Calculate shipment-level PCF"]
  Z(["Shipment-level PCF calculated, stored, and displayed"])
  A --> U1 --> Z
```

|Step|Use case|Activity diagram|Screens (fill in after Block 2)|
|-|-|-|-|
|1|UC-04 Calculate shipment-level PCF|`docs/activity/AD-UC-04.md`|`S-F01-01`|


### Estimate CBAM exposure for an export shipment

```mermaid
flowchart LR
  A(["Export Sales & Marketing needs to estimate CBAM cost exposure for an export shipment"])
  U10["UC-10: Authenticate and Sign In"]
  D{"Valid shipment-level PCF data already available?"}
  U04["UC-04: Calculate shipment-level PCF"]
  U07["UC-07: Estimate & analyze CBAM exposure for shipment"]
  Z(["CBAM estimate and calculation breakdown available for quotation or review"])

  A --> U10 --> D
  D -->|"Yes: use existing PCF data"| U07
  D -->|"No: PCF calculation required"| U04
  U04 -->|"PCF successfully calculated and stored"| U07
  U07 --> Z
```

|Step|Use case|Activity diagram|Screens (fill in after Block 2)|
|-|-|-|-|
|1|UC-10 Authenticate and Sign In|`docs/activity/AD-UC-10.md`|`S-F00-01`|
|2 (if required)|UC-04 Calculate shipment-level PCF|`docs/activity/AD-UC-04.md`|`S-F01-01`|
|3|UC-07 Estimate & analyze CBAM exposure for shipment|`docs/activity/AD-UC-07.md`|`S-F02-01`|

> **Dependency:** UC-07 requires valid shipment and verified carbon/PCF data. UC-04 calculates and stores shipment-level PCF. When valid PCF data already exists, the user can proceed directly to UC-07 without recalculating it. Otherwise, UC-04 may be performed first to prepare the required PCF data. This is a conditional user journey, not an automatic system invocation of UC-04. If required inputs remain missing or invalid, UC-07 blocks estimation and displays validation reasons (FR-02-02).


## Compliance / Product Owner

### Manage CBAM regulatory rules and parameters

```mermaid
flowchart LR
  A(["Compliance / Product Owner needs to review or maintain CBAM regulatory rules"])
  U1["UC-10: Authenticate and Sign In"]
  U2["UC-08: Manage CBAM regulatory rules & parameters"]
  Z(["Configuration reviewed, rule-set draft saved, or validated version activated or scheduled"])
  A --> U1 --> U2 --> Z
```

|Step|Use case|Activity diagram|Screens (fill in after Block 2)|
|-|-|-|-|
|1|UC-10 Authenticate and Sign In|`docs/activity/AD-UC-10.md`|`S-F00-01`|
|2|UC-08 Manage CBAM regulatory rules & parameters|`docs/activity/AD-UC-08.md`|`S-F02-02`, `S-F02-03` (when creating/editing)|

> In UC-08, `S-F02-02` is the settings view and `S-F02-03` is the rule-set editor. `M-F02-03` is an activation-confirmation modal, not an additional use case or screen.

## System Admin

### Administer user access

```mermaid
flowchart LR
  A(["System Admin needs to administer EcoTrace user access"])
  U1["UC-10: Authenticate and Sign In"]
  U2["UC-11: Manage User Access (working title; confirm official title)"]
  Z(["User-access administration task completed"])
  A --> U1 --> U2 --> Z
```

|Step|Use case|Activity diagram|Screens (fill in after Block 2)|
|-|-|-|-|
|1|UC-10 Authenticate and Sign In|`docs/activity/AD-UC-10.md`|`S-F00-01`|
|2|UC-11 Manage User Access (working title)|`AD-UC-11.md` **(not present in current activity-diagram set)**|`S-F04-01`|

### Review platform activity (also available to Auditor)

```mermaid
flowchart LR
  A(["System Admin or Auditor needs to review recorded EcoTrace activity"])
  U1["UC-10: Authenticate and Sign In"]
  U2["UC-12: Review Audit Logs (working title; confirm official title)"]
  Z(["Relevant audit activity reviewed"])
  A --> U1 --> U2 --> Z
```
|Step|Use case|Activity diagram|Screens (fill in after Block 2)|
|-|-|-|-|
|1|UC-10 Authenticate and Sign In|`docs/activity/AD-UC-10.md`|`S-F00-01`|
|2|UC-12 Review Audit Logs (working title)|`AD-UC-12.md` **(not present in current activity-diagram set)**|`S-F04-02`|

## Customer

### Compare and select green steel options

```mermaid
flowchart LR
  A(["Customer needs to compare and select optimal green steel products"])
  U1["UC-10: Authenticate and Sign In"]
  U2["UC-09: Compare and recommend green steel options"]
  Z(["Optimal green steel option selected and recommended product reviewed"])
  A --> U1 --> U2 --> Z
```
|Step|Use case|Activity diagram|Screens (fill in after Block 2)|
|-|-|-|-|
|1|UC-10 Authenticate and Sign In|`docs/activity/AD-UC-10.md`|`S-F00-01`|
|2|UC-09 Compare and recommend green steel options|`docs/activity/AD-UC-09.md`|`S-F03-01, S-F03-02`|


## Use cases without a flow (ai làm thì xóa dòng mặc định và ghi chú này)

| Use case | Started by | Described in |
|---|---|---|
| UC-05 Release no-show seats | Time (15 minutes after a slot starts) | `AD-UC-05.md`; the student only sees the result as an email |

## Checks before you save (chưa fix)

- [x] Every actor that starts a use case has at least one flow.
- [x] An actor with clearly different goals has one flow per goal; no flow has more than about
      9 boxes (Student 4 use cases, Lab Technician 2).
- [x] Every actor name matches the Use Case Diagram and the Context Diagram.
- [x] Coverage: UC-01 to UC-04 (Must) and UC-06, UC-07 (Should) each appear in a flow with the
      same ID and name; UC-05 is listed under "Use cases without a flow"; no Won't-have use case
      appears.
- [x] No box is a click or a screen; no box tells two use cases.
- [x] Every flow renders on mermaid.live.
- [x] Every step has at least one screen, and every screen in `screen-list.md` appears in some step.
