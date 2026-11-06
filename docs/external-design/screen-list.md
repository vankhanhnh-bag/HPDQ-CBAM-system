# Screen List: EcoTrace


Screen ID pattern: `S-<feature>-<nn>`. Shared screens use `S-F00-<nn>`.

| Screen ID | Screen name | Actor | Feature | Shown when (purpose) | Entry points | Use cases / FRs served | Priority | Screen spec |
|---|---|---|---|---|---|---|---|---|
| S-F00-01 | Sign in | All Users | F-00 | A user who is not authenticated opens EcoTrace or requests a protected destination. | EcoTrace sign-in entry; redirect from a protected page when no valid session exists. | UC-10 Authenticate and Sign In; FR-04-01; shared access/authorization gate FR-04-03. | Must | `screens/screen-spec-S-F00-01.md` |
| S-F02-01 | CBAM Estimation & Analysis | Export Sales & Marketing Dep.; Procurement & Raw Materials Dep. | F-02 | User wants to estimate the CBAM cost of a shipment and review supporting detail before quoting or purchasing | Sidebar nav "CBAM & Carbon Cost > CBAM Estimation"; Home dashboard shortcut | UC-07; FR-02-01 to FR-02-13 | Must | `screens/screen-spec-S-F02-01.md` |
| S-F04-01 | User & Access Management | System Admin |F-04|A System Admin opens the administration area to perform the user/access-administration use case (UC-11).|After successful sign-in and authorization; Platform / Access menu.|UC-11 (working label: Manage User Access); FR-04-02 and FR-04-03.| Must |`screens/screen-spec-S-F04-01.md`|
|S-F04-02|Audit Log Viewer|System Admin; Auditor|F-04|A System Admin or Auditor opens the audit area to review system activity recorded by EcoTrace (UC-12).|After successful sign-in and authorization; Platform / Audit Logs menu.|UC-12 (working label: Review Audit Logs); FR-04-04 and FR-04-03.|Must|`screens/screen-spec-S-F04-02.md`|


## Modals and overlays (ai điền thêm thì thêm dòng)

| Modal | Opened from screen | Purpose |
|---|---|---|
| M-F02-01 Share CBAM result | S-F02-01 | Create a shareable, access-controlled link to the current estimate (FR-02-13) |
| M-F02-02 View assumptions | S-F02-01 | Show the full list of regulation version, price reference date and calculation method used (part of FR-02-09) |

## What is deliberately not a screen

| Candidate | Why it is not a screen |
|---|---|
| "Invalid credentials" | Error state of S-F00-01 (Sign In) |
| "Access denied" | Access-control result/state of the requested screen (FR-04-03); it is not a separate screen |
| "Audit log access denied" | Error/authorization state of S-F04-02 (Audit Log Viewer) |
