# Blocks World: AI Search & Heuristic Planning

An algorithmic framework and comparative performance study for solving the classic **Blocks World** planning problem, developed in Python as part of the *Artificial Intelligence* curriculum in the Department of Applied Informatics at the **University of Macedonia**.

The project investigates state-space exploration across both uninformed and informed (heuristic) search strategies, benchmarking execution time, memory overhead (nodes expanded, tree depth), and solution optimality against standard **AIPS-2000 PDDL benchmarks**.

---

## Implemented Algorithms

The system implements four distinct search algorithms to evaluate the trade-offs between path optimality and computational complexity:

### 1. Uninformed Search Strategies
* **Breadth-First Search (BFS):** Explores states layer-by-layer. Guarantees shortest path (optimal step count) with exponential time and space complexity $O(b^d)$.
* **Depth-First Search (DFS):** Traverses individual branches to maximum depth before backtracking. Exhibits minimal linear memory footprint $O(b \cdot m)$, but produces non-optimal path lengths.

### 2. Heuristic (Informed) Search Strategies
* **Best-First Search (Greedy):** Prioritizes node expansion strictly based on heuristic cost estimation $f(n) = h(n)$. Delivers exceptional scaling on large problem instances without optimality guarantees.
* **A\* Search:** Balances historical path cost with forward heuristic evaluation $f(n) = g(n) + h(n)$. Guarantees strictly optimal solutions when paired with admissible heuristics.

---

## Custom Heuristic Functions

Three domain-specific heuristics were formulated and benchmarked:

* **Heuristic 1 ($h_1$ - Misplaced Blocks):** Calculates the direct count of blocks currently placed on an incorrect block or surface relative to the goal state.
* **Heuristic 2 ($h_2$ - Block Clearance & Multi-Move Penalty):** Assigns weighted penalties based on dependency depth (2 cost units for directly movable blocks; 4 units for blocks obstructed by other misplaced elements). Optimized for Best-First Search to accelerate navigation through complex multi-tier stacks.
* **Heuristic 3 ($h_3$ - Bidirectional Stack Consistency):** Evaluates relational alignment by penalizing blocks whose supporting or resting relationships deviate from the goal configuration (+2 for complete misalignment, +1 for partial). Formally **admissible** (never overestimates real path cost), preserving A\* optimality.

---

## Experimental Benchmark & Key Findings

Algorithms were evaluated on standard **AIPS-2000 Blocks World benchmarks** ranging from 4 to 60 blocks across three initial configurations:
1. **Type 0:** All blocks placed flat on the table.
2. **Type 1:** All blocks arranged in a single vertical stack.
3. **Type 2:** Blocks distributed across intermediate sub-stacks.

### Performance Summary

| Algorithm | Optimality | Max Solved Size | Memory Footprint | Key Observation |
| :--- | :---: | :---: | :---: | :--- |
| **BFS** | Optimal | 8 Blocks | Very High ($O(b^d)$) | Incurred exponential state-space explosion (>340k nodes at $N=8$). |
| **DFS** | Non-Optimal | 8 Blocks | Very Low ($O(b \cdot m)$) | Rapid runtime on shallow solutions, but produced excessively long action plans. |
| **A\*** ($h_3$) | Optimal | 14 Blocks | Moderate | Preserved strict optimality while expanding drastically fewer nodes than BFS. |
| **Best-First** ($h_2$) | Non-Optimal | **35 Blocks** | Low | Scaled near-linearly across complex stack structures due to dependency-aware penalties. |

---

## Usage Instructions

The solution requires Python 3.x and executes via the command-line interface:

```bash
python bw.py <algorithm> <problem_file> <solution_file>
```

### Parameters:
* `<algorithm>`: `breadth` | `depth` | `best` | `astar`
* `<problem_file>`: Path to a PDDL benchmark file (e.g., `probBLOCKS-4-0.pddl`)
* `<solution_file>`: Name of the generated output text file containing path execution metrics.

### Example Run:
```bash
python bw.py astar probBLOCKS-8-0.pddl sol.txt
```

---

## Author

* **Valentinos Goudas** (Student ID: ics23170)  
  Department of Applied Informatics, University of Macedonia
