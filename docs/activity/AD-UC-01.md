# Activity Diagram: AD-UC-01 — Submit supplier & raw material carbon data

> **EcoTrace — HPDQ Carbon Management & Reporting System.** This document expands the team's existing activity diagram into the DBIZ2 activity-diagram template. The Mermaid flow is preserved from the supplied file. 

| Field | Value |
|---|---|
| AD ID | AD-UC-01 |
| Use case | UC-01 Submit supplier & raw material carbon data |
| Feature | Supplier and raw-material data collection |
| Primary actor | Procurement Department |
| Lanes | Procurement Department · EcoTrace |
| Inputs (L3–L5) | Supplied AD-UC-01 diagram, Context Diagram, Use Case Diagram, corresponding Sequence Diagram, Architecture, and W1/FR baseline (not supplied for cross-checking) |
| Trigger | The Procurement Department submits supplier delivery information. |
| Preconditions | The actor must be able to access the submission process; authentication/authorization prerequisites are. |
| Success end state | EcoTrace has successfully stored the submitted supplier delivery record (supplier identity, raw material type, quantity, and declared carbon data), and the Procurement Department receives a success confirmation. |
| Failure end state | If storage fails, the user receives an unsuccessful-submission outcome; the flow ends without a confirmed saved record. Incomplete/invalid input is a **correction loop**, not a terminal failure. |
| FRs covered | FR-01-01 (the only FR ID cited by the supplied diagram; verify full UC coverage against W1) |
| Business rules applied | Completeness/validity of required delivery information; save-success verification. |
| Version / status | v0.1 |
| Author | Huyen Khanh |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). The diagram uses `swimlane-beta`, requiring Mermaid 11.16 or later.

```mermaid
swimlane-beta TB
  subgraph ACT["Procurement Department"]
    start@{ shape: sm-circ }
    a2("Submit supplier delivery information")
    a4("Correct missing or invalid delivery information")
    a3("Review successful submission outcome")
    a5("Review unsuccessful submission outcome")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  subgraph SYS["EcoTrace"]
    m1{" "}
    d1{"Required delivery information complete and valid?"}
    s2("Return input errors and request correction (FR-01-01)")
    s1("Capture and store supplier identity, raw material type, quantity and declared carbon data (FR-01-01)")
    d2{"Delivery record stored successfully?"}
    s3("Report that submission was not saved (FR-01-01)")
    s4("Confirm successful submission (FR-01-01)")
  end
  start --> m1 --> a2 --> d1
  d1 -->|"[no]"| s2 --> a4 --> m1
  d1 -->|"[yes]"| s1 --> d2
  d2 -->|"[no]"| s3 --> a5 --> stop2
  d2 -->|"[yes]"| s4 --> a3 --> stop1
```

Reading notes:

- `m1` is a **merge** for the first submission and the retry after the Procurement Department corrects invalid information. It does **not** mean those flows must occur concurrently.
- `d1` checks the required delivery information before the storage action. The `[no]` branch returns validation errors and loops back through user correction; the `[yes]` branch proceeds to record storage.
- `d2` checks whether the record was stored. The `[no]` branch terminates with an unsuccessful outcome, while the `[yes]` branch confirms success. The source diagram does not include a retry after a storage failure.
- The diagram has no fork/join or explicit object-state node. It makes no claim about the record's complete lifecycle or duplicate-submission handling.

## 2. Node trace

Every EcoTrace action cites `FR-01-01` in the source. The decision nodes are traced to the same FR at the **activity level**; the exact validation/persistence clauses still need to be checked against W1.

| Node | Kind | Text | Traces to |
|---|---|---|---|
| `m1` | Merge | Initial submission / corrected resubmission | Flow control; not a requirement action |
| `d1` | Decision | Required delivery information complete and valid? | FR-01-01 (inferred from linked actions; validate against W1) |
| `s2` | Action | Return input errors and request correction | FR-01-01 |
| `s1` | Action | Capture and store supplier identity, raw material type, quantity and declared carbon data | FR-01-01 |
| `d2` | Decision | Delivery record stored successfully? | FR-01-01 (inferred from linked actions; validate against W1) |
| `s3` | Action | Report that submission was not saved | FR-01-01 |
| `s4` | Action | Confirm successful submission | FR-01-01 |

## 3. Branches and acceptance tests

One row for each outgoing decision branch; the `[yes]` branch of `d1` is separated from the two possible outcomes at `d2`. These are **proposed acceptance scenarios** derived from the supplied flow, not confirmed implemented tests.

| Branch | Given | When | Then |
|---|---|---|---|
| `d1 [no]` | A supplier delivery submission is missing a required field or includes an invalid value | Procurement submits the delivery information | EcoTrace returns input errors, asks for correction, and the Procurement Department can correct and resubmit; the storage action is not reached on this attempt. |
| `d1 [yes]` | All required delivery information is complete and valid | Procurement submits the delivery information | EcoTrace attempts to capture and store supplier identity, material type, quantity, and declared carbon data; the flow proceeds to `d2`. |
| `d2 [no]` | Valid information was submitted, but the record was not saved successfully | EcoTrace evaluates the storage outcome | EcoTrace reports that the submission was not saved; Procurement reviews the unsuccessful outcome and the flow ends. |
| `d2 [yes]` | Valid information was submitted and the delivery record was stored successfully | EcoTrace evaluates the storage outcome | EcoTrace confirms submission success; Procurement reviews the confirmation and the flow ends. |

## 4. Object states used

The supplied diagram does **not** show an explicit object node such as `DeliveryRecord [Stored]`. “Stored successfully” and “not saved” are **branch outcomes**, not approved enumeration values for a record's state field.

| Object | States that appear in this diagram | Full state list (all diagrams) |
|---|---|---|
| Supplier delivery record | No explicit UML object states; successful/unsuccessful storage outcomes only | **TBD** — confirm with data model and other diagrams |

## 5. Screens touched

Screen IDs cannot be assigned from the supplied AD alone. To be filled from the Lesson 8 Screen List.

| Procurement Department action / outcome | Screen ID |
|---|---|
| Submit supplier delivery information | - |
| Correct missing or invalid delivery information | - |
| Review successful submission outcome | - |
| Review unsuccessful submission outcome | - |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Which delivery fields are mandatory, and what are the validation rules for quantities, material types and declared carbon data?] | Yes — for precise acceptance tests | No formal validation rules assumed beyond the `d1` check. |
| 2 | [NEEDS CLARIFICATION: Does FR-01-01 explicitly require both correction feedback and persistence-failure feedback, or should these trace to separate FRs?] | Yes — for traceability sign-off | Keep source FR-01-01 references until W1 is checked. |
| 3 | [NEEDS CLARIFICATION: Can Procurement retry a save failure, and how are duplicate submissions prevented?] | No — for documenting the **current** flow; yes if retry/idempotency is a required scenario | Current diagram ends on storage failure; no retry or deduplication rule is assumed. |
| 4 | [NEEDS CLARIFICATION: What are the official Feature ID, actor authentication/access preconditions, BR IDs and Screen IDs?] | Yes — for baseline consistency | Leave fields **TBD** pending source documents. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live (Mermaid 11.16+) | Done | — | — |
| Every decision has guards on all outgoing flows | Pass (static): `d1` and `d2` each have `[yes]` / `[no]` | Draft review | 2026-11-05 |
| Every fork has a matching join | N/A — no fork nodes | Draft review | 2026-11-05 |
| Every system action cites an FR | Pass (static): 4 of 4 EcoTrace actions | Draft review | 2026-11-05 |
| Every FR of UC-01 appears in the diagram | **Pending** — only FR-01-01 is cited; W1/UC baseline not supplied | — | — |
| Lane names match the Context Diagram | **Pending** — Context Diagram not supplied | — | — |
| AI critic prompt run; findings resolved or logged | **Not run**; documentation uncertainties listed in Section 6 | — | — |
