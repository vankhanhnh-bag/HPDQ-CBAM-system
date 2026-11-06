```mermaid
graph TB
    subgraph CUST["Customer"]
        start((Start))
        m1{" "}
        a1["Input and submit comparison criteria (FR-03-01-BE)"]
        a2["Display results (FR-03-03-FE, FR-03-04-FE)"]
        stop1((Stop))
    end

    subgraph SYS["EcoTrace system"]
        s1["Validate parameters"]
        d1{"Valid?"}
        s2["Identify validation errors"]
        s3["Query product profiles & CBAM costs (FR-03-02-BE, FR-03-02-INT)"]
        db1[(Product database)]
        d2{"Products found?"}
        s4["Check partial matches or suggest filter adjustments"]
        f1["Fork"]
        s5["Fetch PCF & data quality"]
        s6["Calculate carbon intensity & CBAM cost"]
        j1["Join"]
        s7["Calculate overall score (FR-03-03-BE)"]
        s8["Rank and assign flags Primary / Alternative (FR-03-04-BE)"]
    end

    start --> m1
    m1 --> a1
    a1 --> s1
    s1 --> d1
    d1 -->|"[Invalid]"| s2
    s2 --> m1
    d1 -->|"[Valid]"| s3
    s3 --> db1
    db1 --> s3
    s3 --> d2
    d2 -->|"[No]"| s4
    s4 --> m1
    d2 -->|"[Valid / Found]"| f1
    f1 --> s5
    f1 --> s6
    s5 --> j1
    s6 --> j1
    j1 --> s7
    s7 --> s8
    s8 --> a2

| Field | Value |
|---|---|
| AD ID | AD-UC-09 |
| Use case | UC-09 Compare and recommend green steel options |
| Feature | Green Steel Comparison & Recommendation |
| Primary actor | Customer |
| Lanes | Customer · EcoTrace system |
| Inputs (L3–L5) | Supplied AD-UC-09 Mermaid diagram, Context Diagram, Use Case Diagram, corresponding Sequence Diagram (SD-UC-09), Architecture, and W3/FR baseline |
| Trigger | The Customer inputs and submits comparison criteria. |
| Preconditions | The customer must have access to the comparison interface and valid authentication/authorization. |
| Success end state | EcoTrace successfully validates inputs, queries product profiles and CBAM costs, calculates scores, ranks options with Primary/Alternative flags, and displays the recommended results to the customer. |
| Failure end state | If validation fails or no products are found, the system triggers a **correction loop** (identifies errors or suggests filter/partial match adjustments, prompting the user to re-input criteria) rather than a terminal failure. |
| FRs covered | FR-03-01, FR-03-02, FR-03-03, FR-03-04 |
| Business rules applied | Input criteria validation; querying product profiles and CBAM costs; parallel calculation of PCF/data quality and carbon intensity/CBAM cost; overall scoring; ranking and assignment of Primary/Alternative flags. |
| Version / status | v1.0 |
| Author | Quan |
    a2 --> stop1
```
