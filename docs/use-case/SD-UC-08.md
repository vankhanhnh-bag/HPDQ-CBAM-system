# Sequence Diagram: SD-UC-08 Manage CBAM Regulatory Rules & Parameters

Lesson 4, analysis level: how EcoTrace components cooperate when a Compliance / Product Owner opens CBAM regulatory settings, saves a rule-set draft, and optionally requests activation. The actor's choices and business-level outcomes appear in [AD-UC-08](../activity/AD-UC-08.md); this sequence focuses on requests and responses between system components.

| Field | Value |
|---|---|
| Use case | **UC-08 Manage CBAM regulatory rules & parameters** |
| Primary actor | Compliance / Product Owner |
| Version | v1.0 |
| FRs | **FR-02-14, FR-02-15, FR-02-16, FR-02-17 (UC-08 Must)**; shared **FR-04-03 (UC-11 Must), FR-04-04 (UC-12 Must)** |
| Sources | Midterm W0: UC-08, F-02; W1 Function List: `FR-02-14-FE/BE` (view active and scheduled configurations), `FR-02-15-FE/BE` (draft form and persistence), `FR-02-16-BE/FE` (activation validation and error feedback), `FR-02-17-BE/FE` (versioning, effective dates and activation), `FR-04-03-BE` (authorization middleware), `FR-04-04-BE` (audit event listener); W4 Must; [AD-UC-08](../activity/AD-UC-08.md) v0.2. |
| Preconditions | The Compliance / Product Owner has already signed in (UC-10); this sequence does not repeat login. RBAC permissions are checked before protected view, edit and activation requests. |

## Diagram

```mermaid
sequenceDiagram
    autonumber
    actor PO as Compliance / Product Owner
    participant UI as CBAM Regulatory UI
    participant SVC as CBAM Rules Service
    participant AUTH as Authorization Service
    participant VAL as Rule Validation Engine
    participant DB as CBAM Rule Versions DB
    participant LOG as Audit Log

    PO->>UI: Request CBAM rule settings
    UI->>SVC: Load regulatory configurations
    SVC->>AUTH: Verify view permission (FR-04-03)
    AUTH-->>SVC: Authorization result

    alt View permission denied
        SVC-->>UI: Deny access
        UI-->>PO: Show access denial
    else View permission granted
        SVC->>DB: Read active/scheduled rules, values, dates and sources (FR-02-14)
        DB-->>SVC: Regulatory configurations
        SVC-->>UI: Regulatory configurations
        UI-->>PO: Show active and scheduled rules

        opt Create or edit a rule set
            PO->>UI: Submit parameters and source references
            UI->>SVC: Save rule-set draft
            SVC->>AUTH: Verify edit permission (FR-04-03)
            AUTH-->>SVC: Authorization result

            alt Edit permission denied
                SVC-->>UI: Reject draft change
                UI-->>PO: Show access denial
            else Edit permission granted
                SVC->>DB: Save draft version, parameters and sources (FR-02-15)
                DB-->>SVC: Saved draft and version ID
                SVC->>LOG: Record immutable draft-change event (FR-04-04)
                SVC-->>UI: Draft saved
                UI-->>PO: Show draft confirmation

                opt Request activation of saved draft
                    PO->>UI: Submit activation request and effective date
                    UI->>SVC: Activate rule-set version
                    SVC->>AUTH: Verify activation permission (FR-04-03)
                    AUTH-->>SVC: Authorization result

                    alt Activation permission denied
                        SVC-->>UI: Reject activation
                        UI-->>PO: Show access denial
                    else Activation permission granted
                        SVC->>VAL: Validate mandatory values, ranges and effective periods (FR-02-16)
                        VAL->>DB: Read existing effective periods
                        DB-->>VAL: Existing version periods
                        VAL-->>SVC: Validation result and reasons

                        alt Invalid or conflicting draft
                            SVC-->>UI: Block activation and return reasons (FR-02-16)
                            UI-->>PO: Show validation reasons
                        else Valid draft
                            SVC->>DB: Version rules with effective date, preserve prior versions (FR-02-17)
                            DB-->>SVC: Updated version and effective-date status
                            SVC->>LOG: Record immutable activation event (FR-04-04)
                            Note over SVC,DB: The applicable version is used by later UC-07 estimates (FR-02-17)
                            SVC-->>UI: Return activation / scheduling result
                            UI-->>PO: Show activation result
                        end
                    end
                end
            end
        end
    end
```

## Interaction notes

The three authorization checks apply **FR-04-03** to three protected requests: viewing regulatory configurations, saving a draft, and activating a version. This is an analysis-level representation of authorization at each protected action. **W1 does not specify distinct permission-key names** for view, edit and activate.

The nested `opt` fragments represent actions the Compliance / Product Owner may choose not to perform. The `alt` fragments distinguish authorization outcomes and activation-validation outcomes. The [activity diagram](../activity/AD-UC-08.md) shows the actor's choices and business outcomes; this sequence shows the messages exchanged between EcoTrace components. The activity diagram currently abstracts edit/activation authorization into its entry check; the two diagrams should be reconciled during review if action-specific denial paths need to be shown in both.

**Traceability and limits:** FR-02-14 displays active and scheduled rule configurations, including values, effective dates and sources. FR-02-15 saves drafts. FR-02-16 prevents activation when required values are missing, ranges are invalid, or effective periods conflict. FR-02-17 preserves prior versions and applies the effective-date version to new UC-07 estimates. FR-04-04 records immutable audit events for completed system-altering actions. The precise parameter ranges, permission identifiers, scheduling mechanism, behavior on database/audit failures and retry rules are **not defined in W1**; therefore this diagram does not prescribe those details.
