# Advanced PLingua Examples

This directory contains advanced-level PLingua examples demonstrating solutions to complex computational problems, including NP-complete problems.

## What You'll Learn

These examples showcase:

1. **NP-Complete Problems** - Non-deterministic polynomial-time problems
2. **Membrane Division** - Creating multiple solution branches
3. **Graph Algorithms** - Traversal, connectivity, and path finding
4. **Optimization Problems** - Finding optimal solutions
5. **Parallel Computation** - Using membrane structure for parallelism
6. **Backtracking Search** - Exploring solution spaces

## Examples

| File | Description | Problem Class | Uses Division |
|------|-------------|---------------|---------------|
| `01_nqueens.pli` | N-Queens puzzle | Constraint satisfaction | Yes |
| `02_subset_sum.pli` | Subset sum problem | NP-complete | Yes |
| `03_knapsack.pli` | 0/1 Knapsack problem | NP-complete, optimization | Yes |
| `04_hamiltonian_path.pli` | Hamiltonian path | NP-complete | Yes |
| `05_graph_connectivity.pli` | Graph connectivity | Graph theory | No |
| `06_palindrome_complex.pli` | Advanced palindrome check | String processing | No |
| `07_matrix_multiply.pli` | Matrix multiplication | Linear algebra | No |

## Key Concepts

### Membrane Division

Division creates multiple computation branches, exploring different possibilities in parallel:

```plingua
@model<membrane_division>

// Creates two child membranes
[object]'membrane --> [option1]'membrane [option2]'membrane;
```

This enables:
- **Non-deterministic search** - Explore all possibilities
- **Exponential parallelism** - 2^n membranes for n choices
- **Natural pruning** - Invalid branches dissolve

### NP-Complete Problems

These examples demonstrate how P-systems can solve NP-complete problems in polynomial time (with exponential space trade-off):

- **SAT variants** - Boolean satisfiability
- **Subset Sum** - Find subset with target sum
- **Knapsack** - Optimize value within weight constraint
- **Hamiltonian Path** - Visit all vertices once

### Graph Algorithms

Representing graphs in PLingua:

```plingua
// Vertices as objects
@ms(1) = vertex{0}, vertex{1}, vertex{2};

// Edges as indexed objects
@ms(1) += edge{0,1}, edge{1,2}, edge{2,0};
```

## How to Run

```bash
# Compile N-Queens solver
plingua advanced/01_nqueens.pli -o nqueens.json -f json

# Run with verbose output to see membrane divisions
psim --psystem nqueens.json -s 20 -v 3

# For large problems, limit steps or use filters
psim --psystem nqueens.json -s 50 --max-membranes 10000
```

## Performance Considerations

- **Membrane explosion** - Division can create exponentially many membranes
- **Step limits** - Set appropriate step limits for convergence
- **Memory usage** - Large problems may require significant RAM
- **Pruning** - Invalid branches should be eliminated quickly

## Prerequisites

- Completion of intermediate examples
- Understanding of computational complexity (P, NP)
- Familiarity with graph theory
- Knowledge of classic algorithms

## Next Steps

After mastering these examples, explore:
- **Expert** examples for advanced NP problems
- **Experimental** examples for cutting-edge applications
