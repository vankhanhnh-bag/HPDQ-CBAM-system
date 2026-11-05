# LabDesk: worked example of a whole DBIZ2 project

Worked example for DBIZ2 Digital Business Project II (VJCBI College, Foreign Trade University).
It shows one small product, LabDesk, going through every lesson of the course, from the first
client meeting to the screens that DBIZ3 will build. It is a teaching example, not a real product
and not a student submission.

**LabDesk** is a seat reservation system for the faculty computer labs. Students book a seat before
they come and check in with a QR code at the lab door; seats nobody uses are released after 15
minutes; Lab Technicians block faulty seats and watch how labs are used.

## Where each lesson's work goes

Each lesson writes into its own folder, so you always know where today's work goes and where the
next lesson will look for it.

```text
docs/
  project/            Lessons 1-2   project-brief.md, ai-log.md
  context/            Lesson 3      cvca.md, context-diagram.md
  use-case/           Lesson 4      use-case-diagram.md, SD-<UC-ID>.md
  architecture/       Lesson 4      architecture.md, adr/ADR-<nnn>-<title>.md
  function-list/      Lessons 5-6   w0-scope.md ... w6-cash-flow.md
  activity/           Lesson 7      activity-index.md, AD-<UC-ID>.md, review-<team>.md
  external-design/    Lesson 8      usage-flow.md, screen-list.md, screen-transition.md,
                                    ui-conventions.md, review-<team>.md
    screens/          Lesson 8      screen-spec-<Screen ID>.md
      img/            Lesson 8      <Screen ID>.png
```

## Read it in this order

| Step | Lesson | File | What it shows |
|---|---|---|---|
| 1 | L1-L2 | [project-brief.md](docs/project/project-brief.md) | Client, problem, goal, team roles, milestones |
| 2 | L3 | [cvca.md](docs/context/cvca.md) | Stakeholders, money and information flows, problems, stakeholder requirements |
| 3 | L3 | [context-diagram.md](docs/context/context-diagram.md) | LabDesk as a black box and the entities around it |
| 4 | L4 | [use-case-diagram.md](docs/use-case/use-case-diagram.md) | Who wants LabDesk to do what; list of sequence diagrams |
| 5 | L4 | [SD-UC-01.md](docs/use-case/SD-UC-01.md), [SD-UC-04.md](docs/use-case/SD-UC-04.md), [SD-UC-08.md](docs/use-case/SD-UC-08.md) | How the parts of LabDesk talk for one use case |
| 6 | L4 | [architecture.md](docs/architecture/architecture.md) and [adr/](docs/architecture/adr/) | System requirements, functional and physical design, three layers, decisions |
| 7 | L5 | [w0-scope.md](docs/function-list/w0-scope.md) | Use case inventory, features, Won't-have, data, business rules |
| 8 | L5 | [w1-function-list.md](docs/function-list/w1-function-list.md) | Every function with its EARS criterion, split into work items |
| 9 | L5 | [w2-cost.md](docs/function-list/w2-cost.md), [w4-priorities.md](docs/function-list/w4-priorities.md), [w5-handover.md](docs/function-list/w5-handover.md) | Cost, priorities, threshold checks, three scenarios |
| 10 | L6 | [w6-cash-flow.md](docs/function-list/w6-cash-flow.md) | Cash flow, NPV, IRR, payback for the three scenarios |
| 11 | L7 | [activity-index.md](docs/activity/activity-index.md), [AD-UC-01.md](docs/activity/AD-UC-01.md), [AD-UC-05.md](docs/activity/AD-UC-05.md), [AD-UC-08.md](docs/activity/AD-UC-08.md), [AD-UC-10.md](docs/activity/AD-UC-10.md) | Inputs check against L3-L4, then activity diagrams |
| 12 | L8 | [usage-flow.md](docs/external-design/usage-flow.md) | One usage flow per actor, linked to activity diagrams and screens |
| 13 | L8 | [screen-list.md](docs/external-design/screen-list.md), [screen-transition.md](docs/external-design/screen-transition.md), [ui-conventions.md](docs/external-design/ui-conventions.md) | Screens, how users move, shared conventions |
| 14 | L8 | [screen-spec-S-F01-02.md](docs/external-design/screens/screen-spec-S-F01-02.md) | One complete Screen Spec with its wireframe |
| all | all | [ai-log.md](docs/project/ai-log.md) | Where AI suggested something wrong, and what the team did |

## How the files connect

Every file refers to the others by ID, never by retelling them:

- `StR-xx` stakeholder requirements (CVCA) become `SyR-xx` system requirements (architecture).
- `UC-xx` use cases (W0 Table 1) are grouped into `F-xx` features (W0 Table 2).
- `FR-xx-yy` functions (W1) carry the EARS criterion; `BR-xx` business rules are in W0.
- `AD-UC-xx` activity diagrams cite FRs on every system action.
- `S-Fxx-nn` screens cite their use cases and FRs, and appear in the usage flow.

Some files show the project mid-course on purpose. For example, row 4 of the Lesson 7 Inputs check
is still open, so SD-UC-02 is not drawn yet. Only AD-UC-01, AD-UC-05 and one Screen Spec are
written out; the index and the screen list name the others, which are left for practice.

## Notes on the notation

- Diagrams are Mermaid code blocks. The activity diagrams use `swimlane-beta`, which needs Mermaid
  11.16 or later; if a diagram does not render in your viewer, paste the block into
  [mermaid.live](https://mermaid.live).
- Mermaid has no use case diagram type; the Use Case Diagram is drawn as a flowchart. In a drawing
  tool, use the UML notation.
- Money is in VND. Rates, AI leverage factors and thresholds are the class Parameters of the
  Lesson 5 workbook; the benefit figures in W6 are team estimates for teaching.
