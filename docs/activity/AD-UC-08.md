# Activity Diagram: AD-UC-08 Manage CBAM regulatory rules & parameters

| Field | Value |
|---|---|
| AD ID | `AD-UC-08` |
| Use case | UC-08 Manage CBAM regulatory rules & parameters |
| Feature | F-02 CBAM Compliance & Carbon Cost Estimation |
| Primary actor | Compliance / Product Owner |
| Lanes | Compliance / Product Owner · HPDQ Carbon Management & Reporting system |
| Inputs (L3–L5) | EcoTrace Context Diagram; Use Case Diagram; W0 UC-08; W1 FR-02-14 through FR-02-17; shared Must FR-04-03 and FR-04-04; `docs/use-case/SD-UC-08.md` (available; cross-diagram alignment still requires review). |
| Trigger | Authorized Compliance / Product Owner requests CBAM regulatory configuration. |
| Preconditions | User has an authenticated session. Permission to read protected rule settings is checked at the start. Existing rules can be retrieved. |
| Success end state(s) | Existing configuration is viewed without change; or draft is saved; or validated version is activated with an effective date and previous versions preserved. |
| Failure end state(s) | Access denied without disclosing protected settings; or activation blocked with validation reasons, leaving the draft unactivated. |
| FRs covered | UC-08 Must: **FR-02-14, FR-02-15, FR-02-16, FR-02-17 (4/4)**; shared Must: **FR-04-03, FR-04-04**. |
| Business rules applied | Mandatory values, valid ranges, nonconflicting effective periods (FR-02-16); effective-date versioning, prior-version traceability (FR-02-17). No separately numbered BRs provided. |
| Version / status | v0.2 |
| Author | Huyền My |

## 1. Diagram

Below is the **short-label, separate-final** version. It preserves all Must logic. `swimlane-beta LR` places the partitions side by side; Mermaid may still place actions vertically inside each partition.

```mermaid
swimlane-beta TB
  subgraph ACT["Compliance / Product Owner"]
    start@{ shape: sm-circ }
    a1("Request CBAM rule settings")
    a2("Review rule configurations")
    d2{"Edit a rule set? (FR-02-15)"}
    a3("Edit parameters and sources")
    d3{"Request activation? (FR-02-17)"}
    a4("Review validation reasons")
    a5("Review activation result")
    endView@{ shape: fr-circ }
    endDraft@{ shape: fr-circ }
    endInvalid@{ shape: fr-circ }
    endSuccess@{ shape: fr-circ }
  end

  subgraph SYS["HPDQ Carbon Management & Reporting system"]
    s1("Check RBAC permission (FR-04-03)")
    d1{"Authorized to view? (FR-04-03)"}
    endDenied@{ shape: fr-circ }
    s2("Show active and scheduled rules (FR-02-14)")
    s3("Save rule-set draft (FR-02-15)")
    s4("Log draft change (FR-04-04)")
    s5("Validate draft for activation (FR-02-16)")
    d4{"Draft valid? (FR-02-16)"}
    s6("Block activation; explain errors (FR-02-16)")
    s7("Activate versioned rules (FR-02-17)")
    s8("Use applicable version for new estimates (FR-02-17)")
    s9("Log activation (FR-04-04)")
  end

  start --> a1 --> s1 --> d1
  d1 -->|"[unauthorized]"| endDenied
  d1 -->|"[authorized]"| s2 --> a2 --> d2
  d2 -->|"[view only]"| endView
  d2 -->|"[create or edit]"| a3 --> s3 --> s4 --> d3
  d3 -->|"[keep draft]"| endDraft
  d3 -->|"[activate]"| s5 --> d4
  d4 -->|"[invalid]"| s6 --> a4 --> endInvalid
  d4 -->|"[valid]"| s7 --> s8 --> s9 --> a5 --> endSuccess
```

**Reading notes**

- Five **separate activity finals** terminate the five possible outcomes right beside their branches. They are not five sequential steps and do not merge into one distant final node. This removes the long return arrows seen in the previous rendering.
- `d1` checks protected access, `d2` is the actor’s choice to view or maintain, `d3` is the actor’s activation choice, and `d4` is the system’s validation decision. All eight outgoing flows have mutually exclusive guards.
- Each action label is deliberately short; the Node Trace below preserves the full W1 meaning (parameter values, source references, effective dates, historical versions). No UI/API/database lanes are introduced.
- **Important:** The W1 wording does not establish separate role keys for view/edit/activate, the parameter-specific valid ranges, or how future-dated activation is scheduled. These remain open questions rather than invented behavior.

## 2. Node trace

| Node | Kind | Short label | Trace / full meaning |
|---|---|---|---|
| s1 | System action | Check RBAC permission | FR-04-03: verify access to protected configuration before viewing |
| d1 | Decision | Authorized to view? | FR-04-03: `[authorized]` / `[unauthorized]` |
| s2 | System action | Show active and scheduled rules | FR-02-14: display active/scheduled rule sets, parameters, effective dates, and source references |
| d2 | Actor choice | Edit a rule set? | FR-02-14 and FR-02-15: view only or create/edit a draft |
| s3 | System action | Save rule-set draft | FR-02-15: save changed regulatory parameters and source references as draft version |
| s4 | System action | Log draft change | FR-04-04: immutable record of completed change to draft |
| d3 | Actor choice | Request activation? | FR-02-15 and FR-02-17: keep saved draft or request activation |
| s5 | System action | Validate draft for activation | FR-02-16: check mandatory values, ranges, and conflicting effective periods |
| d4 | Decision | Draft valid? | FR-02-16: `[valid]` / `[invalid]` based on validation result |
| s6 | System action | Block activation; explain errors | FR-02-16: block activation and show the reason for each validation problem |
| s7 | System action | Activate versioned rules | FR-02-17: activate validated version with effective date and retain prior versions |
| s8 | System action | Use applicable version for new estimates | FR-02-17: select applicable version for **new** CBAM estimates according to its effective date (not backdate prior estimates) |
| s9 | System action | Log activation | FR-04-04: immutable record of completed activation |

## 3. Branches and acceptance tests

All values below are **test fixtures**, not actual regulatory thresholds or policy decisions.

| Branch | Given | When | Then |
|---|---|---|---|
| d1 `[unauthorized]` | User U1 lacks read permission for CBAM settings | U1 requests regulatory configuration | Protected settings are not shown; activity ends at `endDenied` |
| d1 `[authorized]` | User U2 has access to CBAM configuration | U2 requests regulatory configuration | EcoTrace shows active and scheduled rules including parameter values, dates and references |
| d2 `[view only]` | Rule set R-01 is available; U2 has read access | U2 decides not to change a rule set | Rule configuration remains unchanged; activity ends at `endView` |
| d2 `[create or edit]` | U2 can maintain a rule set; draft D-003 has parameter values and source references | U2 submits draft D-003 | Draft version is saved and change is logged; activity proceeds to activation choice |
| d3 `[keep draft]` | Draft D-003 has been saved | U2 elects not to activate it | D-003 stays a draft, not in active use; activity ends at `endDraft` |
| d3 `[activate]` | Draft D-003 has been saved | U2 requests activation | System performs activation validation before changing the applicable rule set |
| d4 `[invalid]` | Draft D-003 omits a required value, fails a configured range check, or conflicts on effective dates | System validates D-003 | Activation is blocked, reasons are shown, draft is not activated; activity ends at `endInvalid` |
| d4 `[valid]` | Draft D-003 meets all configured validation rules and has a nonconflicting sample effective date (e.g. 2027-01-01) | System validates D-003 | A version with the effective date is recorded; previous versions retained; new estimates use the version applicable to their date; activation is logged; activity ends at `endSuccess` |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| CBAM regulatory rule set | Draft, Active / future-effective (business meaning) | **Not finalized** as an enum. W1 refers to `active` and `scheduled` rule sets; confirm the exact statuses and transitions before defining data-model values. |
| CBAM rule-set change audit | Immutable event recorded | Event creation, not a separately defined state; no state enum specified in W1. |

## 5. Screens touched

*Screen IDs taken from the Lesson 8 Screen List; detailed Screen Specifications are not part of this update.*

| Actor action | Screen ID |
|---|---|
| Request and review CBAM rule settings | `S-F02-02` (CBAM Regulatory Settings) |
| Edit parameters and source references | `S-F02-03` (CBAM Regulatory Rule Set Editor) |
| Request activation and review validation/activation outcome | `S-F02-03` (validation and `M-F02-03` confirmation modal); `S-F02-02` (result/status) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Do view, edit and activate require different RBAC permission keys? If yes, add action-specific permission checks to the activity and W1.] | Yes | Only the documented entry permission check is shown; no detailed roles invented. |
| 2 | [NEEDS CLARIFICATION: Does activation with a future effective date create a Scheduled state first, and which date determines version applicability?] | Yes | Only the FR-02-17 requirement that applicable rules be used **from their effective dates** is shown; no scheduler/time trigger invented. |
| 3 | [NEEDS CLARIFICATION: What exact mandatory CBAM parameters, valid ranges, and effective-period conflict rules are enforced?] | Yes | Only categories of failure specified by FR-02-16 are shown. |
| 4 | [NEEDS CLARIFICATION: Should a rejected draft be editable/retried within the same invocation?] | No | Rejected activation terminates; user can start another invocation. No undocumented loop. |
| 5 | [NEEDS CLARIFICATION: Should unsuccessful authorization or activation attempts also be audited?] | No | Only completed system-altering actions are logged (FR-04-04). |
| 6 | [NEEDS CLARIFICATION: Should the actor name Compliance / Product Owner be harmonized with the aggregated Internal Departments entity in the Context Diagram?] | No | Use-case actor label is retained; EcoTrace system lane matches Context Diagram. |
| 7 | [NEEDS ALIGNMENT: Should AD-UC-08 mirror the action-specific edit and activation permission checks shown in SD-UC-08?] | Yes for AD/SD consistency | `SD-UC-08.md` is available; the activity currently models entry permission, while the sequence also checks edit and activation permissions. Team review is still required. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Mermaid Live render (11.16+) | **Pending manual render** (syntax checked statically) | Team | — |
| All decision guards exhaustive and disjoint | Static pass: 4 decisions, each 2 guarded outcomes | Assistant | 2026-10-26 |
| No actions with multiple incoming control flows | Static pass: separate final nodes eliminate long back-routing | Assistant | 2026-10-26 |
| Every fork has a matching join | N/A (no fork) | Assistant | 2026-10-26 |
| All system actions cite an existing Must FR | Static pass: 9 of 9 system actions | Assistant | 2026-10-26 |
| Every UC-08 Must FR appears | **4 of 4** (FR-02-14 through FR-02-17), plus shared FR-04-03 and FR-04-04 | Assistant | 2026-10-26 |
| Lane names match known Context/use-case actors | Static pass; role-label ambiguity recorded in Q6 | Assistant | 2026-10-26 |
| Sequence cross-check | SD-UC-08 exists; preliminary comparison found action-level RBAC detail requiring team reconciliation | Pending team review | 2026-11-07 |
| Static critic | Checked for action IDs, FRs, guards, loops, fork/join; open questions logged | Assistant | 2026-10-26 |
| Peer review | Pending | Assigned peer team | — |

---

Source: DBIZ2 Lesson 7 template and course activity profile. **Revised UC-08 only; no non-Must requirements added.**
