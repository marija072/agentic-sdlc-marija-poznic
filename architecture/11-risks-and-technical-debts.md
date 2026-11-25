# 11 - Risks and Technical Debts

## Overview

This chapter identifies the architectural risks and known technical debts
relevant to the Minesweeper application. Risks are conditions that may
cause problems if left unaddressed. Technical debts are deliberate
simplifications made within the current scope that would need to be
revisited if the application were extended.

---

## Risks

### R-1: Component Boundary Erosion

| Attribute | Detail |
|-----------|--------|
| **Description** | As the codebase grows, game rules or board calculations may gradually migrate into the User Interface, or Tkinter-specific code may appear in Game Logic or Board Management. |
| **Impact** | The logic components become harder to test headlessly. Changes to the UI start requiring changes to logic components, increasing the cost of maintenance. |
| **Likelihood** | Medium. Boundary erosion is a common outcome in small projects without enforced structural checks. |
| **Mitigation** | Keep Tkinter imports absent from Game Logic and Board Management (TC-4). Code reviews and, where practical, import checks in the test suite catch violations early (QM-4). |

---

### R-2: Tkinter Desktop Environment Dependency

| Attribute | Detail |
|-----------|--------|
| **Description** | Tkinter requires a functioning desktop environment. On headless systems — such as a CI server or a minimal Linux installation — the application cannot start and the UI cannot be tested manually. |
| **Impact** | The UI layer cannot be verified in environments without a display. Automated testing is limited to the headless logic components. |
| **Likelihood** | Low for a local desktop application, but relevant if the project is built or assessed in a CI environment. |
| **Mitigation** | Restrict automated tests to Board Management and Game Logic (Chapter 10, QT-1). Accept manual testing for the UI. If CI validation of the UI becomes necessary, a virtual display tool such as Xvfb can be used without changing the application architecture. |

---

### R-3: PlantUML Remote Include Fragility

| Attribute | Detail |
|-----------|--------|
| **Description** | The C4 diagram source files include the C4-PlantUML library via a remote HTTPS URL. If that URL changes, the repository moves, or the network is unavailable, SVG generation fails. This was observed during Chapter 5 when the system PlantUML binary could not resolve the remote include. |
| **Impact** | Diagrams cannot be regenerated without the project-specific PlantUML JAR and network access. The pre-commit hook that generates SVGs silently depends on these conditions being met. |
| **Likelihood** | Low under normal conditions, but already encountered once in this project. |
| **Mitigation** | The project's PlantUML JAR at `~/.local/share/plantuml/plantuml.jar` resolves this with the `-Dplantuml.allowincludeurl=true` flag. A more durable solution would be to vendor the C4-PlantUML library files locally in the repository, removing the network dependency entirely. |

---

### R-4: No Input Sanitisation for Grid Parameters

| Attribute | Detail |
|-----------|--------|
| **Description** | Grid dimensions and mine count are either fixed defaults or player-supplied values. If a player can enter these values directly (for example, in a future configuration dialog), invalid inputs — such as a mine count exceeding the number of cells — could cause undefined behaviour in Board Management. |
| **Impact** | The application may crash or produce a nonsensical board state. |
| **Likelihood** | Low with fixed defaults. Increases if configurable game parameters are introduced. |
| **Mitigation** | Validate grid parameters in Game Logic before passing them to Board Management. Reject configurations where the mine count is zero or equals or exceeds the total cell count. |

---

## Technical Debts

### TD-1: No Persistent Game State

| Attribute | Detail |
|-----------|--------|
| **Description** | Game state is held only in memory. When the application exits, the current game is lost. This was an explicit scope decision (SC-4, ADR-001). |
| **Impact** | Players cannot resume an interrupted game. No game history or high scores can be recorded. |
| **Accepted because** | Persistence is outside the scope of a kata-based project and would add a storage layer inconsistent with the simplicity goal. |
| **If revisited** | A simple file-based save using Python's standard library (for example, `json` or `pickle`) could be added without changing the three-component structure. Game Logic would own the serialisation interface. |

---

### TD-2: No Named Difficulty Presets

| Attribute | Detail |
|-----------|--------|
| **Description** | Grid dimensions and mine count are either fixed defaults or selected by the player at the start of each game (Chapter 8, Configuration). Standard Minesweeper difficulty presets — named levels such as beginner, intermediate, and expert with well-known fixed grid sizes — are not provided. |
| **Impact** | Players must supply or accept raw grid dimensions and mine counts rather than selecting a familiar difficulty label. |
| **Accepted because** | Named difficulty presets are outside the functional goals stated in Chapter 1. The ability to choose game parameters at startup is already supported by the architecture; presets would be a UI convenience on top of that. |
| **If revisited** | Presets could be added to the UI start screen as named shortcuts that populate the parameter fields. No changes to Game Logic or Board Management would be needed. |

---

### TD-3: UI Not Covered by Automated Tests

| Attribute | Detail |
|-----------|--------|
| **Description** | The User Interface is verified only through manual testing. Rendering correctness, event handling, and visual feedback are not checked by the automated test suite. |
| **Impact** | Regressions in the UI layer may go undetected until a manual test is run. |
| **Accepted because** | Automated GUI testing for Tkinter requires either a virtual display or a specialised testing framework, both of which add complexity disproportionate to the project's scope. |
| **If revisited** | A virtual display (such as Xvfb on Linux) combined with pytest-based event simulation could cover basic UI interactions without changing the application architecture. |
