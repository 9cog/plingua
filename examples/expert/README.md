# Expert PLingua Examples

This directory contains expert-level PLingua examples demonstrating solutions to NP-hard and NP-complete optimization problems using advanced membrane computing techniques.

## What You'll Learn

These examples showcase:

1. **NP-Hard Optimization** - Problems harder than NP-complete
2. **SAT Variants** - Boolean satisfiability in various forms
3. **Graph Optimization** - Coloring, covering, packing problems
4. **Combinatorial Optimization** - Finding optimal solutions in large search spaces
5. **Constraint Satisfaction** - Meeting multiple constraints simultaneously
6. **Resource Allocation** - Optimal distribution of limited resources

## Examples

| File | Description | Complexity | Optimization |
|------|-------------|------------|--------------|
| `01_3sat.pli` | 3-SAT solver | NP-complete | Decision |
| `02_tsp.pli` | Traveling salesman | NP-hard | Minimize distance |
| `03_vertex_cover.pli` | Minimum vertex cover | NP-complete | Minimize vertices |
| `04_max_clique.pli` | Maximum clique | NP-hard | Maximize size |
| `05_independent_set.pli` | Maximum independent set | NP-complete | Maximize size |
| `06_graph_coloring.pli` | Graph coloring | NP-complete | Minimize colors |
| `07_bin_packing.pli` | Bin packing | NP-hard | Minimize bins |

## Theoretical Background

### Complexity Classes

- **P**: Problems solvable in polynomial time
- **NP**: Problems verifiable in polynomial time
- **NP-complete**: Hardest problems in NP
- **NP-hard**: At least as hard as NP-complete

### P-Systems and Complexity

P-systems with membrane division can solve NP-complete problems in polynomial time by:

1. **Trading time for space** - Create exponentially many membranes
2. **Massive parallelism** - Explore all possibilities simultaneously
3. **Natural pruning** - Invalid solutions dissolve
4. **Filtering** - Select optimal among valid solutions

**Caveat**: This requires exponential physical resources (membranes).

## Advanced Techniques

### Non-Deterministic Choice

```plingua
// Branch into multiple possibilities
[state{i}]'2 --> 
    [state{i}, choice{0}]'2    // Option 1
    [state{i}, choice{1}]'2    // Option 2
    ...
```

### Constraint Checking

```plingua
// Invalidate solutions violating constraints
[constraint{x}, violation{x}]'2 --> [invalid]'2;
```

### Optimization Filters

```plingua
// Keep only better solutions
[cost{c1}]'2, [cost{c2}]'2 --> [cost{c1}]'2 : c1 < c2;
```

### Bitmasking for Sets

```plingua
// Use bitmasks to represent sets efficiently
@ms(1) = selected{0b1011};  // Vertices 0,1,3 selected
```

## How to Run

```bash
# Compile 3-SAT solver
plingua expert/01_3sat.pli -o 3sat.json -f json

# Run with appropriate step limit
psim --psystem 3sat.json -s 50 -v 2

# For TSP with custom parameters
plingua expert/02_tsp.pli 5 -o tsp5.json -f json
psim --psystem tsp5.json -s 100
```

## Performance Warnings

⚠️ **Exponential Growth**: These examples can create millions of membranes
⚠️ **Memory Intensive**: Large instances may exceed available RAM
⚠️ **Long Runtime**: Simulation may take minutes to hours
⚠️ **Step Limits**: Set appropriate limits based on problem size

## Optimization Strategies

1. **Early Pruning** - Eliminate invalid branches quickly
2. **Constraint Propagation** - Apply constraints incrementally
3. **Symmetry Breaking** - Reduce equivalent solutions
4. **Heuristic Ordering** - Process variables in smart order

## Real-World Applications

- **3-SAT**: Hardware verification, AI planning
- **TSP**: Logistics, circuit design, astronomy
- **Vertex Cover**: Network security, bioinformatics
- **Clique**: Social network analysis, protein structure
- **Independent Set**: Wireless networks, scheduling
- **Graph Coloring**: Register allocation, timetabling
- **Bin Packing**: Cloud computing, cutting stock

## Prerequisites

- Completion of advanced examples
- Strong understanding of computational complexity
- Familiarity with NP-completeness proofs
- Graph theory knowledge
- Understanding of optimization techniques

## Further Reading

- *Computers and Intractability* by Garey & Johnson
- *The P=NP Question* (Millennium Prize Problem)
- *P Systems with Active Membranes*
- Research papers on membrane computing complexity

## Next Steps

Explore:
- **Experimental** examples for cutting-edge applications
- Hybrid algorithms combining P-systems with other techniques
- Probabilistic and stochastic variants
