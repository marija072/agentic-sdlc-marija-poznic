# 02 - Architecture Constraints

Constraints are conditions the architecture must satisfy regardless of
design preferences. They are fixed by technology choices, the project scope,
or the course context.

---

## Technical Constraints

| ID | Constraint | Rationale |
|----|------------|-----------|
| TC-1 | The application must be implemented in **Python**. | Established in ADR-001. Python is the chosen language for game logic and overall application code. |
| TC-2 | The graphical interface must use **Tkinter**. | Established in ADR-001. Tkinter ships with standard Python installations and requires no additional dependencies for the GUI layer. |
| TC-3 | The application must run **locally on the desktop** without any network connection, external server, or database. | Stated in Chapter 1. The scope is a self-contained desktop application. |
| TC-4 | Game logic and board management must be **independent of Tkinter**. | Required by the Testability quality goal. Logic components must be exercisable via unit tests without launching the GUI. |
| TC-5 | The application is **planned to start from a single entry point** (e.g. `main.py` or a package entry point). | A single entry point is the intended design for simplicity, but is a convention rather than a hard requirement. |
| TC-6 | No external runtime services or cloud infrastructure may be introduced. | Follows directly from the local-execution constraint and the simplicity quality goal. |

---

## Organizational Constraints

| ID | Constraint | Rationale |
|----|------------|-----------|
| OC-1 | The project is part of the **Agentic SDLC course** (Module 2: Software Architecture). | The design must serve as a clear, assessable example of arc42 and C4 usage. |
| OC-2 | Architecture documentation is maintained in the `architecture/` directory using the **arc42 template**, one chapter per file. | Defined by the course workflow. Approved chapters must not be overwritten without explicit consent. |
| OC-3 | Diagrams are stored in `architecture/diagrams/` as `.puml` source files with matching `.svg` exports. | Keeps diagram source and rendered output together and version-controlled. |
| OC-4 | ADR-001 is already accepted and must be **preserved**. | Previously approved decisions are locked unless a change is explicitly requested and justified. |

---

## Project Scope Constraints

| ID | Constraint | Rationale |
|----|------------|-----------|
| SC-1 | The core algorithm must implement the **Coding Dojo Minesweeper kata**: calculate the adjacent mine count for every safe cell in a grid. | The kata is the stated starting point of the project (Chapter 1). |
| SC-2 | The game extends the kata with an **interactive interface** but must not introduce features beyond those listed as functional goals in Chapter 1. | Prevents scope creep. The design should remain proportionate to a small kata-based project. |
| SC-3 | The architecture is organised around **three main responsibilities**: Board Management, Game Logic, and User Interface. | Established in ADR-001. These three responsibilities must remain distinct. Supporting classes or helpers within a responsibility do not require a new ADR, but introducing a new top-level responsibility does. |
| SC-4 | No persistence layer (file save, database, leaderboard) is required. | Outside the stated scope. The game state lives only in memory for the duration of a session. |
