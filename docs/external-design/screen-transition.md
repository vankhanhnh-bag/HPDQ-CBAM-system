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

## 7. ...

```mermaid
flowchart LR
  entry(["Entry: clicks EcoTrace link from Hòa Phát website, or opens a bookmarked URL"])
  S00_01["S-F00-01<br>Sign in"]
  S02_01["S-F02-01<br>CBAM Estimation & Analysis"]

  entry --> S00_01
  S00_01 -->|"signed in"| S02_01
  S02_01 -->|"selects Order No."| S02_01
  S02_01 -->|"taps Calculate"| S02_01
  S02_01 -->|"taps View Assumptions<br>[opens M-F02-02]"| S02_01
  S02_01 -->|"taps Share<br>[opens M-F02-01]"| S02_01
  S02_01 -->|"confirms Share in dialog"| S02_01
  S02_01 -->|"taps Download Report<br>[file downloads]"| S02_01
```

## 10. Customers

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

## 11. System Admin

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
## 12. Auditor / System Admin

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

## 13. Compliance / Product Owner

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

## Checks (chưa fix)

- Every screen in the Screen List appears: yes (7 of 7).
- Every screen reachable from an entry point: yes.
- No dead ends: yes.
- Usage-flow paths: every arrow of [usage-flow.md](usage-flow.md) has a path (table below).
- Sign in is shared; signed-out users who open an email or QR link see S-F00-01 first and then
  return to the screen they asked for (screen-level rule in S-F00-01).

## Usage-flow paths

|Usage-flow arrow|Path on this diagram|
|-|-|
|UC-09 to UC-09|Customers: `S-F00-01` → `S-F03-01` (authenticated; procurement permission granted) → `S-F03-02` (submits filter criteria and clicks compare). |
|UC-10 to UC-11|System Admin: `S-F00-01` → `S-F04-01` (authenticated; permission granted).|
|UC-10 to UC-12|System Admin / Auditor: `S-F00-01` → `S-F04-02` (authenticated; audit-view permission granted).|
|UC-11 to UC-12|System Admin: `S-F04-01` → `S-F04-02` (opens Audit Logs).|



