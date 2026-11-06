# Screen List: EcoTrace


Screen ID pattern: `S-<feature>-<nn>`. Shared screens use `S-F00-<nn>`.

| Screen ID | Screen name | Actor | Feature | Shown when (purpose) | Entry points | Use cases / FRs served | Priority | Screen spec |
|---|---|---|---|---|---|---|---|---|
| S-F00-01 | Sign in | All Users | F-00 | A user who is not authenticated opens EcoTrace or requests a protected destination. | EcoTrace sign-in entry; redirect from a protected page when no valid session exists. | UC-10 Authenticate and Sign In; FR-04-01; shared access/authorization gate FR-04-03. | Must | `screens/screen-spec-S-F00-01.md` |
| S-F04-01 | User & Access Management | System Admin |F-04|A System Admin opens the administration area to perform the user/access-administration use case (UC-11).|After successful sign-in and authorization; Platform / Access menu.|UC-11 (working label: Manage User Access); FR-04-02 and FR-04-03.| Must |`screens/screen-spec-S-F04-01.md`|
|S-F04-02|Audit Log Viewer|System Admin; Auditor|F-04|A System Admin or Auditor opens the audit area to review system activity recorded by EcoTrace (UC-12).|After successful sign-in and authorization; Platform / Audit Logs menu.|UC-12 (working label: Review Audit Logs); FR-04-04 and FR-04-03.|Must|`screens/screen-spec-S-F04-02.md`|


## Modals and overlays (ai điền thêm thì xóa 2 dòng mặc định trong bảng)

| Modal | Opened from screen | Purpose |
|---|---|---|
| Cancel booking? | S-F01-03 | Confirm cancellation of one booking |
| Block seat | S-F03-02 | Enter reason and period for blocking a seat |

## What is deliberately not a screen

| Candidate | Why it is not a screen |
|---|---|
| "Invalid credentials" | Error state of S-F00-01 (Sign In) |
| "Access denied" | Access-control result/state of the requested screen (FR-04-03); it is not a separate screen |
| "Audit log access denied" | Error/authorization state of S-F04-02 (Audit Log Viewer) |
