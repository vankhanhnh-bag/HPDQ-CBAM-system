# Screen Transition Diagram: EcoTrace

## 1. All users

```mermaid
flowchart LR
  entry(["Entry: user opens EcoTrace or requests a protected page while signed out"])
  S00_01["S-F00-01<br>Sign In"]
  destination(["Requested EcoTrace workspace / feature<br>(outside this F-04 screen list)"])

  entry --> S00_01
  S00_01 -->|"credentials accepted; session established"| destination
  S00_01 -->|"credentials rejected; user retries"| S00_01
```

The sign-in flow ends at the destination the user originally requested. The destination may belong to another EcoTrace feature and is therefore outside this Feature 4 screen list.

## 2. Production Department

```mermaid
flowchart LR
  entry(["Entry: Production Department user opens EcoTrace workspace while signed out"])
  S00_01["S-F00-01<br>Sign In"]
  S01_04["S-F01-04<br>Batch Carbon Intensity"]
  workspace(["EcoTrace workspace<br>(outside this F-01 screen list)"])

  entry --> S00_01
  S00_01 -->|"authenticated; Production permission granted"| S01_04
  S01_04 -->|"selects a production batch; intensity and sources displayed"| S01_04
  S01_04 -->|"returns to workspace"| workspace
```

The batch carbon intensity is calculated in the background (FR-01-03); `S-F01-04` only displays it.

## 3. Procurement & Raw Materials Department

```mermaid
flowchart LR
  entry(["Entry: Procurement & Raw Materials user opens EcoTrace workspace while signed out"])
  S00_01["S-F00-01<br>Sign In"]
  S01_02["S-F01-02<br>Supplier & Raw Material Carbon Data Entry"]
  S01_05["S-F01-05<br>Scope 3 Assessment"]
  workspace(["EcoTrace workspace<br>(outside this F-01 screen list)"])

  entry --> S00_01
  S00_01 -->|"authenticated; Procurement permission granted"| S01_02
  S00_01 -->|"authenticated; opens Scope 3 assessment"| S01_05
  S01_02 -->|"submits supplier delivery data; record saved"| S01_02
  S01_02 -->|"validation fails; reasons displayed"| S01_02
  S01_05 -->|"selects supplier or material category and requests assessment"| S01_05
  S01_02 -->|"returns to workspace"| workspace
  S01_05 -->|"returns to workspace"| workspace
```

## 4. Sustainability & ESG Department

```mermaid
flowchart LR
  entry(["Entry: Sustainability & ESG user opens EcoTrace workspace while signed out"])
  S00_01["S-F00-01<br>Sign In"]
  S01_03["S-F01-03<br>GHG Methodology & Emission Factor Library"]
  S01_06["S-F01-06<br>Carbon Traceability Record"]
  workspace(["EcoTrace workspace<br>(outside this F-01 screen list)"])

  entry --> S00_01
  S00_01 -->|"authenticated; Sustainability & ESG permission granted"| S01_03
  S00_01 -->|"authenticated; opens traceability record"| S01_06
  S01_03 -->|"adds or edits a methodology or emission factor; new version saved"| S01_03
  S01_03 -->|"validation fails; reasons displayed"| S01_03
  S01_06 -->|"selects a shipped product batch; full traceability chain displayed"| S01_06
  S01_03 -->|"returns to workspace"| workspace
  S01_06 -->|"returns to workspace"| workspace
```

The traceability links between raw material, production batch and shipment are created in the background when records are saved (FR-01-07); `S-F01-06` only displays them.

## 5. Export Sales & Marketing / Procurement & Raw Materials Department

```mermaid
flowchart LR
  entry(["Entry: clicks EcoTrace link from Hòa Phát website, or opens a bookmarked URL"])
  S00_01["S-F00-01<br>Sign in"]
  S02_01["S-F02-01<br>CBAM Estimation & Analysis"]
  S01_01["S-F01-01<br>Shipment-level PCF Calculation"]
  workspace(["EcoTrace workspace<br>(outside this F-01 / F-02 screen list)"])

  entry --> S00_01
  S00_01 -->|"signed in; stored shipment PCF available"| S02_01
  S00_01 -->|"Export Sales: opens PCF calculation if required"| S01_01
  S02_01 -->|"Export Sales: shipment PCF missing; opens PCF calculation"| S01_01
  S01_01 -->|"PCF calculated and stored; continue to CBAM"| S02_01
  S02_01 -->|"selects Order No."| S02_01
  S02_01 -->|"taps Calculate"| S02_01
  S02_01 -->|"taps View Assumptions<br>[opens M-F02-02]"| S02_01
  S02_01 -->|"taps Share<br>[opens M-F02-01]"| S02_01
  S02_01 -->|"confirms Share in dialog"| S02_01
  S02_01 -->|"taps Download Report<br>[file downloads]"| S02_01
  S01_01 -->|"returns to workspace"| workspace
  S02_01 -->|"returns to workspace"| workspace
```

UC-04 (`S-F01-01`) is an optional preparation step for Export Sales & Marketing when valid shipment PCF is not yet available. With existing verified carbon/PCF inputs, Export Sales & Marketing or Procurement & Raw Materials proceeds directly to UC-07 (`S-F02-01`).

## 6. Compliance / Product Owner

```mermaid
flowchart LR
  entry(["Entry: Compliance / Product Owner opens CBAM Regulatory Settings"])
  S00_01["S-F00-01<br>Sign In"]
  S02_02["S-F02-02<br>CBAM Regulatory Settings"]
  S02_03["S-F02-03<br>CBAM Regulatory Rule Set Editor"]
  workspace(["EcoTrace workspace"])

  entry --> S00_01
  S00_01 -->|"authenticated; view permission granted"| S02_02
  S00_01 -->|"view permission denied; access blocked"| workspace
  S02_02 -->|"reviews active and scheduled rule sets"| S02_02
  S02_02 -->|"creates a rule set or edits a draft; edit permitted"| S02_03
  S02_02 -->|"edit permission denied; remains read-only"| S02_02
  S02_03 -->|"saves rule-set draft"| S02_03
  S02_03 -->|"validation fails; reasons displayed"| S02_03
  S02_03 -->|"requests activation; opens M-F02-03"| S02_03
  S02_03 -->|"cancels activation modal"| S02_03
  S02_03 -->|"confirms; validation and authorization pass; activated or scheduled"| S02_02
  S02_03 -->|"activation not permitted; remains in editor"| S02_03
  S02_03 -->|"back to regulatory settings"| S02_02
  S02_02 -->|"returns to workspace"| workspace
```

`M-F02-03` is a confirmation modal over `S-F02-03`, **not** another screen. Access-denied, invalid-draft and cancellation outcomes are states of existing screens, not separate Screen IDs. UC-08 covers FR-02-14 to FR-02-17 and shared access/audit controls.

## 7. Customers

```mermaid
flowchart LR
  entry(["Entry: Procurement user opens EcoTrace workspace or comparison link while signed out"])
  S00_01["S-F00-01<br>Sign In"]
  S_F03_01["S-F03-01<br>Green Steel Filter & Comparison Criteria"]
  S_F03_02["S-F03-02<br>Green Steel Comparison & Recommendation Board"]
  workspace(["EcoTrace workspace<br>(outside this F-03 screen list)"])

  entry --> S00_01
  S00_01 -->|"authenticated; procurement permission granted"| S_F03_01
  S_F03_01 -->|"submits filter criteria and clicks compare"| S_F03_02
  S_F03_02 -->|"modifies filters or goes back"| S_F03_01
  S_F03_01 -->|"returns to workspace"| workspace
  S_F03_02 -->|"returns to workspace"| workspace
```

## 8. System Admin

```mermaid
flowchart LR
  entry(["Entry: System Admin opens Platform / Access while signed out"])
  S00_01["S-F00-01<br>Sign In"]
  S04_01["S-F04-01<br>User & Access Management"]
  S04_02["S-F04-02<br>Audit Log Viewer"]
  workspace(["EcoTrace workspace<br>(outside this F-04 screen list)"])

  entry --> S00_01
  S00_01 -->|"authenticated; required permission granted"| S04_01
  S04_01 -->|"opens Audit Logs"| S04_02
  S04_02 -->|"returns to User & Access Management"| S04_01
  S04_01 -->|"returns to workspace"| workspace
  S04_02 -->|"returns to workspace"| workspace
```
## 9. Auditor / System Admin

```mermaid
flowchart LR
  entry(["Entry: System Admin or Auditor opens Audit Logs while signed out"])
  S00_01["S-F00-01<br>Sign In"]
  S04_02["S-F04-02<br>Audit Log Viewer"]
  workspace(["EcoTrace workspace<br>(outside this F-04 screen list)"])

  entry --> S00_01
  S00_01 -->|"authenticated; audit-view permission granted"| S04_02
  S04_02 -->|"returns to workspace"| workspace
```


## Checks
- [x] Every screen in the Screen List appears in exactly one or more of the diagrams.
- [x] Every screen is reachable from an entry point.
- [x] No dead end: every screen has a way out (back, home, or a next step).
- [x] The main task of each actor reaches its result in three screens or fewer from the start
screen, or the team has written down why not.
- [x] Every arrow label is a user action or a system event, never a screen name.
- [x] Usage-flow paths: for every arrow "UC-a to UC-b" in usage-flow.md there is a path from a
screen of UC-a to a screen of UC-b, or UC-b starts from an entry point (email link, QR
code) because the actor leaves the product in between. Fill the table below.
- [x] The diagram renders on mermaid.live.


## Usage-flow paths

|Usage-flow arrow|Path on this diagram|
|-|-|
|UC-10 to UC-01|Procurement & Raw Materials: `S-F00-01` → `S-F01-02` (authenticated; Procurement permission granted).|
|UC-10 to UC-02|Sustainability & ESG: `S-F00-01` → `S-F01-03` (authenticated; Sustainability & ESG permission granted).|
|UC-10 to UC-03|Production: `S-F00-01` → `S-F01-04` (authenticated; Production permission granted).|
|UC-10 to UC-05|Procurement & Raw Materials: `S-F00-01` → `S-F01-05` (authenticated; opens Scope 3 assessment).|
|UC-10 to UC-06|Sustainability & ESG: `S-F00-01` → `S-F01-06` (authenticated; opens traceability record).|
|UC-10 to UC-04|Export Sales & Marketing: `S-F00-01` → `S-F01-01` (signed in; opens shipment-level PCF calculation).|
|UC-04 to UC-07 (if PCF required)|Export Sales & Marketing: `S-F01-01` → `S-F02-01` (PCF calculated and stored; user proceeds to CBAM estimation).|
|UC-10 to UC-07|Export Sales & Marketing / Procurement & Raw Materials: `S-F00-01` → `S-F02-01` (signed in; stored shipment PCF available; UC-07 validates verified carbon intensity and other required inputs).|
|UC-10 to UC-08|Compliance / Product Owner: `S-F00-01` → `S-F02-02` (authenticated; view permission granted).|
|UC-08 rule maintenance|Compliance / Product Owner: `S-F02-02` → `S-F02-03` (create/edit permitted) → `S-F02-02` (after successful activation or return). `M-F02-03` is a modal on `S-F02-03`, not a separate screen.|
|UC-10 to UC-09|Customers: `S-F00-01` → `S-F03-01` → `S-F03-02` (authenticated; submits comparison criteria and starts comparison).||UC-10 to UC-11|System Admin: `S-F00-01` → `S-F04-01` (authenticated; permission granted).|
|UC-10 to UC-12|System Admin / Auditor: `S-F00-01` → `S-F04-02` (authenticated; audit-view permission granted).|
|UC-11 to UC-12|System Admin: `S-F04-01` → `S-F04-02` (opens Audit Logs).|



