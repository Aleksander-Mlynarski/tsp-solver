# Travelling Salesman Problem (TSP) Solver

A C++ implementation of the **Travelling Salesman Problem**, developed as part of an **Advanced Object-Oriented Programming** laboratory course.

The program takes a cost matrix as input and returns the **shortest Hamiltonian cycle** — a route that visits every city exactly once and returns to the starting city.

---

## Problem Description

Given a complete graph where:

- **vertices** represent cities
- **edge weights** represent travel costs between cities

the goal is to find a cycle of minimum total cost that visits every vertex exactly once.

This is an NP-hard problem. Exact methods become impractical for large instances because the search space grows factorially with the number of cities.

---

## Approach

The solver uses a **Branch and Bound** algorithm with **cost matrix reduction**:

1. **Reduce** the cost matrix by subtracting row and column minima (Hungarian-style reduction).
2. **Select** the next edge to include — among zero-cost entries, pick the one with the highest penalty for not choosing it.
3. **Branch** into two subproblems:
   - **Left branch** — include the chosen edge in the path.
   - **Right branch** — forbid the chosen edge.
4. **Prune** branches whose lower bound exceeds the best solution found so far.
5. **Explore** the search tree using a **LIFO stack** until a 2×2 matrix remains, then reconstruct the full path.

When multiple optimal routes exist, all of them are returned.

---

## Project Structure

```
tsp-solver/
├── main.cpp              # Entry point and sample cost matrices
├── CMakeLists.txt        # Build configuration
├── include/
│   ├── TSP.hxx           # Core types, CostMatrix, StageState, solve_tsp()
│   └── tsp_setup.hxx     # Cost types, INF constant, IStageState interface
└── src/
    ├── TSP.cpp           # Branch-and-bound implementation
    └── tsp_setup.cpp     # Utility functions
```

### Key Components

| Component | Role |
|-----------|------|
| `CostMatrix` | Stores the cost matrix; supports row/column reduction and vertex penalty calculation |
| `StageState` | Represents a partial solution at a given stage of the search tree |
| `solve_tsp()` | Runs the Branch and Bound search and returns optimal solutions |
| `filter_solutions()` | Keeps only solutions with the minimum total cost |

---

## Requirements

- **C++17** compiler (GCC, Clang, or MSVC)
- **CMake** 3.13 or newer

---

## Build and Run

```bash
git clone https://github.com/Aleksander-Mlynarski/tsp-solver.git
cd tsp-solver
cmake -B build
cmake --build build
```

On Windows (Visual Studio generator):

```bash
cmake -B build
cmake --build build --config Debug
./build/Debug/tsp.exe
```

On Linux and macOS, the executable is typically located at `./build/tsp`.

---

## Usage

Edit the cost matrix in `main.cpp`. Use `INF` for forbidden transitions (the diagonal is always `INF` since a city cannot connect to itself):

```cpp
cost_matrix_t cm {
    {INF, 12,   3,  45,   6},
    { 78, INF, 90,  21,   3},
    {  5,  56, INF,  23,  98},
    { 12,   6,   8, INF,  34},
    {  3,  98,   3,   2, INF}
};
```

Run the program:

```bash
./build/tsp
```

Example output:

```
30 : 4 3 2 0 1
```

Each line shows the **total tour cost** followed by the **vertex indices** in visit order (0-based).

Additional test matrices are included as comments in `main.cpp`. Uncomment a different matrix to try other instances.

---

## Testing

The repository includes commented-out support for Google Test unit tests in `main.cpp`. To enable them, swap the active `main()` function as described in the file comments.

---

## Technologies

- C++17
- CMake
- Object-oriented design (`CostMatrix`, `StageState`, `IStageState`)

---

## Author

**Aleksander Młynarski**
