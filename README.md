# EcoTrace: HPDQ Carbon Management & Reporting System

This repository contains the DBIZ2 Digital Business Project II work for EcoTrace, the HPDQ Carbon
Management & Reporting System. It follows the course sequence from business context and use cases
through the function list, activity diagrams and external design.

**EcoTrace** brings together supplier, production, energy and shipment information for carbon
traceability and product carbon footprints (PCF). The defined scope also includes shipment-level
CBAM exposure estimation, CBAM regulatory configuration, green steel comparison, and platform
access and audit functions.

## Where each lesson's work goes

Each lesson writes into its own folder, so you always know where today's work goes and where the
next lesson will look for it.

```text
docs/
  project/            Lessons 1-2   project-brief.md, ai-log.md
  context/            Lesson 3      cvca.md, context_diagram.md
  use-case/           Lesson 4      use-case-diagram.md, SD-<UC-ID>.md
  architecture/       Lesson 4      architecture.md
  function-list/      Lessons 5-6   Parameters.md, W0_Scope.md ... W6_CashFlow.md
  activity/           Lesson 7      activity-index.md, AD-<UC-ID>.md
  external-design/    Lesson 8      usage-flow.md, screen-list.md, screen-transition.md,
                                    ui-conventions.md
    screens/          Lesson 8      screen-spec-S-F01-02.md (worked example)
      img/            Lesson 8      example screen images
```

## Read it in this order

| Step | Lesson | File | What it shows |
|---|---|---|---|
| 1 | L1-L2 | [project-brief.md](docs/project/project-brief.md) | Lesson 1–2 project brief template; still the LabDesk worked example |
| 2 | L3 | [cvca.md](docs/context/cvca.md) | EcoTrace stakeholders, value exchanges, problems and stakeholder requirements |
| 3 | L3 | [context_diagram.md](docs/context/context_diagram.md) | EcoTrace system boundary, internal departments, external partners and data flows |
| 4 | L4 | [use-case-diagram.md](docs/use-case/use-case-diagram.md) | Four features, UC-01 to UC-12, actors and the sequence-diagram inventory |
| 5 | L4 | [SD-UC-01.md](docs/use-case/SD-UC-01.md), [SD-UC-02.md](docs/use-case/SD-UC-02.md), [SD-UC-04.md](docs/use-case/SD-UC-04.md), [SD-UC-06.md](docs/use-case/SD-UC-06.md), [SD-UC-07.md](docs/use-case/SD-UC-07.md), [SD-UC-08.md](docs/use-case/SD-UC-08.md), [SD-UC-09.md](docs/use-case/SD-UC-09.md), [SD-UC-10.md](docs/use-case/SD-UC-10.md), [SD-UC-11.md](docs/use-case/SD-UC-11.md) | Existing Sequence Diagrams for UC-01, UC-02, UC-04, UC-06, UC-07, UC-08, UC-09, UC-10 and UC-11; missing SD files are not linked |
| 6 | L4 | [architecture.md](docs/architecture/architecture.md) | System requirements, functional design, physical design, three layers and decisions |
| 7 | L5 | [W0_Scope.md](docs/function-list/W0_Scope.md) | Use-case inventory, F-01 to F-04, Won't-have scope and ILF/EIF data functions |
| 8 | L5 | [W1_FunctionList.md](docs/function-list/W1_FunctionList.md) | The 35 functional requirements, EARS criteria and FE/BE/INT work items |
| 9 | L5 | [W2_Costing.md](docs/function-list/W2_Costing.md), [W2_Rollup.md](docs/function-list/W2_Rollup.md), [W3_CrossReview.md](docs/function-list/W3_CrossReview.md), [W4_Prioritization.md](docs/function-list/W4_Prioritization.md), [W5_Handover.md](docs/function-list/W5_Handover.md) | Effort and cost, review findings, priorities, thresholds and handover scenarios |
| 10 | L6 | [W6_CashFlow.md](docs/function-list/W6_CashFlow.md) | Cash flow, NPV, IRR and payback under three scope scenarios |
| 11 | L7 | [activity-index.md](docs/activity/activity-index.md), [AD-UC-01.md](docs/activity/AD-UC-01.md), [AD-UC-02.md](docs/activity/AD-UC-02.md), [AD-UC-04.md](docs/activity/AD-UC-04.md), [AD-UC-05.md](docs/activity/AD-UC-05.md), [AD-UC-06.md](docs/activity/AD-UC-06.md), [AD-UC-07.md](docs/activity/AD-UC-07.md), [AD-UC-08.md](docs/activity/AD-UC-08.md), [AD-UC-09.md](docs/activity/AD-UC-09.md), [AD-UC-10.md](docs/activity/AD-UC-10.md), [AD-UC-11.md](docs/activity/AD-UC-11.md) | Activity Diagram index and all existing AD files; AD-UC-05 is still the teacher's LabDesk worked example, not the EcoTrace Scope 3 diagram |
| 12 | L8 | [usage-flow.md](docs/external-design/usage-flow.md) | Actor-organized usage flows connected to use cases, activity diagrams and screen IDs |
| 13 | L8 | [screen-list.md](docs/external-design/screen-list.md), [screen-transition.md](docs/external-design/screen-transition.md), [ui-conventions.md](docs/external-design/ui-conventions.md) | EcoTrace screen inventory and navigation; UI conventions file is still a LabDesk worked example |
| 14 | L8 | [screen-spec-S-F01-02.md](docs/external-design/screens/screen-spec-S-F01-02.md) | Teacher's LabDesk Screen Spec worked example, not an EcoTrace Screen Spec |
| all | all | [ai-log.md](docs/project/ai-log.md) | Teacher's LabDesk AI-log worked example; no EcoTrace-specific log entered here yet |

## How the files connect

Every file refers to the others by ID, never by retelling them:

- `StR-xx` stakeholder requirements (CVCA) are traced to `SyR-xx` system requirements (architecture).
- `UC-xx` use cases (W0 Table 1) are grouped into `F-xx` features (W0 Table 2).
- `FR-xx-yy` functions (W1) carry EARS criteria and are linked to the use cases they serve.
- `AD-UC-xx` activity diagrams trace actions and decisions to the applicable FRs.
- `S-Fxx-nn` screens appear in the Screen List, Usage Flow and Screen Transition Diagram.

Some files still show the course's LabDesk worked example rather than EcoTrace work:
`project-brief.md`, `ai-log.md`, `ui-conventions.md`, the sample
`screen-spec-S-F01-02.md` and `AD-UC-05.md`. The EcoTrace activity for UC-05 has
not replaced that sample. There is no `AD-UC-03.md` or `AD-UC-12.md` in the current
repository, and the Use Case Diagram lists SD-UC-03, SD-UC-05 and SD-UC-12 without matching
files. The Activity Index distinguishes completed files from these gaps.

## Notes on the notation

- Diagrams are Mermaid code blocks. Activity diagrams that use `swimlane-beta` need Mermaid
  11.16 or later; if a diagram does not render in your viewer, paste the block into
  [mermaid.live](https://mermaid.live).
- Mermaid has no use case diagram type; the Use Case Diagram is drawn as a flowchart. In a drawing
  tool, use UML notation.
- Money is in VND. Rates, AI leverage factors and thresholds are defined in
  [Parameters.md](docs/function-list/Parameters.md); the cash-flow scenarios in W6 use the
  project assumptions recorded in the function-list files.
