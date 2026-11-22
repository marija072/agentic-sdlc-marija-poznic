# 04 - Solution Strategy

This chapter explains the key decisions that shape the solution and connects
them to the goals and constraints established in Chapters 1 and 2.

---

## Technology Choice: Python and Tkinter

Python was selected as the implementation language because its syntax is
readable and its standard library includes everything needed for a small
desktop game. No additional runtime dependencies are required for the logic
layer.

Tkinter was selected for the graphical interface because it ships with
standard Python installations on most desktop platforms. This eliminates
external GUI dependencies and keeps the setup process simple: install Python,
run the application (TC-2, TC-3, TC-6).

Both choices were established in ADR-001 and are consistent with the
simplicity quality goal (Chapter 1).

---

## Separation into Three Responsibilities

The application is organised around three distinct responsibilities, as
established in ADR-001 and constrained in SC-3:

| Responsibility | What it does |
|----------------|--------------|
| **Board Management** | Holds the grid state, records mine positions, and calculates the adjacent mine count for each safe cell — the core of the Coding Dojo kata. |
| **Game Logic** | Processes player moves, applies game rules (reveal, flag, cascade reveal of empty cells), and determines the win or loss outcome. |
| **User Interface** | Renders the current board state using Tkinter and translates mouse events into actions passed to Game Logic. |

Keeping these responsibilities separate means each one can be understood,
changed, and tested in isolation. Game Logic and Board Management have no
dependency on Tkinter, satisfying TC-4 and the testability quality goal.

---

## The Kata as the Core

The Coding Dojo Minesweeper kata defines the foundational algorithm: given
a grid of cells, some containing mines, calculate the number of adjacent
mines for every safe cell. This calculation is the responsibility of Board
Management.

The interactive game is built on top of that core. Game Logic uses the
board's mine and count data to decide what to reveal, when to cascade, and
when the game ends. The User Interface makes those outcomes visible and
collects the player's next input. Each layer depends on the one below it
but not on the one above it, so the kata's algorithm can be developed and
verified independently of the rest of the application (SC-1, SC-2).

---

## How the Architecture Supports the Quality Goals

**Simplicity** — Three components in a single local process. No network,
no database, no additional frameworks. The structure reflects the problem
directly without introducing layers that serve no purpose at this scale.

**Testability** — Board Management and Game Logic are plain Python with no
Tkinter imports. They can be exercised by a standard test runner (such as
pytest) without displaying any window. The User Interface is the only
component that requires a running desktop environment.

**Maintainability** — Each component has one clear job. The interfaces
between them are narrow: the UI reads state from Game Logic and sends user
actions to it; Game Logic reads and updates Board Management. A change in
one component does not ripple into the others as long as those interfaces
remain stable.

---

## A Single Local Desktop Application

The project scope (Chapter 1) and constraints (TC-3, TC-6, SC-4) rule out
a distributed or networked architecture. Minesweeper is a single-player,
stateless-between-sessions game. There is nothing to synchronise across
machines, no shared data to persist, and no concurrent users to serve. A
single process running on the player's desktop is the most direct solution
and introduces no unnecessary complexity.
