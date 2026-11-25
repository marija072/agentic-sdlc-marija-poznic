# 12 - Glossary

Terms are listed alphabetically. Each definition reflects the meaning used
consistently across Chapters 1–11 of this architecture documentation.

---

## A

**Adjacent Mine Count**
The number of mines in the eight cells surrounding a given cell (horizontally,
vertically, and diagonally). Calculating this value for every safe cell is the
core algorithm of the Coding Dojo Minesweeper kata and the primary
responsibility of Board Management.

**arc42**
A template for software architecture documentation consisting of twelve
chapters. Used as the documentation structure for this project.

---

## B

**Board**
The rectangular grid of cells that forms the playing field. The board has a
fixed number of rows and columns, a set of mine positions, and an adjacent
mine count for each safe cell. Owned and managed by Board Management.

**Board Management**
The component responsible for holding the grid state, recording mine
positions, and calculating adjacent mine counts. It has no dependency on
Game Logic or the User Interface. See Chapter 5.

---

## C

**C4 Model**
A hierarchical approach to software architecture diagrams using four levels:
System Context, Container, Component, and Code. This project uses the first
three levels. Diagrams are written in PlantUML using the C4-PlantUML library.

**Cascade Reveal**
The automatic reveal of neighbouring cells that occurs when a revealed cell
has an adjacent mine count of zero. The process continues iteratively for
each newly revealed cell that also has a zero count. Implemented as a
flood-fill in Game Logic. See Chapter 6, Scenario 2.

**Cell**
A single position on the board, identified by its row and column. A cell is
in one of the following states: hidden, revealed, or flagged. A cell either
contains a mine or is safe.

**Component**
In the C4 model, a named unit of responsibility within a container. This
project has three components: Board Management, Game Logic, and User
Interface. See Chapter 5.

**Container**
In the C4 model, a deployable or runnable unit. This project has one
container: the Minesweeper Desktop Application. See Chapter 5.

**Coding Dojo Minesweeper Kata**
A programming exercise that defines the core problem: given a grid with mine
positions, calculate the adjacent mine count for every safe cell. This
project implements the kata algorithm in Board Management and extends it into
a fully interactive game.

---

## D

**Desktop Application**
The single deployable unit of the Minesweeper system. It runs as a Python
process on the player's local machine and contains all three components.
See Chapters 5 and 7.

---

## F

**Flag**
A marker placed on a hidden cell by the player to indicate a suspected mine.
A flagged cell cannot be revealed until the flag is removed. Flagging and
unflagging are managed by Game Logic and recorded by Board Management.
See Chapter 6, Scenario 4.

**Flood-fill**
The iterative algorithm used to implement cascade reveal. Game Logic
maintains a work queue of cells to process; cells with a zero adjacent mine
count add their unrevealed neighbours to the queue. See Chapter 6, Scenario 2.

---

## G

**Game Logic**
The component responsible for processing player actions, applying game rules,
managing the cascade reveal, and determining the win or loss outcome. It
depends on Board Management and has no Tkinter dependency. See Chapter 5.

**Game State**
The current condition of a game session. The possible states are: in
progress, WON (all safe cells revealed), and LOST (a mine cell was revealed).
Owned by Game Logic.

---

## M

**Mine**
A hidden hazard placed on the board at the start of a game. Revealing a cell
that contains a mine ends the game with a loss. Mine positions are set by
Board Management during board initialisation.

**Minefield**
The full board as presented to the player — a grid of hidden cells, some of
which contain mines. Synonymous with Board in this documentation.

---

## P

**PlantUML**
A text-based diagramming tool used to author and render architecture diagrams
in this project. Sequence diagrams and C4 diagrams are written as `.puml`
files and rendered to `.svg` files.

**Player**
The human who interacts with the Minesweeper application. The only external
actor in the system. See Chapter 3.

---

## R

**Reveal**
The player action of uncovering a hidden cell by left-clicking it. If the
cell contains a mine, the game ends with a loss. If the cell is safe, its
adjacent mine count is displayed. If the count is zero, a cascade reveal
occurs. Processed by Game Logic.

---

## S

**Safe Cell**
A cell that does not contain a mine. Revealing all safe cells wins the game.

**Separation of Concerns**
The architectural principle of assigning each distinct responsibility to
exactly one component. In this project it means Board Management holds grid
data, Game Logic holds rules, and the User Interface holds rendering and
input handling. No component crosses into another's responsibility.
See ADR-001, Chapters 4 and 5.

**System Context**
In the C4 model, the outermost view of a system showing its boundary and
its relationships with external actors. This project's system context
contains one actor (the Player) and one system (the Minesweeper System).
See Chapter 3.

---

## T

**Tkinter**
The Python standard library module used to build the graphical interface.
It wraps the Tk GUI toolkit and provides the window, grid display, and event
handling for the User Interface component. Selected in ADR-001.

---

## U

**User Interface**
The component responsible for rendering the board using Tkinter and
translating player mouse events into calls to Game Logic. It contains no
game rules and is the only component with a Tkinter dependency. See Chapter 5.

---

## W

**Win Condition**
The state reached when all safe cells have been revealed, regardless of
whether mines have been flagged. Evaluated by Game Logic after each reveal
action. See Chapter 1, Functional Goal 7, and Chapter 10, QR-4.
