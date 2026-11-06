# Sequence Diagram: SD-UC-11 Configure Role-Based Access Control (RBAC) and system authorization

| Field | Value |
|---|---|
| Use case | **UC-11 Configure Role-Based Access Control (RBAC) and system authorization** |
| Version | v1.0 |
| FRs | **FR-04-02, FR-04-03 (UC-11 Must)**; shared **FR-04-04 (UC-12 Must)** |

```mermaid
sequenceDiagram
  actor A as System Admin
  participant W as Admin configuration interface
  participant M as Authorization middleware
  participant R as RBAC configuration API
  participant D as RBAC policy database
  participant L as Audit event listener
  participant G as Audit log database

  A->>W: request RBAC configuration access
  W->>M: request protected RBAC resource
  M->>D: read requester roles and permissions (FR-04-03)
  D-->>M: RBAC permissions
  alt requester is not authorized
    M-->>W: access denied (FR-04-03)
    W-->>A: configuration access denied
  else requester is authorized
    M-->>W: access granted (FR-04-03)
    A->>W: submit role and permission configuration
    W->>M: submit RBAC policy change
    M->>D: verify permission to modify policy (FR-04-03)
    D-->>M: authorization decision
    alt requester not authorized at submission
      M-->>W: change denied (FR-04-03)
      W-->>A: RBAC policy unchanged
    else requester authorized at submission
      M->>R: updateRoleConfiguration(configuration)
      R->>D: update access-control policies (FR-04-02)
      D-->>R: policy update completed
      R->>L: policy changed (FR-04-04)
      L->>G: record immutable audit entry (FR-04-04)
      R-->>W: configuration updated
      W-->>A: updated access policies
    end
  end
```
