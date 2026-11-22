# 01 - Introduction and Goals

## Purpose

This document describes the architecture of a Minesweeper desktop application
built in Python with Tkinter. The project originates from the
[Coding Dojo Minesweeper kata](https://codingdojo.org/kata/Minesweeper/),
which defines the core problem: given a minefield grid, calculate the number
of adjacent mines for every safe cell and render the annotated grid.

This project implements that core calculation and extends it into a fully
interactive game: the player can reveal cells, flag suspected mines, and
receive a win or loss outcome. No external servers, databases, or cloud
infrastructure are involved. The application runs entirely on the local
desktop.

---

## Functional Goals

| # | Goal |
|---|------|
| 1 | Display a rectangular minefield grid to the player. |
| 2 | Allow the player to reveal a cell by clicking on it. |
| 3 | Reveal all neighboring cells automatically when a revealed cell has zero adjacent mines. |
| 4 | Allow the player to flag a cell as a suspected mine. |
| 5 | Calculate and display the count of adjacent mines for each revealed safe cell. |
| 6 | End the game with a loss when the player reveals a mine. |
| 7 | End the game with a win when all safe cells have been revealed. |
| 8 | Allow the player to start a new game after a win or loss. |

---

## Quality Goals

The following quality goals are listed in priority order. They guide
architectural trade-offs throughout the design.

| Priority | Quality Goal | Motivation |
|----------|--------------|------------|
| 1 | **Simplicity** | The application is small and must remain straightforward to understand, run, and change. Unnecessary complexity — extra layers, frameworks, or infrastructure — is explicitly avoided. |
| 2 | **Testability** | The game logic and board management must be testable without starting the graphical interface. This keeps feedback fast and reduces the effort needed to verify correctness. |
| 3 | **Maintainability** | Each of the three components — Board Management, Game Logic, and User Interface — has a single, clear responsibility. Clear boundaries make future changes predictable and low-risk. |

---

## Stakeholders

| Stakeholder | Role | Main Interest |
|-------------|------|---------------|
| Player | End user | A working, responsive game that is easy to play. |
| Developer | Implementer and maintainer | Clear component boundaries, testable logic, and readable code. |
| Architecture reviewer | Course assessor | A documented design that demonstrates arc42 and C4 concepts correctly and consistently. |
