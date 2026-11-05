# Sequence Diagram: SD-UC-10 Authenticate and sign in to the platform

A user enters valid credentials (or OTP), the system authenticates the user and grants a secure login session.

| Field | Value |
|---|---|
| Use case | UC-04 Authenticate and sign in to the platform |
| Version | v1 |
| FRs | FR-04-01 |

```mermaid
sequenceDiagram
    actor U as All users

    box LightBlue EcoTrace internal system
        participant UI as Sign-in UI
        participant Auth as Authentication service
        participant RBAC as User & RBAC store
        participant AZ as Authorisation service
        participant Audit as Audit log
    end

    U->>UI: 1. Enter credentials
    UI->>Auth: 2. Submit sign-in request
    Auth->>RBAC: 3. Verify user's identity
    RBAC-->>Auth: 4. Identity status + User record

    alt Invalid credentials
        Note right of Auth: Authentication fails
        Auth-->>UI: Deny session
        UI-->>U: Show error message
        Auth->>Audit: Record failed-attempt audit event

    else Valid credentials
        Auth->>AZ: 5. Load roles / permissions
        AZ->>RBAC: 6. Read RBAC assignment
        RBAC-->>AZ: 7. Roles / permissions
        AZ-->>Auth: 8. Authorisation context

        Auth->>Auth: 9. Create authenticated session / token
        Auth->>Audit: 10. Record successful sign-in event

        Auth-->>UI: 11. Session + access context
        UI-->>U: 12. Open authorised platform
    end
```
