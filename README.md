# Viking Chess: Copenhagen Edition (Web Engine)

An unbundled, lightweight web application for playing **Hnefatafl**, specifically adhering to the "Copenhagen" competitive standard. This engine is engineered exclusively with **native JavaScript** and the **HTML5 Canvas API**, avoiding all external libraries to demonstrate raw algorithmic efficiency and state management.

[**🔗 Launch Application**](https://copenhagenknefatafl.netlify.app)

## ⚔️ Game Synopsis

Hnefatafl is a historic table-top simulation of asymmetrical warfare. The **White forces** act as bodyguards, tasked with ferrying their King from the center to any of the four corner sanctuaries. The **Black forces**, possessing a 2-to-1 numerical advantage, must coordinate to capture the King before he flees.

This software strictly enforces the **Copenhagen Protocol**, a ruleset known for complex topological mechanics, including "Shieldwall" captures and total board isolation.

## ⚙️ Engineering Architecture

The codebase highlights fundamental computer science principles applied to game logic:

### 1. Graph Traversal for Isolation Logic
To validate the **"Encirclement"** victory condition (where Black creates a closed loop around White), the engine employs a **Breadth-First Search (BFS)** algorithm.
* **Methodology:** Instead of looking for the loop itself, the algorithm tries to "flood" the board with virtual air starting from the perimeter.
* **Outcome:** If the recursive pathfinding fails to touch the King or his guards, the system confirms they are hermetically sealed and awards the win to Black.
* **Dual Use:** This same logic is inverted to detect **"Fortress"** victories, where the King builds an unbreakable bunker against the board edge.

### 2. Hash-Based Stalemate Prevention
To avoid infinite gameplay cycles, the engine enforces a strict **Three-Fold Repetition** limit.
* **Snapshotting:** After every turn, the board arrangement is serialized into a unique string signature.
* **Frequency Mapping:** These signatures are stored in a frequency map. If the same snapshot is detected for a third time, the active player instantly forfeits.
* **Memory Efficiency:** This architecture facilitates a lightweight "Step Back" feature, storing history as compact strings rather than cloning the entire game object for every turn.

### 3. Vector-Based Edge Scanning
While standard captures check specific coordinates, the **"Shieldwall"** mechanic (removing a full rank of units along the boundary) requires dynamic vector evaluation.
* The system scans the board boundaries during every move validation.
* It calculates "Pincer" conditions (enemies anchoring both ends of the line) and "Frontal Occlusion" (enemies blocking the row) to trigger multi-unit elimination.

### 4. Native Rendering Pipeline
* **Direct Painting:** All visuals are drawn directly to the Canvas context, ensuring a smooth 60hz refresh rate without DOM manipulation overhead.
* **Resource Pre-loading:** A custom handler ensures all audio cues and graphical assets are fully cached in memory before the game loop initializes.
* **Dynamic View Layer:** Players can hot-swap between "Thematic" (pixel art) and "Abstract" (geometric) visual modes instantly.

## 🕹️ Interface & Rules

### Input Mapping
* **Mouse/Touch:** Drag pieces to valid tiles.
* **[R] Key:** Restart Match.
* **[U] Key:** Step Back (Undo).
* **[S] Key:** Switch Visual Style.
* **[H] Key:** Open Rulebook.

### Core Mechanics (Copenhagen Standard)
1.  **Locomotion:** All units slide orthogonally like Rooks.
2.  **Combat:** Enemies are removed by "pinching" them between two friendly units.
3.  **The Monarch:** The King is captured if surrounded on all four cardinals. He wins by touching a corner tile.
4.  **Shieldwalls:** A row of units against the wall can be captured simultaneously if flanked and blocked.
5.  **The Loop:** If Black forms a contiguous chain enclosing all White units, Black wins immediately.

## 📦 Deployment

This project requires no compilers, bundlers (Webpack/Vite), or package managers (NPM).

1.  **Download Source:**
    ```bash
    git clone [https://github.com/yourusername/hnefatafl-web.git](https://github.com/yourusername/hnefatafl-web.git)
    ```
2.  **Launch:**
    * Open `index.html` in a web browser.
    * *Recommendation:* To avoid CORS errors with audio assets, serve the files via a local host rather than the file system.

    ```bash
    # Example using Python 3
    python -m http.server 8000
    ```

## 📂 Directory Map

```text
/
├── index.html          # Application skeleton
├── assets/
│   ├── images/         # Game sprites
│   └── sounds/         # Interaction SFX
└── README.md           # This document
