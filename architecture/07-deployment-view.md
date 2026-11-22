# 07 - Deployment View

## Deployment Environment

The Minesweeper application runs on a local desktop computer. The only
prerequisite is a Python installation that includes Tkinter support. No
network connection, server, database, or cloud account is required at any
point — neither to install nor to run the application.

---

## Runtime Structure

The entire application executes as a single Python process on the player's
machine. There are no background services, worker processes, or inter-process
communication channels.

```
┌─────────────────────────────────────────────────────┐
│                  Local Desktop Computer             │
│                                                     │
│  ┌─────────────────────────────────────────────┐   │
│  │            Python Runtime                   │   │
│  │                                             │   │
│  │  ┌───────────────────────────────────────┐  │   │
│  │  │     Minesweeper Desktop Application   │  │   │
│  │  │                                       │  │   │
│  │  │  User Interface (Tkinter)             │  │   │
│  │  │  Game Logic (Python)                  │  │   │
│  │  │  Board Management (Python)            │  │   │
│  │  └───────────────────────────────────────┘  │   │
│  └─────────────────────────────────────────────┘   │
│                                                     │
│  Operating System (Windows, macOS, or Linux)        │
└─────────────────────────────────────────────────────┘
```

The Python runtime sits between the application and the operating system.
Tkinter delegates window creation and event handling to the OS's native
display layer, which is why a desktop environment must be available.

---

## Starting the Application

Starting the application requires two steps:

1. **Install dependencies.** Python (with Tkinter) must be present on the
   machine. On most systems Tkinter is bundled with the standard Python
   distribution. Additional Python packages, if any are introduced during
   implementation, can be installed with `pip`.

2. **Run the entry point.** Launch the application from the project
   directory, for example:

   ```bash
   python main.py
   ```

   The process starts, the Tkinter window opens, and the game is ready to
   play. When the player closes the window the process exits.

---

## Relationship Between Application, Python Runtime, and Operating System

| Layer | Role |
|-------|------|
| Operating System | Provides the desktop environment, window management, and input event delivery (mouse, keyboard). |
| Python Runtime | Executes the application code and provides the standard library, including the `tkinter` module. |
| Minesweeper Application | Runs entirely within the Python process. Tkinter calls translate into OS-level window and drawing operations. |

The application has no direct system calls of its own. All OS interaction
is mediated by the Python runtime and Tkinter.

---

## Why No External Infrastructure Is Required

The deployment model is intentionally minimal, in line with constraints
TC-3, TC-6, and SC-4:

- **No server** — all three components run in a single local process.
  There is no network socket, no HTTP endpoint, and no service to start
  separately.
- **No database** — game state exists only in memory for the duration of
  a session. When the application exits, the state is discarded. Persistence
  was explicitly excluded from scope in Chapter 2 (SC-4).
- **No cloud infrastructure** — the application is self-contained on the
  player's machine. No external account, API key, or connection is needed.
- **No installer or container** — running a Python script from the project
  directory is sufficient. No packaging, containerisation, or deployment
  pipeline is required for local use.
