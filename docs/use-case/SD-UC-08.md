
# Sequence Diagram: SD-UC-08 Manage CBAM regulatory rules & parameters

| Field | Value |
|---|---|
| Use case | UC-08 Manage CBAM regulatory rules & parameters |
| Version | v1.0 |
| FRs | FR-02-14 to FR-02-17; shared FR-04-03, FR-04-04 |

```mermaid
sequenceDiagram
  actor PO as Compliance / Product Owner
  participant W as Web app
  participant A as Access & audit service
  participant C as CBAM service
  participant D as EcoTrace database

  PO->>W: open CBAM regulatory settings
  W->>A: check view permission (FR-04-03)
  A->>D: read role and permissions
  D-->>A: permission context
  alt view not authorized
    A-->>W: access denied
    W-->>PO: show access denial
  else view authorized
    A-->>W: access granted
    W->>C: load regulatory configurations
    C->>D: read active and scheduled rule sets (FR-02-14)
    D-->>C: parameters, effective dates and sources
    C-->>W: regulatory configurations
    W-->>PO: show active and scheduled rules
    PO->>W: submit rule-set parameters and source references
    W->>A: check edit permission (FR-04-03)
    alt edit not authorized
      A-->>W: access denied
      W-->>PO: show access denial
    else edit authorized
      A-->>W: access granted
      W->>C: save rule-set draft
      C->>D: persist draft version (FR-02-15)
      D-->>C: saved draft and version ID
      C->>A: record draft change (FR-04-04)
      A->>D: append immutable audit entry
      C-->>W: draft saved
      W-->>PO: show draft confirmation
      PO->>W: request activation with effective date
      W->>A: check activation permission (FR-04-03)
      alt activation not authorized
        A-->>W: access denied
        W-->>PO: show access denial
      else activation authorized
        A-->>W: access granted
        W->>C: validate draft for activation
        C->>D: read existing effective periods
        D-->>C: existing version periods
        alt invalid or conflicting draft
          C-->>W: block activation, return reasons (FR-02-16)
          W-->>PO: show validation reasons
        else valid draft
          C->>D: version rules with effective date, preserve history (FR-02-17)
          D-->>C: version and effective-date status
          C->>A: record activation (FR-04-04)
          A->>D: append immutable audit entry
          Note over C,D: Applicable version is used for later UC-07 estimates (FR-02-17)
          C-->>W: activation or scheduling result
          W-->>PO: show activation result
        end
      end
    end
  end
```
