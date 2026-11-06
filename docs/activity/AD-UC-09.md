swimlane-beta LR
subgraph CUST["Customer"]
start@{ shape: sm-circ }
m1{" "}
a1**("Input and submit comparison criteria (FR-03-01-BE)")
a2("Display results (FR-03-03-FE,** FR-03-04-FE**)")
stop1@{ shape: fr-circ }
end
subgraph SYS["EcoTrace system"]
s1("Validate parameters")**
d1{" "}
s2**("Identify validation errors")**
s3**("Query product profiles & CBAM costs (FR-03-02-BE,** FR-03-02-INT**)")
db1[(Product database)]
d2{" "}
s4("Check partial matches or suggest filter adjustments")**
f1@{ shape: fork }
s5**("Fetch PCF & data quality")**
s6**("Calculate carbon intensity & CBAM cost")**
j1@{ shape: join }
s7**("Calculate overall score (FR-03-03-BE)")
s8("Rank and assign flags Primary / Alternative (FR-03-04-BE)**")
end
start --> m1 --> a1 --> s1 --> d1
d1 -->|"[Invalid]"| s2 --> m1
d1 -->|"[Valid]"| s3
db1 -.-> s3
s3 --> d2
d2 -->|"[No]"| s4 --> m1
d2 -->|"[Products found]"| f1
f1 --> s5 --> j1
f1 --> s6 --> j1
j1 --> s7 --> s8 --> a2 --> stop1
