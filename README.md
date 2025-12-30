# AI Pac-Man Game with Maze Solver Algorithms

A sophisticated AI-powered Pac-Man game implementation in C++ using OpenGL/GLUT for graphics. This project demonstrates various maze-solving algorithms including BFS (Breadth-First Search), A* (A-star), and risk-based pathfinding strategies.

## 🎮 Overview

This project features an intelligent Pac-Man game where:
- **Pac-Man** uses BFS and risk assessment algorithms to collect food while avoiding ghosts
- **Ghosts** use A* pathfinding to efficiently chase Pac-Man
- The game implements a **State Machine** pattern for character behavior management
- The maze is **randomly generated** with fair game validation

## ✨ Features

### Core Gameplay
- **Randomly Generated Mazes**: 50x50 grid with procedurally generated walls
- **Multiple Characters**: 1 Pac-Man and 3 Ghosts
- **Food Collection**: 4 food items scattered throughout the maze
- **Win Conditions**:
  - Pac-Man wins by collecting all food items
  - Ghosts win by surrounding Pac-Man with multiple attackers

### AI Algorithms

#### 1. **BFS (Breadth-First Search)**
- Used by Pac-Man to find the shortest path to food
- Limited depth search (DEPTH = 20) for performance
- Explores neighbors in all four directions (up, down, left, right)

#### 2. **A* (A-star) Algorithm**
- Used by Ghosts to efficiently chase Pac-Man
- Uses heuristic function (Euclidean distance) + step penalty
- Priority queue implementation for optimal pathfinding

#### 3. **Safe Distance Algorithm**
- Risk assessment for food collection
- Calculates safety score based on:
  - Distance to all ghosts
  - Distance to Pac-Man
  - Penalizes food too close to ghosts
- Selects the safest food item using a priority queue

### State Machine Pattern

The game implements a sophisticated state machine with three states:

1. **EatingState** (Pac-Man)
   - Default state for Pac-Man
   - Actively searches for and collects food
   - Transitions to ChaseState when ghosts are nearby

2. **ChaseState** (Both)
   - Ghosts chase Pac-Man using A*
   - Pac-Man runs away and evaluates safety of neighboring cells
   - Transitions to AttackState when distance < CLOSE_DISTANCE (1.4)

3. **AttackState** (Both)
   - Ghosts attempt to capture Pac-Man
   - Requires multiple ghosts (2+) within range to win
   - Pac-Man can defeat a single ghost after a delay (PacmanFightDelay = 2)
   - Transitions back to EatingState/ChaseState after combat

## 🏗️ Project Structure

```
AI_AFEKA_FINAL_PROJECT/
├── Graphics/
│   ├── main.cpp                 # Entry point, GLUT initialization
│   ├── Game.h/cpp               # Main game logic and orchestration
│   ├── Maze.h/cpp               # Maze generation, validation, and rendering
│   ├── Character.h/cpp          # Character AI and movement logic
│   ├── Cell.h/cpp               # Cell representation with A* properties
│   ├── CellComparator.h/cpp     # Comparator for A* priority queue
│   ├── SafeDistanceComparator.h/cpp  # Comparator for safe food selection
│   ├── State.h                  # State machine base class
│   ├── EatingState.h/cpp        # Pac-Man food collection state
│   ├── ChaseState.h/cpp         # Chasing/running state
│   ├── AttackState.h/cpp        # Combat state
│   └── [OpenGL dependencies]    # freeglut, GLEW libraries
├── Graphics.sln                 # Visual Studio solution file
└── README.md                    # This file
```

## 🔧 Requirements

### Dependencies
- **C++ Compiler**: Visual Studio 2019 or later (or compatible C++17 compiler)
- **OpenGL**: OpenGL 2.0 or higher
- **GLUT**: FreeGLUT library
- **GLEW**: OpenGL Extension Wrangler Library

### Libraries Included
- `freeglut.dll`, `freeglut.lib`, `freeglut.h` - GLUT implementation
- `glew32.dll`, `glew32.lib`, `glew.h` - GLEW library

## 🚀 Building and Running

### Windows (Visual Studio)

1. Open `Graphics.sln` in Visual Studio
2. Build the solution (Build → Build Solution or `Ctrl+Shift+B`)
3. Run the executable from `Debug/Graphics_current.exe` or press `F5`

### Manual Compilation

```bash
# Navigate to Graphics directory
cd Graphics

# Compile (example with g++)
g++ -o pacman_game main.cpp Game.cpp Maze.cpp Character.cpp Cell.cpp \
    CellComparator.cpp SafeDistanceComparator.cpp \
    ChaseState.cpp AttackState.cpp EatingState.cpp \
    -lfreeglut -lglew32 -lopengl32 -lglu32
```

## 🎯 How to Play

### Controls
- **Right-Click Menu**:
  - **"One Step"**: Execute one game step (ghosts move, then Pac-Man moves)
  - **"Run"**: Automatically play until game ends
  - **"Restart Game"**: Generate a new random maze and reset the game

### Game Mechanics

1. **Pac-Man Behavior**:
   - Evaluates all available food items for safety
   - Selects the safest food using risk assessment
   - Uses BFS to find path to selected food
   - Switches to escape mode when ghosts are close
   - Can defeat a single ghost after a 2-step delay

2. **Ghost Behavior**:
   - Use A* algorithm to find optimal path to Pac-Man
   - Transition to attack mode when within 1.4 units
   - Must coordinate (2+ ghosts) to capture Pac-Man

3. **Maze Generation**:
   - Random wall placement with different probabilities for odd/even rows
   - Fair game validation ensures all food is reachable
   - Regenerates if insufficient space is available

## 📊 Algorithm Details

### BFS Implementation (Pac-Man)
```cpp
// Limited depth BFS search
while (!pacmanQueue.empty() && currentDepth <= DEPTH) {
    currentCell = pacmanQueue.front();
    // Check neighbors (up, down, left, right)
    // Mark visited cells
    // Find path to food
}
```

### A* Implementation (Ghosts)
```cpp
// Priority queue with f = g + h
ghostsPQ.push(cell);  // Ordered by f-value
// g = step penalty (0.4 per step)
// h = Euclidean distance to Pac-Man
```

### Risk Assessment Formula
```cpp
risk = Σ(distance_to_ghosts) - 3 × distance_to_pacman
// Negative risk = safer food
// Food too close to ghosts gets -DBL_MAX (avoided)
```

## 🎨 Visual Representation

- **Black**: Empty space (SPACE)
- **Gray**: Walls
- **Yellow**: Pac-Man
- **Purple**: Ghosts (GHOST_1, GHOST_2, GHOST_3)
- **White**: Food items

## 🔬 Technical Highlights

1. **Object-Oriented Design**: Clean separation of concerns with classes for Game, Maze, Character, and States
2. **State Pattern**: Flexible state management for character behaviors
3. **Priority Queues**: Efficient implementation for A* and safe distance selection
4. **Fair Game Validation**: BFS-based validation ensures all food is reachable
5. **Performance Optimization**: Limited depth BFS, efficient data structures

## 📝 Constants and Configuration

Key constants defined in the code:
- `MSZ = 50`: Maze size (50x50 grid)
- `NUM_OF_GHOSTS = 3`: Number of ghost enemies
- `NUM_OF_FOOD = 4`: Number of food items
- `DEPTH = 20`: BFS search depth limit
- `STEP_PENALTY = 0.4`: Cost per step in A* algorithm
- `CLOSE_DISTANCE = 1.4`: Distance threshold for attack mode
- `PacmanFightDelay = 2`: Steps required for Pac-Man to defeat a ghost

## 🐛 Known Limitations

- BFS depth is limited to 20 for performance (may not find food in very large mazes)
- Maze generation is random and may occasionally create difficult layouts
- Single-threaded execution (no parallel pathfinding)

## 🔮 Future Enhancements

Potential improvements:
- Configurable maze size and difficulty
- Multiple difficulty levels
- Score tracking and high scores
- Sound effects and animations
- Multi-threaded pathfinding
- Machine learning for adaptive ghost behavior
- Custom maze editor

## 📄 License

This project appears to be an academic project. Please check with the authors for licensing information.

## 👥 Credits

Developed as part of the AI course at Afeka College of Engineering.

## 📚 References

- A* Pathfinding Algorithm
- Breadth-First Search Algorithm
- State Machine Design Pattern
- OpenGL/GLUT Graphics Programming

---

**Enjoy the game!** 🎮👻

