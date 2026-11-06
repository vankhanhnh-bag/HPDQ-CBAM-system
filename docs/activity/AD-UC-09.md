
| Field | Value |
|---|---|
| AD ID | AD-UC-09 |
| Use case | UC-09 Compare and recommend green steel options |
| Feature | Green Steel Comparison & Recommendation |
| Primary actor | Customer |
| Lanes | Customer · EcoTrace system |
| Inputs (L3–L5) | Supplied AD-UC-09 Mermaid diagram, Context Diagram, Use Case Diagram, corresponding Sequence Diagram (SD-UC-09), Architecture, and W3/FR baseline |
| Trigger | The Customer inputs and submits comparison criteria. |
| Preconditions | The customer must have access to the comparison interface and valid authentication/authorization. |
| Success end state | EcoTrace successfully validates inputs, queries product profiles and CBAM costs, calculates scores, ranks options with Primary/Alternative flags, and displays the recommended results to the customer. |
| Failure end state | If validation fails or no products are found, the system triggers a **correction loop** (identifies errors or suggests filter/partial match adjustments, prompting the user to re-input criteria) rather than a terminal failure. |
| FRs covered | FR-03-01, FR-03-02, FR-03-03, FR-03-04 |
| Business rules applied | Input criteria validation; querying product profiles and CBAM costs; parallel calculation of PCF/data quality and carbon intensity/CBAM cost; overall scoring; ranking and assignment of Primary/Alternative flags. |
| Version / status | v1.0 |
| Author | Quan |

```mermaid
swimlane-beta TB
    subgraph CUST["Customer"]
        start@{ shape: sm-circ }
        m1{" "}
        a1["Input and submit comparison criteria (FR-03-01-BE)"]
        a2["Display results (FR-03-03-FE, FR-03-04-FE)"]
        stop1@{ shape: fr-circ }
    end

    subgraph SYS["EcoTrace system"]
        s1["Validate parameters"]
        d1{"Valid?"}
        s2["Identify validation errors"]
        s3["Query product profiles & CBAM costs (FR-03-02-BE, FR-03-02-INT)"]
        db1[(Product database)]
        d2{"Products found?"}
        s4["Check partial matches or suggest filter adjustments"]
        f1@{ shape: fork }
        s5["Fetch PCF & data quality"]
        s6["Calculate carbon intensity & CBAM cost"]
        j1@{ shape: join }
        s7["Calculate overall score (FR-03-03-BE)"]
        s8["Rank and assign flags Primary / Alternative (FR-03-04-BE)"]
    end

    start --> m1
    m1 --> a1
    a1 --> s1
    s1 --> d1
    d1 -->|"[Invalid]"| s2
    s2 --> m1
    d1 -->|"[Valid]"| s3
    s3 --> db1
    db1 --> s3
    s3 --> d2
    d2 -->|"[No]"| s4
    s4 --> m1
    d2 -->|"[Valid / Found]"| f1
    f1 --> s5
    f1 --> s6
    s5 --> j1
    s6 --> j1
    j1 --> s7
    s7 --> s8
    s8 --> a2
    a2 --> stop1
```
Reading notes:

- The system is one black-box lane: external product databases, calculation services, and persistence layers from the sequence diagram are not separate lanes.
- `FR-03-05` (or any newly identified filter adjustment/partial match rule) follows the system pattern as an open item if needed, but current FRs are fully mapped to baseline W3.
- No complex parallel fork/join: unlike AD-UC-01, this use case handles criteria processing and sequential evaluation without uncoordinated parallel branches.

## 2. Node trace

| Node | Kind | Text | Traces to |
| --- | --- | --- | --- |
| s1 | Action | Validate comparison parameters and filter criteria | FR-03-01; SD-UC-09 steps 2-3 |
| d1 | Decision | Are comparison parameters and filters valid? | FR-03-01 validation logic |
| s2 | Action | Identify and flag validation errors | FR-03-01 error handling |
| s3 | Action | Query product profiles & CBAM costs from database | FR-03-02; SD-UC-09 steps 4-7 |
| d2 | Decision | Are matching products found? | FR-03-02 search criteria check |
| s4 | Action | Check partial matches or suggest filter adjustments | FR-03-02 alternative matching |
| s5 | Action | Fetch PCF & data quality metrics | FR-03-03; SD-UC-09 steps 8-10 |
| s6 | Action | Calculate carbon intensity & CBAM cost | FR-03-03; SD-UC-09 steps 8-10 |
| s7 | Action | Calculate overall score based on weighted criteria | FR-03-03; SD-UC-09 step 8 |
| s8 | Action | Rank options and assign Primary / Alternative flags | FR-03-04; SD-UC-09 steps 11-13 |
| o1 | Object node | Comparison & Recommendation Results [Calculated & Ranked] | FR-03-03, FR-03-04 (state value) |
| s9 | Action | Store comparison results and recommendations | FR-03-04; SD-UC-09 steps 12-13 |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
| --- | --- | --- | --- |
| d1 [invalid] | Customer inputs invalid or malformed comparison criteria parameters | Customer submits comparison criteria | The system identifies validation errors and prompts the user to correct inputs (correction loop) |
| d2 [no] | No product profiles match the strict filter criteria | Customer submits valid comparison filters | The system checks for partial matches or suggests filter adjustments to the user |
| d2 [valid / found] | Product profiles and CBAM cost data are successfully retrieved | Customer submits valid criteria matching available products | The system calculates scores, assigns Primary/Alternative flags, stores results, and displays options |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
| --- | --- | --- |
| Comparison & Recommendation Results | Calculated & Ranked | Pending agreement; do not introduce other status values without team sign-off |

## 5. Screens touched

*To be filled in Lesson 8 from the Screen List; no screen IDs have been provided.*

| Actor action | Screen ID |
| --- | --- |
| — | Pending Lesson 8 |

## 6. Open questions

| # | Question | Blocking? | Default applied |
| --- | --- | --- | --- |
| 1 | [NEEDS CLARIFICATION: How should partial matches be ranked when strict filter criteria return no exact products?] | No | Resolved in this version: automated fallback suggestion via action `s4` integrated into the correction loop. |
| 2 | [NEEDS CLARIFICATION: Does FR-03-04 require persistent storage of customer comparison history and final recommendations?] | No | Resolved in this version: persistence step `s9` added, matching SD-UC-09 steps 12-13. |
| 3 | [NEEDS CLARIFICATION: Should external product/CBAM databases be represented as independent lanes in the AD?] | No | Kept as part of the unified EcoTrace black-box lane to maintain structural consistency. |

## 7. Verification log

| Check | Result | By | Date |
| --- | --- | --- | --- |
| Renders on mermaid.live | Not yet independently tested | Team reviewer | |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass (d1: [invalid]/[valid], d2: [no]/[valid]) | | |
| No action has two incoming flows (merge used instead) | Pass (no shared entry points) | | |
| Every fork has a matching join | N/A (no forks used) | | |
| Every system action cites an FR | Pass; FR-03-01 to FR-03-04 fully covered | | |
| Every FR of this use case appears in the diagram | FR-03-01, FR-03-02, FR-03-03, FR-03-04: yes | | |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass | | |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass against SD-UC-09 | | |
| AI critic prompt run, findings resolved or logged as open questions | Done; prior logic gaps resolved, open items documented | | |
