# Activity Diagram: AD-UC-04 Calculate shipment-level PCF

| Field | Value |
| --- | --- |
| AD ID | AD-UC-04 |
| Use case | UC-04 Calculate shipment-level PCF |
| Feature | F-01 Carbon Traceability & Product Carbon Footprint Management |
| Primary actor | Export Sales & Marketing |
| Lanes | Export Sales & Marketing · EcoTrace · Logistics Partner (Hoa Phat Logistics) |
| Inputs (L3-L5) | Context Diagram (provided) · Use Case Diagram (provided) · Sequence: SD-UC-04 (provided image) · W0 Table 1 · W1 FR-01-05 (cross-reference: FR-01-07, FR-01-09) |
| Trigger | An Export Sales & Marketing user selects a shipment and requests its PCF. |
| Preconditions | Access is authorized under the common platform rules (UC-10/UC-11). |
| Success end state | The shipment-level PCF has been calculated from constituent-batch carbon intensities and applicable logistics emissions, stored, and shown to the requester. |
| Failure end state | No PCF is calculated; the system reports that required shipment, batch-intensity, or linkage data is missing. |
| FRs covered | FR-01-05, FR-01-09 (new — reported to integrator, not yet in W1) |
| Business rules applied | None supplied for UC-04. |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph ACT["Export Sales & Marketing"]
    start@{ shape: sm-circ }
    a1("Select shipment for PCF calculation")
    a2("See missing-data notice")
    a3("Review shipment-level PCF")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  subgraph SYS["EcoTrace"]
    s1("Retrieve shipment and batch references from SAP (FR-01-05)")
    s2("Retrieve constituent-batch carbon intensities (FR-01-05)")
    d1{"Shipment, batch intensities and linkage all available?"}
    s3("Flag missing shipment or batch data (FR-01-09)")
    s4("Request shipment logistics emissions (FR-01-05)")
    s5("Calculate shipment-level PCF from batch intensity and logistics emissions (FR-01-05)")
    o1["Shipment PCF [Calculated]"]
    s6("Store shipment-level PCF (FR-01-05)")
  end
  subgraph LOG["Logistics Partner (Hoa Phat Logistics)"]
    e1("Provide shipment logistics emissions")
  end
  start --> a1 --> s1 --> s2 --> d1
  d1 -->|"[no]"| s3 --> a2 --> stop2
  d1 -->|"[yes]"| s4 --> e1 --> s5 --> o1 --> s6 --> a3 --> stop1
```

Reading notes:

- The system is one black-box lane: SAP, the carbon-data/traceability service, and the PCF database
  from the sequence diagram are not separate lanes.
- `FR-01-09` does not exist in W1 yet. It is an open item to report to the integrator (see section 6),
  not an invented requirement — do not treat it as confirmed until it is added.
- No fork/join: unlike AD-UC-01, this use case has no two steps that genuinely happen in any order,
  so a fork would add nothing but noise.

## 2. Node trace

| Node | Kind | Text | Traces to |
| --- | --- | --- | --- |
| s1 | Action | Retrieve shipment and batch references from SAP | FR-01-05; SD-UC-04 steps 3-4 |
| s2 | Action | Retrieve constituent-batch carbon intensities | FR-01-05; SD-UC-04 steps 5-6 |
| d1 | Decision | Shipment, batch intensities and linkage all available? | FR-01-09 (new) |
| s3 | Action | Flag missing shipment or batch data | FR-01-09 (new) |
| s4 | Action | Request shipment logistics emissions | FR-01-05; SD-UC-04 step 7 |
| e1 | Action (external) | Provide shipment logistics emissions | SD-UC-04 step 8 |
| s5 | Action | Calculate shipment-level PCF from batch intensity and logistics emissions | FR-01-05; SD-UC-04 step 9 |
| o1 | Object node | Shipment PCF [Calculated] | FR-01-05 (state value) |
| s6 | Action | Store shipment-level PCF | FR-01-05; SD-UC-04 step 10 |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
| --- | --- | --- | --- |
| d1 [yes] | Shipment SH-001 has batches B-01, B-02 with known carbon intensity and a known batch-to-shipment linkage | Export Sales selects SH-001 for PCF calculation | The system calculates, stores, and displays the shipment-level PCF |
| d1 [no] | Shipment SH-002 has a batch with no recorded carbon intensity | Export Sales selects SH-002 for PCF calculation | No PCF is calculated; the user sees a missing-data notice naming the gap |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Shipment PCF | Calculated | Pending agreement; do not introduce other status values without team sign-off |

## 5. Screens touched

*To be filled in Lesson 8 from the Screen List; no screen IDs have been provided.*

| Actor action | Screen ID |
|---|---|
| — | Pending Lesson 8 |

## 6. Open questions

| # | Question | Blocking? | Default applied |
| --- | --- | --- | --- |
| 1 | [NEEDS CLARIFICATION: What should EcoTrace do if shipment-to-batch linkage, batch intensity, or logistics emissions are missing or invalid?] | No | Resolved in this version: decision `d1` added, routed to a new NC function `FR-01-09` reported to the integrator for W1. |
| 2 | [NEEDS CLARIFICATION: Does FR-01-05 require permanent PCF record storage?] | No | Resolved in this version: persistence step `s6` added under FR-01-05, matching SD-UC-04 step 10. |
| 3 | [NEEDS CLARIFICATION: Should SAP S/4HANA be listed as an external system in the Context Diagram, or is it intentionally represented by Internal Departments?] | No | SAP remains a named source in a system action, not an extra AD lane. |

## 7. Verification log

| Check | Result | By | Date |
| --- | --- | --- | --- |
| Renders on mermaid.live | Not yet independently tested | Team reviewer | |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass (d1: [yes]/[no]) | | |
| No action has two incoming flows (merge used instead) | Pass (no shared entry points) | | |
| Every fork has a matching join | N/A (no forks) | | |
| Every system action cites an FR | Pass; FR-01-09 flagged as new, pending W1 | | |
| Every FR of this use case appears in the diagram | FR-01-05: yes. FR-01-09: new, to be added to W1 | | |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass | | |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass against SD-UC-04 | | |
| AI critic prompt run, findings resolved or logged as open questions | Done; 2 prior open questions resolved, 1 remains informational | | |

---

*Template source: DBIZ2 Lesson 7 (VJCBI College, FTU); notation: `mermaid-activity-patterns.md`. This activity diagram models one use case as a black box. Internal databases, UI, services and APIs from the sequence diagram are intentionally not independent lanes.*
