```mermaid
swimlane-beta TB
subgraph USER["Sustainability & ESG user"]
start@{ shape: sm-circ }
m1{" "}
a1**("Enter methodology version,** factor value**,** unit**,** effective date (FR-01-02-FE**)")
a2("Show saved confirmation (FR-01-02-FE)")
stop1@{ shape: fr-circ }
end
subgraph SYS["EcoTrace system"]
s1("Validate methodology version,** factor value**,** unit**,** effective date (FR-01-02-BE**)")
d1{"Valid?"}
s2("Show validation errors")**
d2{"Existing factor?"}
d3{"Reason given?"}
s3**("Reject: Change Justification required")**
s4**("Set Created By and Created At (FR-01-02-BE)")
s5("Record Updated By,** Updated At and Change Justification")
db1[(GHG methodology & emission factor library)]
db2[(Audit log)]
s6**("Create new version,** keep previous version (FR-01-02-BE**)")
s7("Write audit log entry Who,** when**,** old to new**,** reason (FR-01-02-BE**)**")
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
