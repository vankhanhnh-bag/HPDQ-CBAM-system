# Activity Diagram: AD-UC-11 Configure role-based access control (RBAC)

| Field | Value |
|---|---|
| AD ID | AD-UC-11 |
| Use case | UC-11 Configure role-based access control (RBAC) |
| Feature | RBAC configuration / access-control administration |
| Primary actor | System Admin |
| Lanes | System Admin · EcoTrace |
| Inputs (L3–L5) | Supplied AD-UC-11 diagram; **TBD:** Context Diagram, Use Case Diagram, corresponding Sequence Diagram, Architecture, and W1/FR baseline |
| Trigger | A System Admin requests access to RBAC configuration. |
| Preconditions | An actor attempts the admin RBAC workflow |
| Success end state | RBAC access-control policies are updated, an immutable RBAC-change audit event is recorded, and the System Admin reviews the updated policies. |
| Failure end state | If the requester lacks RBAC configuration permission, EcoTrace denies configuration access; the update and change-audit actions are not reached. |
| FRs covered | FR-04-02 · FR-04-03 · FR-04-04 |
| Business rules applied | Only authorized principals may configure RBAC (expressed by FR-04-03); successful changes must be audited (expressed by FR-04-04). Formal BR IDs: **TBD**. |
| Version / status | v0.1 |
| Author | Huyen Khanh |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). The diagram uses `swimlane-beta`, requiring Mermaid 11.16 or later.

```mermaid
swimlane-beta TB
  subgraph ACT["System Admin"]
    start@{ shape: sm-circ }
    a1("Request RBAC configuration access")
    a2("Define role and permission assignments")
    a3("Submit role configuration")
    a4("Review updated access policies")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  subgraph SYS["EcoTrace"]
    s1("Verify admin RBAC permissions (FR-04-03)")
    d1{"Authorized to configure RBAC? (FR-04-03)"}
    s2("Deny unauthorized configuration access (FR-04-03)")
    s3("Update RBAC access-control policies (FR-04-02)")
    s4("Record immutable RBAC change audit event (FR-04-04)")
  end
  start --> a1 --> s1 --> d1
  d1 -->|"[not authorized]"| s2 --> stop2
  d1 -->|"[authorized]"| a2 --> a3 --> s3 --> s4 --> a4 --> stop1
```

Reading notes:

- `s1` checks the requester's permissions. Decision `d1` has two guarded outcomes: `[authorized]` and `[not authorized]`.
- On the **authorized** path, the System Admin defines and submits the configuration; EcoTrace updates access-control policies (`s3`) **before** recording the immutable change audit event (`s4`), and the actor then reviews the updated policies.
- On the **not authorized** path, EcoTrace denies access (`s2`) and reaches `stop2` without executing a policy update. The source does not model a separate actor-lane action for reviewing the denial.
- No validation decision, failed-update branch, or failed-audit branch is shown. These are gaps to clarify, **not** additional behaviors already covered by the diagram.

## 2. Node trace

Every EcoTrace action and the authorization decision cite an FR in the supplied code. The mapping below is limited to those explicit references.

| Node | Kind | Text | Traces to |
|---|---|---|---|
| `s1` | Action | Verify admin RBAC permissions | FR-04-03 |
| `d1` | Decision | Authorized to configure RBAC? | FR-04-03 |
| `s2` | Action | Deny unauthorized configuration access | FR-04-03 |
| `s3` | Action | Update RBAC access-control policies | FR-04-02 |
| `s4` | Action | Record immutable RBAC change audit event | FR-04-04 |

## 3. Branches and acceptance tests

One row for each outgoing branch of `d1`. The scenarios are **proposed acceptance tests** based on the supplied activity flow.

| Branch | Given | When | Then |
|---|---|---|---|
| `d1 [not authorized]` | A requester does not have permission to configure RBAC | The requester requests RBAC configuration access | EcoTrace denies configuration access; no role/permission configuration is submitted through this flow, and no RBAC policy update occurs. |
| `d1 [authorized]` | A System Admin has permission to configure RBAC and has defined role/permission assignments | The admin submits the role configuration | EcoTrace updates the RBAC policy and records an immutable RBAC-change audit event; the admin can review the updated policies. |

## 4. Object states used

The supplied diagram does **not** include explicit UML object-state nodes. “Updated” describes the result of an action, while “immutable” is a property of the audit event, **not** a state enumeration.

| Object | States that appear in this diagram | Full state list (all diagrams) |
|---|---|---|
| RBAC access-control policy | No explicit state; the diagram performs a policy update | - |
| RBAC change audit event | No explicit state; event creation is shown by action `s4` | - |

## 5. Screens touched

Screen IDs cannot be derived from the supplied AD alone. To be filled from the Lesson 8 Screen List.

| System Admin action / outcome | Screen ID |
|---|---|
| Request RBAC configuration access | - |
| Define role and permission assignments | - |
| Submit role configuration | - |
| Review updated access policies | -|
| Access denied (`d1 [not authorized]`) | - |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: What is the approved UC-11 title and its Feature ID/name?] | Yes — for traceability sign-off | Use provisional label “Configure role-based access control (RBAC)” only in this document. |
| 2 | [NEEDS CLARIFICATION: How are invalid role-permission assignments, conflicting permissions, and a failed policy update handled?] | Yes — if the UC must cover unsuccessful configuration beyond unauthorized access | Current diagram contains only the authorization failure branch; no extra validation or rollback behavior assumed. |
| 3 | [NEEDS CLARIFICATION: If audit-event recording fails after the policy update, should the change roll back, be queued for retry, or remain applied?] | Yes — potential integrity/compliance issue | No recovery/rollback behavior is assumed; the present diagram proceeds sequentially from `s3` to `s4`. |
| 4 | [NEEDS CLARIFICATION: Should unauthorized access attempts create a separate security audit log, and how is the denial displayed to the actor?] | No — for the current depicted success/denial paths; may be required by security baseline | No change-audit event or explicit denial screen action is shown on the denied path. |
| 5 | [NEEDS CLARIFICATION: What are the official BR IDs, preconditions, and Screen IDs, and does W1 define any other FRs for UC-11?] | Yes — for complete cross-artifact verification | Leave unspecified items **TBD** until the team's baseline is reviewed. |

## 7. Verification log

“Pass (static)” means a text-level inspection of the supplied Mermaid source, **not** a verified render or a team acceptance review.

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live (Mermaid 11.16+) | Done | — | — |
| Every decision has guards on all outgoing flows | Pass (static): `d1` has `[authorized]` and `[not authorized]` | Draft review | 2026-11-05 |
| Every fork has a matching join | N/A — no fork nodes | Draft review | 2026-11-05 |
| Every system action cites an FR | Pass (static): 4 of 4 EcoTrace actions | Draft review | 2026-11-05 |
| Every FR of UC-11 appears in the diagram | **Pending** — three FRs cited; W1/UC baseline not supplied | — | — |
| Lane names match the Context Diagram | **Pending** — Context Diagram not supplied | — | — |
| AI critic prompt run; findings resolved or logged | **Not run**; documentation uncertainties listed in Section 6 | — | — |
