# Brick Breaker Game

## Overview
> **Project status:** This is a Visual Studio C++/OpenGL course project, not a deployed application. The checked-in solution is under `8-2_Assignment/`. Build success, integration tests, SQLite persistence and GoogleTest execution have not been verified in the current repository checks. Treat the feature descriptions below as project documentation, not a current test report.

## Repository layout and review

- [Visual Studio solution](8-2_Assignment/8-2_Assignment.sln)
- [C++ source and headers](8-2_Assignment/)
- [Game logic](8-2_Assignment/Collision.cpp) and [data access](8-2_Assignment/DataAccess.cpp)

Open the solution in Visual Studio with matching OpenGL, GLFW, GLEW and SQLite dependencies. Paths mentioned below are local examples and may need adjustment for your installation. The repository includes some generated Visual Studio artifacts; these are historical files and should not be mistaken for source.

This project is a modernized implementation of a classic brick breaker game written in **C++** with OpenGL for rendering and GLFW for window and input handling.  
Originally developed as a single monolithic file, the codebase has been refactored into modular components that separate game logic, rendering, input, and entity management.  
The game demonstrates skills in software design, algorithms, data structures, and database integration, and is included as an artifact for my ePortfolio.

## Features
- **Modular Design**: Clear separation of classes such as `Brick`, `Circle` (ball), `Paddle`, and supporting systems like `Collision`, `Grid`, and `Level`.
- **Modern C++ Practices**: Use of `enum class`, smart pointers, `const` correctness, `<chrono>` for delta time, and `<random>` for reproducible randomness.
- **Algorithms & Data Structures**:  
  - Spatial partitioning grid to reduce collision checks from O(n*m) to localized queries.  
  - Continuous collision detection to prevent tunneling issues.  
- **Graphics**:  
  - OpenGL rendering via GLFW context.  
  - Future-proofed renderer class to allow migration away from immediate mode to modern OpenGL.  
- **Persistence with SQLite**:  
  - Scores table storing player name, points, date, and duration.  
  - Settings table for adjustable parameters like window size and difficulty.  
  - Data access layer using prepared statements for security.  
  - In-game leaderboard that displays top scores.  
- **Testing & Tooling**:  
  - Unit tests for collision logic with GoogleTest.  
  - Integration tests for database operations.  
  - Configurable build with CMake for cross-platform support.  
  - Static analysis and formatting configuration applied.

## Controls
- **Arrow Keys**: Move paddle left and right.  
- **Spacebar**: Launch the ball (or spawn a new one if attached).  
- **Escape**: Exit the game.  
- **P**: Pause/unpause gameplay.

## Installation & Setup

### Prerequisites
- Visual Studio 2022 (or a modern C++17 compiler)
- CMake (if building outside of Visual Studio)
- OpenGL development libraries
- [GLFW 3.4](https://www.glfw.org/) and [GLEW 2.2](http://glew.sourceforge.net/) binaries
- SQLite3 (via vcpkg, amalgamation source, or prebuilt DLL/lib)

### Building
1. Clone the repository.
2. Open the solution in Visual Studio or configure with CMake.
3. Make sure the following include paths are set in project properties:
   - `glfw-3.4.bin.WIN64/include`
   - `glew-2.2.0/include`
   - SQLite headers (from amalgamation or vcpkg)
4. Link against:
   - `glfw3dll.lib` or `glfw3_mt.lib`
   - `glew32.lib` or `glew32s.lib`
   - `opengl32.lib`
   - `sqlite3.lib` (if not building from `sqlite3.c` directly)
5. Copy the required DLLs (`glfw3.dll`, `glew32.dll`, `sqlite3.dll`) next to your executable if using dynamic builds.
6. Build and run the project.

### Database Setup
On first run, the game creates `game.db` in the executable folder with the required schema:
- `scores` table for leaderboard entries
- `settings` table for key-value configuration

You can inspect the database with the SQLite CLI:
```sh
sqlite3 game.db
.tables
SELECT * FROM scores ORDER BY points DESC LIMIT 10;
