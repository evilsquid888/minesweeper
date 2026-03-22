# CLAUDE.md — Minesweeper for Android

## Project Overview

A classic Minesweeper game built as a native Android application in Java. The game features a 9x9 grid with 10 mines, a timer, mine counter, and the traditional smiley-button restart mechanism. Originally developed by VertexVerveInc.

## Language & Framework

- **Language:** Java
- **Platform:** Android (legacy Eclipse-style project, no Gradle)
- **Min SDK:** 4 (Android 1.6)
- **Target SDK:** 12 (Android 3.1)
- **Build target:** android-8 (Android 2.2)

## Build & Run

This is a legacy Android project (pre-Gradle, Eclipse/Ant era). There is no `build.gradle` or `pom.xml`.

To build with Ant (if Android SDK + Ant are available):

```bash
android update project --path .
ant debug
```

Alternatively, import the project into Android Studio as a legacy Eclipse project and let it convert to Gradle.

> **Note:** The project includes a `bin/classes.dex` and `bin/resources.ap_`, indicating a pre-built state is checked in.

## Project Structure

```
├── AndroidManifest.xml          # App manifest (package: com.VertexVerveInc.GamesSquid)
├── default.properties           # Legacy project properties (target=android-8)
├── src/com/VertexVerveInc/Games/
│   ├── MinesweeperGame.java     # Main Activity — game logic, UI, timer
│   └── Block.java               # Custom Button subclass representing a single cell
├── res/
│   ├── layout/main.xml          # Main layout (TableLayout-based grid + header)
│   ├── drawable-nodpi/          # Game images (smiley faces, grid squares, background)
│   ├── drawable-{l,m,h}dpi/    # Density-specific app icons
│   └── values/strings.xml       # String resources
├── assets/fonts/lcd2mono.ttf    # LCD-style font for timer and mine counter
└── bin/                         # Pre-built output (classes.dex, resources)
```

## Architecture

### Two-class design

- **`MinesweeperGame`** (`Activity`) — Contains all game logic:
  - Mine field creation with a border row/column for boundary calculations
  - Mine placement (randomized, excluding first-click location)
  - Recursive ripple-uncover (flood fill) for empty cells
  - Win/loss detection
  - Timer (Handler + Runnable callback loop)
  - Click → uncover cell; Long-press → cycle through flag/question-mark/blank

- **`Block`** (`extends Button`) — Represents a single grid cell with state:
  - `isCovered`, `isMined`, `isFlagged`, `isQuestionMarked`
  - `numberOfMinesInSurrounding` — count of adjacent mines
  - Color-coded number display (1=blue, 2=green, 3=red, etc.)

### Grid layout

The mine field uses a `(rows+2) x (cols+2)` array. The outer border cells are hidden and hold sentinel values (mine count = 9) to simplify boundary checks. Only inner cells (indices 1..N) are added to the UI `TableLayout`.

## Coding Conventions

- Java with Android SDK widget classes (no third-party UI libraries)
- Allman brace style (opening brace on its own line)
- Tab indentation
- Inline anonymous `OnClickListener` / `OnLongClickListener` classes
- No unit tests present
- Method names: camelCase; class names: PascalCase
- Note: there is a typo in `getNumberOfMinesInSorrounding()` (should be "Surrounding") — preserved for compatibility

## Dependencies

- **Google AdMob SDK** (legacy `com.google.ads.*`) — integrated in layout XML and Activity code
- No modern dependency management (no Gradle, Maven, or libs folder with JARs visible)

## Tests

No test suite exists in this project.
