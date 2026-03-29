# HTML5 Jigsaw Puzzle Game

A self-contained, high-performance HTML5 jigsaw puzzle application. This application provides a premium, interactive jigsaw experience using Vanilla JavaScript and deep canvas rendering optimizations.

## 🧩 Features

*   **Dynamic Image Processing:** Upload any custom image (`.jpg`, `.png`, etc.) directly from your device.
*   **Procedural Puzzle Geometry:** Edges are procedurally generated using complex S-curve Bezier algorithms. The tabs and necks are mathematically molded for a realistic "die-cut" physical cardboard shape entirely identical to physical premium puzzles (similar to styling found on Jigidi).
*   **3D Bevel & Emboss Engine:** Pieces feature realistic drop shadows, soft bright top-left highlights, and rich dark bottom-right shadows simulating physical lighting. 
*   **Intelligent Snap Physics:** Calculates snapping tolerances dynamically based on piece size and grouping contexts, making connections satisfying without being overly permissive.
*   **Fully self-contained / Zero Dependencies:** No React, No NPM, No Webpack, No libraries. Just 100% Vanilla ES6 JS, HTML5 Canvas, and modern CSS3 in a single portable `puzzle.html` file.
*   **Advanced Engine Optimization:** Uses hidden `<canvas>` offscreen-caching to store complex 3D Bezier shapes for blazing-fast 60FPS dragging of giant piece groupings.
*   **Web Audio Synth:** Replaces external `.wav` files with a dynamically synthesized thunk sound (Oscillator & Noise Generation) that plays when pieces snap together.

## 🚀 How to Run

Because this project is absolutely self-contained, no build tools or servers are required:

1. Download or clone this repository.
2. Open `puzzle.html` in your favorite modern web browser (Chrome, Firefox, Safari, Edge).
3. Upload an Image, choose your piece count, and start playing!

## 🛠️ Project Structure
*   `puzzle.html`: The core application code containing inline CSS styling tokens and the entire ES6 Game Loop and Canvas render engine.
*   `gemini.md`: Internal technical overview documentation detailing the architecture of the game loop and mechanics.
