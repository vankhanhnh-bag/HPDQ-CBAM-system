# Cash Flow, NPV, IRR & Payback

> Whole-project analysis using the Base scope (Must + Should). Initial investment links to W5; update only the yellow assumption cells.

<!-- Chỉ sửa trong các bảng 'input'. Phần giữa BEGIN/END sẽ được build.py ghi lại. -->

## 1. Cash-flow assumptions

<!-- BEGIN:assumptions | input table -->
| Assumption | Value | Unit | Source / basis |
|---|---|---|---|
| Scope | Whole project — Base (Must + Should) |  | Lecturer guidance: whole-project analysis is acceptable |
| Initial implementation cost | 109,860,000 | VND | Linked from W5_Handover Base cost (Must + Should) |
| Discount rate | 0.1 | % | Team assumption — replace with WACC / required return if available |
| Analysis horizon | 5 | years | Team assumption; model supports 1–10 years |
| Year 1 gross annual benefit | 65600000 | VND / year | INPUT REQUIRED — quantify validated business benefits |
| Annual benefit growth | 0.3 | % / year | Team assumption |
| Year 1 recurring operating cost | 16,479,000 | VND / year | INPUT REQUIRED — hosting, maintenance, data/API, support, etc. |
| Annual operating-cost growth | 0.0331 | % / year | Team assumption |
| Terminal / residual value | 0 | VND | Optional; received at the end of the analysis horizon |
<!-- END:assumptions -->

## 2. Scenario drivers | inherited assumptions and W5 scope

<!-- BEGIN:drivers | derived - auto-generated, do not edit -->
| Input / metric | Pessimistic: Must | Base: Must + Should | Optimistic: Full | Calculation basis |
|---|---|---|---|---|
| Build cost (VND) | 83,160,000 | 109,860,000 | 112,300,000 | W5 scope costs; Base also links to existing B8 |
| Must-only contingency (VND) | 8,316,000 | 8,316,000 | 8,316,000 | 10% of Must cost as defined in W5 assumptions |
| Year-0 investment (VND) | 91,476,000 | 118,176,000 | 120,616,000 | Build cost + Must contingency |
| Benefit allocation / Base | 75.7% | 100.0% | 102.2% | Proxy: relative build cost vs. the Base scope |
| Year-1 gross annual benefit (VND) | 49,656,800 | 65,600,000 | 67,056,982 | Existing B11 × benefit allocation |
| Year-1 annual running cost (VND) | 12,474,000 | 16,479,000 | 16,845,000 | Existing B13 × benefit allocation |
<!-- END:drivers -->

_Benefit and running-cost allocations are proportional sensitivity proxies; confirm scenario-specific benefits before treating decisions as final._

## 3. Pessimistic: Must only | cash flow (VND)

<!-- BEGIN:pessimistic | derived - auto-generated, do not edit -->
| Line item (VND) | 0 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|---|
| Benefits | 0 | 49,656,800 | 64,553,839 | 83,919,991 | 109,095,989 | 141,824,785 |
| Running cost | 0 | -12,474,000 | -12,886,889 | -13,313,445 | -13,754,120 | -14,209,382 |
| Investment (build + contingency) | -91,476,000 | 0 | 0 | 0 | 0 | 0 |
| Terminal / residual value | 0 | 0 | 0 | 0 | 0 | 0 |
| Net cash flow | -91,476,000 | 37,182,800 | 51,666,950 | 70,606,546 | 95,341,868 | 127,615,403 |
| Cumulative net cash flow | -91,476,000 | -54,293,200 | -2,626,250 | 67,980,295 | 163,322,164 | 290,937,567 |
| PV of net cash flow | -91,476,000 | 33,802,545 | 42,699,959 | 53,047,743 | 65,119,779 | 79,239,125 |
| Payback point (years) |  |  |  | 2.0372 |  |  |
<!-- END:pessimistic -->

## 3. Base: Must + Should | cash flow (VND)

<!-- BEGIN:base | derived - auto-generated, do not edit -->
| Line item (VND) | 0 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|---|
| Benefits | 0 | 65,600,000 | 85,280,000 | 110,864,000 | 144,123,200 | 187,360,160 |
| Running cost | 0 | -16,479,000 | -17,024,455 | -17,587,964 | -18,170,126 | -18,771,557 |
| Investment (build + contingency) | -118,176,000 | 0 | 0 | 0 | 0 | 0 |
| Terminal / residual value | 0 | 0 | 0 | 0 | 0 | 0 |
| Net cash flow | -118,176,000 | 49,121,000 | 68,255,545 | 93,276,036 | 125,953,074 | 168,588,603 |
| Cumulative net cash flow | -118,176,000 | -69,055,000 | -799,455 | 92,476,581 | 218,429,655 | 387,018,258 |
| PV of net cash flow | -118,176,000 | 44,655,455 | 56,409,541 | 70,079,666 | 86,027,644 | 104,680,258 |
| Payback point (years) |  |  |  | 2.0086 |  |  |
<!-- END:base -->

## 3. Optimistic: full scope | cash flow (VND)

<!-- BEGIN:optimistic | derived - auto-generated, do not edit -->
| Line item (VND) | 0 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|---|
| Benefits | 0 | 67,056,982 | 87,174,076 | 113,326,299 | 147,324,189 | 191,521,445 |
| Running cost | 0 | -16,845,000 | -17,402,570 | -17,978,595 | -18,573,686 | -19,188,475 |
| Investment (build + contingency) | -120,616,000 | 0 | 0 | 0 | 0 | 0 |
| Terminal / residual value | 0 | 0 | 0 | 0 | 0 | 0 |
| Net cash flow | -120,616,000 | 50,211,982 | 69,771,507 | 95,347,704 | 128,750,503 | 172,332,970 |
| Cumulative net cash flow | -120,616,000 | -70,404,018 | -632,512 | 94,715,193 | 223,465,695 | 395,798,665 |
| PV of net cash flow | -120,616,000 | 45,647,256 | 57,662,402 | 71,636,142 | 87,938,326 | 107,005,216 |
| Payback point (years) |  |  |  | 2.0066 |  |  |
<!-- END:optimistic -->

## 4. Results | five-year scenario investment appraisal

<!-- BEGIN:results | derived - auto-generated, do not edit -->
| Metric | Pessimistic: Must only | Base: Must + Should | Optimistic: full scope |
|---|---|---|---|
| Year-0 investment (VND) | 91,476,000 | 118,176,000 | 120,616,000 |
| Year-1 gross benefit (VND) | 49,656,800 | 65,600,000 | 67,056,982 |
| Year-5 gross benefit (VND) | 141,824,785 | 187,360,160 | 191,521,445 |
| NPV at hurdle rate (VND) | 182,433,151 | 243,676,565 | 249,273,341 |
| IRR | 57.3% | 58.6% | 58.7% |
| Simple payback (years) | 2.0372 | 2.0086 | 2.0066 |
| Profitability index | 2.9943 | 3.062 | 3.0667 |
| Decision signal | ACCEPT | ACCEPT | ACCEPT |
<!-- END:results -->

## 5. Reading the model

- The three MoSCoW cases use W5_Handover's Must, Must + Should and full-scope build costs, not LabDesk's sample VND figures.
- Existing CashFlow assumptions B7:B15 have been retained. Benefits use original Year-1 Base benefit and annual growth; costs use original recurring-cost assumption and annual growth.
- Must contingency equals the 10% assumption in W5_Handover and is added once in Year 0 for each scenario. No separate 20% Could money buffer is added.
- Pessimistic and Optimistic benefit and running-cost estimates are scaled proportionally to build cost. This is an illustrative sensitivity proxy, not separately validated revenue or savings.
- NPV = sum of discounted Year-0 to Year-5 net cash flows; IRR solves NPV = 0; simple payback interpolates the first positive cumulative cash flow; PI = 1 + NPV / Year-0 investment.
- ACCEPT when NPV is positive, IRR exceeds the existing hurdle rate and PI is greater than 1. Payback is shown separately; financial acceptance still requires validating project benefits.
