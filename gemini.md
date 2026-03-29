# Technical Overview: HTML5 Jigsaw Puzzle Game

This document provides a technical overview of `puzzle.html`, a fully self-contained web-based jigsaw puzzle application implementing complex interactions and procedural drawing algorithms within a single file.

## 1. Architecture & Stack
The application uses vanilla web technologies and does not depend on external frameworks.
*   **HTML5:** Semantic structure dividing the application into three main logical views (Setup, Game, Completion).
*   **CSS3:** Inline custom styling using CSS Custom Properties (variables) for design tokens.
*   **JavaScript (ES6):** Inline script handling all business logic, game state, canvas rendering, input handling, and audio synthesis.

## 2. Core Modules & Systems

### 2.1 State Management
The application manages its state through global variables. Key state includes:
*   **Game State:** Tracks the current phase (`setup`, `playing`, `completed`) and manages transitions.
*   **Grid Variables:** Tracks the target `numPieces`, `gridCols`, `gridRows`, and calculated dimensions like `boardWidth`, `puzzleWidth`, `pieceWidth`.
*   **Entity Collections:**
    *   `pieces`: Array of objects containing piece metadata (ID, target grid pos, current pos, tabs patterns).
    *   `groups`: Array managing connected clusters of pieces (`id`, `pieceIds`). This allows chunks of the puzzle to be moved together.

### 2.2 Rendering Engine (Canvas)
The puzzle rendering relies on the HTML5 `<canvas>` API with significant performance optimizations:
*   **Infinite Board Illusion:** Calculates a massive workspace relative to the viewport size.
*   **Procedural Jigsaw Paths:** The `createJigsawPathOnContext` function mathematically generates organic, interlocking puzzle piece shapes using complex Bezier curves (`bezierCurveTo`), neck widths, and tab sizes.
*   **Offscreen Caching (Performance):** To ensure 60FPS while dragging multiple pieces, the app uses a `pieceImageCache`. When the grid is generated, each piece is clipped from the source image and drawn to a dedicated invisible offscreen `<canvas>`. During the main render loop, `drawCachedPiece` uses `drawImage` to rapidly copy these pre-rendered bitmaps instead of recalculating complex Bezier paths every frame.
*   **Redraw Throttling:** Uses a `needsRedraw` flag with `requestAnimationFrame` to prevent unnecessary draw calls.

### 2.3 Input & Interaction Handling
The system normalizes unified event handling for both mouse and touch screens:
*   **Unified Coordinates:** `getEventCoords` computes normalized `x, y` for both standard mouse events and multi-touch data arrays.
*   **Pan & Drag Mechanics:** 
    *   Clicking a piece calculates an offset and targets a dragging group (`draggedGroup`).
    *   Clicking an empty board space initiates a pan operation, scrolling the container to navigate the infinite board.
*   **Event Throttling:** `handleMouseMove` is rate-limited to `~60fps` using timestamps (`mouseThrottle`) to prevent input spam dragging down the framerate.

### 2.4 Snapping & Collision Logic
Releasing a piece evaluates potential connection events:
*   **Dynamic Threshold:** `checkForSnapToNeighbors` determines a variable snap threshold derived from piece dimensions that guarantees fair snapping for puzzles of multiple sizes.
*   **Distance Calculation:** Checks distance delta against expected coordinate matrices (differences between actual coordinates versus intended grid target distances).
*   **Group Merging:** When pieces lock, `snapPiecesTogetherByGroup` merges the two respective groups, aligns all pieces to their precise delta offsets, and updates the `connectedPiecesCount`.

### 2.5 Audio System
*   **Web Audio API:** The app fully synthesizes its connection "thunk" sound dynamically, avoiding the need for external `.wav` files.
*   **Procedural Generation:** Uses a Triangle Oscillator for bass frequencies, paired with a randomly populated audio buffer acting as a noise burst for texture, routed through envelope-shaped Gain Nodes and Lowpass Filters.
*   **Interaction Unlocking:** Audio context instantiation is bound to a one-time global `click`/`touchstart` event listener to adhere to browser autoplay restriction policies.

## 3. User Interface (UI) Features
*   **Responsive Design:** CSS Grid/Flexbox layouts scale across device viewports.
*   **Theming Options:** Media queries enable automatic Dark Mode (`@media (prefers-color-scheme: dark)`) through swapping CSS variable tokens. 
*   **Controls:** Real-time Zoom slider (manipulates CSS `scale()`), dynamic Background color selector, timer implementation, completion tracking, and preview thumbnail overlays.
