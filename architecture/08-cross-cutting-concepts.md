# 08 - Cross-cutting Concepts

Cross-cutting concepts are concerns that apply across multiple components
rather than belonging to a single one. This chapter documents how the
Minesweeper application handles these concerns consistently.

---

## Separation of Concerns

The separation of the three main responsibilities — Board Management, Game
Logic, and User Interface — is the foundational structural principle of the
application. It is documented in ADR-001 and constrained in SC-3.

In practice this means:

- **Board Management** contains no game rules and no UI code. It holds grid
  state and calculates adjacent mine counts.
- **Game Logic** contains no Tkinter imports. It applies rules and manages
  game state using data from Board Management.
- **User Interface** contains no game rules. It translates player input into
  calls to Game Logic and renders the state it receives back.

This separation is enforced by keeping Tkinter imports out of Game Logic and
Board Management (TC-4). Any logic that belongs to a rule or a board
calculation must not migrate into the UI layer.

---

## Input Validation

Player input arrives as mouse events handled by the User Interface. The UI
is responsible for filtering out events that are not actionable before
forwarding them to Game Logic. These are normal situations that arise during
play and are silently ignored — no exception is raised.

| Situation | Handling |
|-----------|----------|
| Player clicks a cell that is already revealed | UI ignores the event; no call to Game Logic. |
| Player right-clicks a revealed cell | UI ignores the event; flagging applies only to hidden cells. |
| Player interacts with the board after the game has ended | UI ignores all board events once game state is WON or LOST. |

Game Logic applies a second check: it confirms that any action it receives
is valid for the current game state before delegating to Board Management.
If an action is not applicable in the current state — for example, a reveal
request when the game is already over — Game Logic discards it without
raising an exception. This provides a consistent safeguard regardless of
how the UI evolves.

Board Management does not validate player input. It assumes it receives
well-formed requests from Game Logic. Its internal checks are limited to
ensuring grid coordinates are within bounds.

---

## Error Handling

The application is a local desktop game with a small, well-defined input
space. The error-handling approach distinguishes between normal invalid
player actions, which are silently ignored, and internal programming errors,
which should surface loudly during development.

**Normal invalid player actions** are handled at the UI and Game Logic
layers as described in Input Validation above. They do not propagate to
Board Management and do not raise exceptions. A player clicking in the wrong
place at the wrong time is expected behaviour, not an error.

**Internal programming errors** are a different category. These arise from
incorrect calls between components — for example, Game Logic passing
out-of-bounds coordinates to Board Management, or a component being called
in a state it cannot handle due to a bug in the calling code. These should
raise exceptions during development so that bugs are caught early:

- Board Management raises an exception if it receives coordinates outside
  the grid bounds. Game Logic is responsible for ensuring coordinates are
  valid before calling Board Management.
- If a component receives a call that is structurally impossible given
  correct usage — such as being asked to operate on a board that has not
  been initialised — it raises an exception to make the programming error
  visible.

**Unexpected errors** — Unhandled exceptions propagate to the Python runtime
and terminate the process with a traceback. For a local development-stage
application this is acceptable. The traceback provides the information needed
to diagnose the problem. A production-quality application could wrap the
Tkinter main loop in a top-level exception handler to display a user-friendly
message, but that is outside the current scope.

---

## Testing

Testability is a first-class quality goal (Chapter 1). The testing approach
follows directly from the component separation.

**Board Management** is tested in isolation. Unit tests supply a fixed grid
and mine layout and assert that adjacent mine counts are calculated correctly.
This is the core of the Coding Dojo kata and can be verified without any
other component.

**Game Logic** is tested without a GUI. Tests instantiate a Board directly,
drive Game Logic through a sequence of moves, and assert on the resulting
game state (cell revealed, cascade triggered, game won or lost). No Tkinter
window is opened.

**User Interface** is not covered by automated unit tests. Its behaviour
depends on a running desktop environment and the Tkinter event loop. Manual
testing is used to verify rendering and event handling.

The test runner is **pytest**, consistent with standard Python practice. No
additional testing frameworks are required.

---

## Logging

The application does not use a logging framework. For a small desktop game
running in a single local process, print statements or Python's built-in
`logging` module are sufficient for development-time diagnostics. No
structured logging, log files, or log aggregation is needed.

If the application is extended in the future, the standard `logging` module
provides a straightforward upgrade path without changing the component
structure.

---

## Configuration

There is no external configuration file. Game parameters — grid dimensions
and mine count — are either fixed defaults or selected by the player through
the UI at the start of each game. All configuration is in-process and
in-memory for the duration of a session.
