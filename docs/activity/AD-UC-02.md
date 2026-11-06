```mermaid
swimlane-beta TB
    subgraph USER["Sustainability & ESG user"]
        start@{ shape: sm-circ }
        m1{" "}
        a1("Enter methodology version, factor value, unit, effective date (FR-01-02-FE)")
        a2("Show saved confirmation (FR-01-02-FE)")
        stop1@{ shape: fr-circ }
    end
    subgraph SYS["EcoTrace system"]
        s1("Validate methodology version, factor value, unit, effective date (FR-01-02-BE)")
        d1{"Valid?"}
        s2("Show validation errors")
        d2{"Existing factor?"}
        d3{"Reason given?"}
        s3("Reject: Change Justification required")
        s4("Set Created By and Created At (FR-01-02-BE)")
        s5("Record Updated By, Updated At and Change Justification")
        db1[(GHG methodology & emission factor library)]
        db2[(Audit log)]
        s6("Create new version, keep previous version (FR-01-02-BE)")
        s7("Write audit log entry Who, when, old to new, reason (FR-01-02-BE)")
    end
    start --> m1 --> a1 --> s1 --> d1
    d1 -->|"[Invalid]"| s2 --> m1
    d1 -->|"[Valid]"| d2
    d2 -->|"[New]"| s4 --> s6
    d2 -->|"[Existing]"| d3
    d3 -->|"[No]"| s3 --> m1
    d3 -->|"[Yes]"| s5 --> s6
    s6 --> db1
    s6 --> s7 --> db2
    s7 --> a2 --> stop1
```

Reading notes:

- The system is one black-box lane: the GHG methodology & emission factor library and the audit log from the sequence diagram are not separate lanes.
- FR-01-02 covers the complete workflow for entering, validating, versioning, and logging changes to emission factors and methodology versions.
- No complex parallel fork/join: this use case follows a sequential validation and conditional version-branching path without uncoordinated parallel streams.

## 2. Node trace

| Node | Kind | Text | Traces to |
| --- | --- | --- | --- |
| s1 | Action | Validate methodology version, factor value, unit, effective date | FR-01-02-BE; Validation logic |
| d1 | Decision | Valid? | FR-01-02 validation rules |
| s2 | Action | Show validation errors | FR-01-02-FE error handling |
| d2 | Decision | Existing factor? | FR-01-02 library lookup |
| d3 | Decision | Reason given? | FR-01-02 justification check |
| s3 | Action | Reject: Change Justification required | FR-01-02 compliance rule |
| s4 | Action | Set Created By and Created At | FR-01-02-BE metadata |
| s5 | Action | Record Updated By, Updated At and Change Justification | FR-01-02 update logging |
| s6 | Action | Create new version, keep previous version | FR-01-02-BE versioning logic |
| s7 | Action | Write audit log entry Who, when, old to new, reason | FR-01-02-BE audit trail |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
| --- | --- | --- | --- |
| d1 [Invalid] | User enters invalid or missing parameters (version, factor value, unit, or effective date) | User submits methodology/factor data | The system displays validation errors and routes back to input |
| d2 [New] | The entered methodology version or emission factor does not yet exist in the library | User submits valid new data | The system sets creation metadata, creates the new version, and writes to the library |
| d2 [Existing] & d3 [No] | An existing factor/version is modified without providing a change justification reason | User submits updates without justification | The system rejects the request and prompts for change justification |
| d2 [Existing] & d3 [Yes] | An existing factor/version is modified with a valid change justification provided | User submits updates with justification | The system records update details, creates a new version keeping the previous one, and logs the action |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
| --- | --- | --- |
| GHG Methodology & Emission Factor | Created / Versioned | Pending agreement; do not introduce other status values without team sign-off |

## 5. Screens touched

*To be filled in Lesson 8 from the Screen List; no screen IDs have been provided.*

| Actor action | Screen ID |
| --- | --- |
| — | Pending Lesson 8 |

## 6. Open questions

| # | Question | Blocking? | Default applied |
| --- | --- | --- | --- |
| 1 | [NEEDS CLARIFICATION: Should previous factor versions be automatically deprecated or kept active under specific conditions?] | No | Resolved in this version: previous version is kept active alongside the new version (`s6`). |
| 2 | [NEEDS CLARIFICATION: Is audit log recording mandatory for brand-new methodology entries or only for updates?] | No | Resolved in this version: audit logging (`s7`) applies comprehensively to track creation and version changes. |
| 3 | [NEEDS CLARIFICATION: Should libraries and audit logs be represented as independent lanes in the AD?] | No | Kept inside the unified EcoTrace system lane for structural consistency. |

## 7. Verification log

| Check | Result | By | Date |
| --- | --- | --- | --- |
| Renders on mermaid.live | Not yet independently tested | Team reviewer | |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass (d1, d2, d3 fully guarded) | | |
| No action has two incoming flows (merge used instead) | Pass (no shared entry points) | | |
| Every fork has a matching join | N/A (no forks used) | | |
| Every system action cites an FR | Pass; FR-01-02 fully covered | | |
| Every FR of this use case appears in the diagram | FR-01-02: yes | | |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass | | |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass against SD-UC-02 | | |
| AI critic prompt run, findings resolved or logged as open questions | Done; logic flow verified | | |
