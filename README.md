# 🎨 Paint for Kids

A desktop drawing application built with **C++** and **OOP principles**, designed as an educational tool for kids. The app has two main modes: a **Draw Mode** for creating and editing shapes, and a **Play Mode** with interactive games to help kids learn shapes and colors.

> Built using the CMU Graphics Library and Visual Studio on Windows.

---

## 📌 Table of Contents

- [About](#about)
- [Features](#features)
- [Project Architecture](#project-architecture)
- [Supported Shapes](#supported-shapes)
- [Draw Mode](#draw-mode)
- [Play Mode](#play-mode)
- [How to Build](#how-to-build)
- [Project Structure](#project-structure)

---

## About

**Paint for Kids** is a university-level OOP project that simulates a simplified painting application. Users can draw geometric shapes on a canvas, customize their colors, move and resize them, and even record and replay their drawing sessions. The app also includes a play mode with mini-games that test the user's ability to identify shapes by type, color, or both.

---

## Features

### 🖌️ Drawing
- Draw **5 different shapes**: Rectangle, Circle, Triangle, Square, and Hexagon
- Each shape is drawn by clicking points on the canvas
- Shapes support both **outline (draw) color** and **fill color**
- **6 available colors**: Red, Green, Blue, Yellow, Black, Orange

### ✏️ Editing
- **Select** a figure by clicking on it (highlighted in magenta)
- **Move** a figure to a new position by clicking the destination
- **Drag & Drop** — real-time dragging of figures across the canvas
- **Resize** — interactively resize a figure by dragging its corners
- **Change Draw Color** — change the outline/border color of a selected figure
- **Change Fill Color** — fill a selected figure with a chosen color
- **Delete** a selected figure
- **Clear All** — remove all figures from the canvas

### ↩️ Undo / Redo
- **Undo** up to the last 5 actions (add, delete, move, color change)
- **Redo** previously undone actions
- Performing a new action clears the redo history

### 💾 Save & Load
- **Save** the current drawing to a text file (stores colors, figure count, and each figure's data)
- **Load** a previously saved drawing from a file
- File format stores figure type, coordinates, draw color, and fill color

### 🎙️ Recording & Playback
- **Start Recording** — records up to 20 user actions into a queue
- **Stop Recording** — ends the recording session
- **Play Recording** — replays all recorded actions step-by-step with a 2-second delay between each
- Recording can only start at the beginning or after a clear-all
- Non-recordable actions (save, load, exit, switch mode) are filtered out

### 🔊 Voice Feedback
- Toggle **voice mode** on/off
- When enabled, drawing a shape plays an audio cue (`.wav` file) for the shape being drawn
- Separate voice files for: Rectangle, Circle, Triangle, Square, Hexagon

### 🔄 Mode Switching
- Switch between **Draw Mode** and **Play Mode** via toolbar buttons
- Each mode has its own dedicated toolbar with relevant icons

---

## Supported Shapes

| Shape     | Drawing Method                        | Identifier |
|-----------|---------------------------------------|------------|
| Rectangle | Click two opposite corners            | RECT       |
| Circle    | Click center, then a point on edge    | CIRC       |
| Triangle  | Click three vertices                  | TRIANG     |
| Square    | Click center (fixed half-length = 50) | SQ         |
| Hexagon   | Click center (fixed radius = 80)      | HEX        |

---

## Draw Mode

The draw mode toolbar includes **23 items**:

| Icon | Action |
|------|--------|
| Play Mode | Switch to play mode |
| Start Rec | Start recording actions |
| Stop Rec | Stop recording |
| Play Rec | Replay recorded actions |
| Select | Select a figure |
| Move | Move a figure to a point |
| Drag | Drag a figure in real-time |
| Resize | Resize a figure interactively |
| Clear | Clear all figures |
| Delete | Delete selected figure |
| Rectangle | Draw a rectangle |
| Circle | Draw a circle |
| Hexagon | Draw a hexagon |
| Triangle | Draw a triangle |
| Square | Draw a square |
| Undo | Undo last action |
| Redo | Redo last undone action |
| Save | Save drawing to file |
| Load | Load drawing from file |
| Draw Color | Change outline color |
| Fill Color | Change fill color |
| Voice | Toggle voice feedback |
| Exit | Exit the application |

---

## Play Mode

The play mode offers **3 educational mini-games**:

### 1. Pick by Figure Type
A random figure type is selected. The user must click all figures of that type on the canvas. Correct picks hide the figure; wrong picks increment the error counter.

### 2. Pick by Fill Color
A random figure is selected and the user must pick all figures that share the same fill color. Also supports picking non-filled figures.

### 3. Pick by Figure Type AND Fill Color
Combines both criteria — the user must pick all figures that match both the type and the fill color of the randomly selected figure.

**Scoring:** After completing a game, the app displays the percentage score:
```
Bravoooooo, You got XX%
```

---

## Project Architecture

The project follows an **Object-Oriented Architecture** with clear separation of concerns:

```
┌─────────────────────────────────┐
│            main.cpp             │  ← Entry point (action loop)
├─────────────────────────────────┤
│       ApplicationManager        │  ← Central controller
├──────────┬──────────┬───────────┤
│  Actions │ Figures  │    GUI    │
│  (logic) │ (shapes) │ (I/O)    │
└──────────┴──────────┴───────────┘
```

- **ApplicationManager** — manages figures list, undo/redo stacks, recording state, and dispatches actions
- **Action (base class)** — abstract class; each user action is a derived class with `Execute()`, `undo()`, and `clone()`
- **CFigure (base class)** — abstract class for all shapes; each shape implements `Draw()`, `InFigure()`, `Save()`, `Load()`, `move()`, `Resize()`, and `clone()`
- **Input / Output** — handle mouse clicks, keyboard input, toolbar rendering, figure drawing, and status bar messages

### Design Patterns Used
- **Command Pattern** — each action is encapsulated as an object with execute/undo
- **Prototype Pattern** — `clone()` method on actions and figures for deep copying
- **Queue Data Structure** — used in the recording system (`StartRecAction`) with enqueue/dequeue

---

## How to Build

### Requirements
- **Windows OS**
- **Visual Studio** (2010 or later)
- **CMU Graphics Library** (included in the project)

### Steps
1. Clone or download the repository
2. Open `PT-Project.sln` in Visual Studio
3. Build the solution (`Ctrl + Shift + B`)
4. Run the project (`F5`)

> ⚠️ This project uses the Windows API (`PlaySound`, `Sleep`) and the CMU Graphics Library, so it only runs on Windows.

---

## Project Structure

```
Paint-for-kids-OOP/
├── main.cpp                  # Entry point
├── ApplicationManager.cpp/h  # Central manager
├── DEFS.h                    # Enums & global definitions
├── helper.cpp/h              # Color serialization helpers
│
├── Actions/                  # All user actions
│   ├── Action.h              # Abstract base action
│   ├── AddRectAction         # Draw rectangle
│   ├── AddCircleAction       # Draw circle
│   ├── AddTriangleAction     # Draw triangle
│   ├── AddSquareAction       # Draw square
│   ├── AddHexaAction         # Draw hexagon
│   ├── SelectAction          # Select a figure
│   ├── DeletefigAction       # Delete a figure
│   ├── MoveAction            # Move to point
│   ├── DraggingMove          # Drag & drop
│   ├── ResizeAction          # Resize figure
│   ├── SelectDrawColour      # Change draw color
│   ├── SelectFillColour      # Change fill color
│   ├── Undo / Redo           # Undo & redo
│   ├── SaveAction            # Save to file
│   ├── LoadAction            # Load from file
│   ├── StartRecAction        # Start recording
│   ├── StopRecAction         # Stop recording
│   ├── PlayRecAction         # Play recording
│   ├── PlayVoiceAction       # Toggle voice
│   ├── PickFigAction         # Play: pick by type
│   ├── PickClrAction         # Play: pick by color
│   ├── PickClrFig            # Play: pick by type+color
│   ├── clearall              # Clear all figures
│   └── ExitAction            # Exit app
│
├── Figures/                  # Shape classes
│   ├── CFigure.cpp/h         # Abstract base figure
│   ├── CRectangle.cpp/h      # Rectangle
│   ├── CCircle.cpp/h         # Circle
│   ├── CTriangle.cpp/h       # Triangle
│   ├── CSquare.cpp/h         # Square
│   └── CHexagon.cpp/h        # Hexagon
│
├── GUI/                      # User interface
│   ├── Input.cpp/h           # Mouse & keyboard input
│   ├── Output.cpp/h          # Rendering & drawing
│   └── UI_Info.h             # UI constants & enums
│
├── CMUgraphicsLib/           # Graphics library
├── images/MenuItems/         # Toolbar icon images
├── Voices/                   # Shape voice audio files
├── save1.txt, save2.txt ...  # Sample save files
└── PT-Project.sln            # Visual Studio solution
```

---

## 🛠️ Built With

- **C++** — Core language
- **OOP** — Inheritance, Polymorphism, Encapsulation, Abstraction
- **CMU Graphics Library** — Window management and rendering
- **Windows API** — `PlaySound` for audio, `Sleep` for delays
- **Visual Studio** — IDE and build system
