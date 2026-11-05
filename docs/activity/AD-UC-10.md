# Activity Diagram: AD-UC-10 Authenticate and sign in to the platform

| Field | Value |
| --- | --- |
| AD ID | AD-UC-10 |
| Use case | UC-10 Authenticate and sign in to the platform |
| Feature | F-04 Platform, Access & Operations |
| Primary actor | All users |
| Lanes | All users · HPDQ Carbon Management & Reporting system |
| Inputs (L3-L5) | Context Diagram (provided, version not stated) · Use Case Diagram (provided, version not stated) · Sequence: SD-UC-10 (provided image) · W0 Table 1 · W1 FR-04-01; shared authorization FR-04-03 (UC-11), audit-related FR-04-04 (UC-12) |
| Trigger | A platform user supplies credentials (or OTP) to sign in. |
| Preconditions | The user has an account with a configured authentication method and any applicable role assignments. |
| Success end state | The user is authenticated and granted a secure session with appropriate authorized access. |
| Failure end state(s) | An invalid credential does not produce a session, or a request for protected access is denied due to insufficient permission. |
| FRs covered | FR-04-01, FR-04-03 |
| Business rules applied | FR-04-01 valid-credential condition; FR-04-03 RBAC gate for protected resources. No separate BR IDs supplied. |
| Version / status | v0.1 · Draft (source reconciliation pending) |
| Author | EcoTrace Group G · individual reviewer/owner to assign |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph ACT["All users"]
    start@{ shape: sm-circ }
    a1("Provide credentials or OTP")
    a2("Review rejected sign-in")
    a3("Review access denial")
    a4("Access authorized platform functions")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
    stop3@{ shape: fr-circ }
  end
  subgraph SYS["HPDQ Carbon Management & Reporting system"]
    s1("Verify user identity (FR-04-01)")
    d1{"Are credentials valid? (FR-04-01)"}
    s2("Reject authentication and deny session (FR-04-01)")
    s3("Read access roles and permissions (FR-04-03)")
    d2{"Is requested access authorized? (FR-04-03)"}
    s4("Deny unauthorized protected access (FR-04-03)")
    s5("Grant secure session with authorized access (FR-04-01)")
  end
  start --> a1 --> s1 --> d1
  d1 -->|"[invalid]"| s2 --> a2 --> stop2
  d1 -->|"[valid]"| s3 --> d2
  d2 -->|"[not authorized]"| s4 --> a3 --> stop3
  d2 -->|"[authorized]"| s5 --> a4 --> stop1
```

Reading notes:

- SD-UC-10 loads RBAC roles after authentication, so FR-04-03 (Must; W1 traces it to UC-11) is included only as a cross-use-case access gate, not as a new role-configuration activity. UC-11 remains separate.
- The sequence also writes both successful and failed sign-in audit events. W1 FR-04-04 only specifies logging system-altering actions, so login logging cannot honestly be claimed covered by FR-04-04. It is explicitly an open question, not added as an untraced system action.
- FR-04-01 EARS describes valid credentials, while the invalid-credential failure branch is supported by the provided SD-UC-10 alt. An explicit IF requirement should be added for complete W1 coverage (Q2).

## 2. Node trace

| Node | Kind | Text | Traces to |
| --- | --- | --- | --- |
| s1 | Action | Verify user identity | FR-04-01; SD-UC-10 steps 3–4 |
| d1 | Decision | Are credentials valid? | FR-04-01; SD-UC-10 invalid-credentials alt |
| s2 | Action | Reject authentication and deny session | FR-04-01 (failure-side extension from SD; Q2) |
| s3 | Action | Read roles/permissions | FR-04-03 (shared UC-11 authorization rule) |
| d2 | Decision | Is requested protected access authorized? | FR-04-03 |
| s4 | Action | Deny unauthorized protected access | FR-04-03 |
| s5 | Action | Grant secure session | FR-04-01 |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
| --- | --- | --- | --- |
| d1 [invalid] | An account exists but the submitted password is wrong | User attempts to sign in | No login session is granted and authentication fails; required display/audit policy remains Q1–Q2 |
| d1 [valid] | A user supplies a correct credential with a configured role | User signs in | The role/permission record is assessed before a protected session is granted or access is refused |
| d2 [not authorized] | Authentication succeeded but the account lacks permission for the protected platform resource | User requests protected access | Access to that resource is denied (FR-04-03); fine-grained session policy awaits Q3 |
| d2 [authorized] | Authentication succeeded and RBAC grants the requested protected access | User requests protected access | A secure authorized session is available (FR-04-01; FR-04-03) |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| — | No formal object-state transition specified by the supplied W1/sequence | Pending agreement; do not introduce made-up status enum values |

## 5. Screens touched

*To be filled in Lesson 8 from the Screen List; no screen IDs have been provided.*

| Actor action | Screen ID |
|---|---|
| — | Pending Lesson 8 |

## 6. Open questions

| # | Question | Blocking? | Default applied |
| --- | --- | --- | --- |
| 1 | [NEEDS CLARIFICATION: Should login successes and failed login attempts always produce immutable audit events? W1 FR-04-04 is only triggered by system-altering actions.] | Yes | Audit steps from SD-UC-10 not added as system actions until a dedicated EARS requirement is added or FR-04-04 is expanded. |
| 2 | [NEEDS CLARIFICATION: What are the exact rejected-credentials error and retry/lockout rules? W1 FR-04-01 only defines the valid-credentials WHEN case.] | Yes | Diagram follows the invalid-credentials alt in SD-UC-10 without inventing retry thresholds. |
| 3 | [NEEDS CLARIFICATION: Can authenticated users with no authorized resource still receive a sign-in session, or is session issuance conditional on authorization?] | Yes | Diagram follows SD-UC-10 authorization context before session creation; needs client confirmation. |
| 4 | [NEEDS CLARIFICATION: Is OTP locally verified or provided by an external OTP/identity service missing from the Context Diagram?] | No | No third-party identity-provider lane was invented. |

## 7. Verification log

| Check | Result | By | Date |
| --- | --- | --- | --- |
| Renders on mermaid.live | Not yet independently tested in mermaid.live | Team reviewer | 2026-10-26 |
| Every decision has guards on all outgoing flows | Static pass (or N/A if no decisions) | Assistant | 2026-10-26 |
| No action has two incoming flows without merge | Static pass | Assistant | 2026-10-26 |
| Every fork has a matching join | N/A (no forks) | Assistant | 2026-10-26 |
| Every system action cites an FR | Static pass; provisional W1 trace exceptions detailed in section 6 | Assistant | 2026-10-26 |
| Every FR of this use case appears in the diagram | 1 of 1 direct UC-10 Must FR (FR-04-01); plus 1 shared Must FR-04-03 (from UC-11) | Assistant | 2026-10-26 |
| Lane names match Context Diagram/use case, no internal components as lanes | Pass for declared actors and system; upstream source inconsistencies logged | Assistant | 2026-10-26 |
| Sequence names match; its alt fragments appear as decisions | Reviewed against supplied image; differences logged in section 6 | Assistant | 2026-10-26 |
| AI critic prompt run; findings resolved or logged | Checked d1 and d2 each have 2 exhaustive opposing guards; no missing merges, no fork, no UI detail. Audit and session-order inconsistencies logged. | Assistant | 2026-10-26 |
| Peer-review by another team | Pending | Peer reviewer | — |

---

*Template source: DBIZ2 Lesson 7 (VJCBI College, FTU); notation: `mermaid-activity-patterns.md`. This activity diagram models one use case as a black box. Internal databases, UI, services and APIs from the sequence diagram are intentionally not independent lanes.*
