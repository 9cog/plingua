# PLingua Examples

This directory contains a comprehensive collection of PLingua examples organized by difficulty level to help users learn membrane computing progressively.

## Directory Structure

```
examples/
├── beginner/          # Introductory examples (7 examples)
├── intermediate/      # Algorithmic problems (7 examples)
├── advanced/          # Complex problems & NP-complete (7 examples)
├── expert/            # NP-hard optimization (7 examples)
├── experimental/      # Cutting-edge applications (5 examples)
└── [legacy examples]  # Original examples from the repository
```

## Learning Path

### 1. Beginner Level (Start Here!)

**Directory**: `beginner/`  
**Prerequisites**: None  
**Focus**: Core P-Lingua syntax and basic membrane computing concepts

Learn fundamental concepts:
- Object evolution and transformation
- Multisets and multiplicities
- Membrane structures (compartments)
- Basic communication between membranes
- Indexed objects and ranges
- Object creation and destruction
- Rule priorities

**Time Investment**: 2-4 hours  
**Examples**: 7 well-documented programs

[→ Go to Beginner Examples](beginner/)

### 2. Intermediate Level

**Directory**: `intermediate/`  
**Prerequisites**: Completion of beginner examples  
**Focus**: Classic algorithms and computational patterns

Implement familiar algorithms:
- Factorial and Fibonacci
- Prime number checking
- String manipulation
- Sorting algorithms
- GCD computation
- Exponentiation

**Time Investment**: 4-8 hours  
**Examples**: 7 algorithmic problems

[→ Go to Intermediate Examples](intermediate/)

### 3. Advanced Level

**Directory**: `advanced/`  
**Prerequisites**: Understanding of algorithms and complexity  
**Focus**: NP-complete problems and advanced techniques

Solve hard computational problems:
- N-Queens puzzle
- Subset Sum problem
- 0/1 Knapsack
- Hamiltonian Path
- Graph algorithms
- Matrix operations

**Time Investment**: 8-12 hours  
**Examples**: 7 complex problems

[→ Go to Advanced Examples](advanced/)

### 4. Expert Level

**Directory**: `expert/`  
**Prerequisites**: Strong CS theory background  
**Focus**: NP-hard optimization and advanced graph problems

Master difficult problems:
- 3-SAT solver
- Traveling Salesman Problem (TSP)
- Vertex Cover
- Maximum Clique
- Graph Coloring
- Bin Packing

**Time Investment**: 12-20 hours  
**Examples**: 7 optimization problems

[→ Go to Expert Examples](expert/)

### 5. Experimental Level

**Directory**: `experimental/`  
**Prerequisites**: Research interest in novel applications  
**Focus**: Cutting-edge and emerging applications

Explore frontier topics:
- Neural network simulation
- Evolutionary algorithms
- Quantum computing simulation
- Stochastic processes
- Distributed consensus

**Time Investment**: Variable (research-oriented)  
**Examples**: 5 experimental programs

[→ Go to Experimental Examples](experimental/)

## Quick Start

```bash
# Navigate to examples directory
cd examples/

# Start with the simplest example
plingua beginner/01_simple_evolution.pli -o output.json -f json

# Simulate for 5 steps with verbose output
psim --psystem output.json -s 5 -v 2

# Try more examples
plingua beginner/05_counter.pli -o counter.json -f json
psim --psystem counter.json -s 10 -v 2
```

## Example Naming Convention

Examples follow a consistent naming pattern:
```
<level>/<number>_<descriptive_name>.pli
```

Example: `intermediate/01_factorial.pli`

## Learning Resources

### Documentation
- [Language Reference](../docs/guides/language_reference.md) - Complete syntax guide
- [Implementation Guide](../docs/guides/implementation_guide.md) - API and integration
- [Architecture Overview](../docs/architecture/overview.md) - System design

### External Resources
- [Membrane Computing Portal](http://ppage.psystems.eu/)
- [Research Group Website](http://www.gcn.us.es/)
- Academic papers on P-systems

## Key Concepts by Level

| Level | Key Concepts | Complexity | Applications |
|-------|--------------|------------|--------------|
| **Beginner** | Syntax, rules, membranes | O(1)-O(n) | Learning basics |
| **Intermediate** | Algorithms, iteration | O(n)-O(n²) | Standard problems |
| **Advanced** | NP-complete, division | Exponential | Hard problems |
| **Expert** | NP-hard, optimization | Exponential | Research-grade |
| **Experimental** | Novel applications | Variable | Cutting-edge |

## Compilation Tips

```bash
# Basic compilation
plingua input.pli -o output.json -f json

# With parameters to main()
plingua input.pli 10 20 -o output.json -f json

# With global variables
plingua input.pli -g "n=10 p=0.5" -o output.json -f json

# Verbose compilation
plingua input.pli -o output.json -f json -v 3

# Other output formats
plingua input.pli -o output.xml -f xml
plingua input.pli -o output.bin -f binary
plingua input.pli -o output.cpp -f c++
```

## Simulation Tips

```bash
# Basic simulation
psim --psystem output.json -s 100

# Randomized execution
psim --psystem output.json -r -s 100

# Verbose output (levels 1-3)
psim --psystem output.json -s 100 -v 2

# Save final configuration
psim --psystem output.json -s 100 -o final.json

# With custom initial configuration
psim --psystem output.json -c init.json -s 50
```

## Contributing Examples

We welcome contributions! When adding examples:

1. **Choose appropriate difficulty level**
2. **Follow naming conventions**
3. **Add comprehensive comments**
4. **Test compilation and simulation**
5. **Update README in that directory**
6. **Include complexity analysis**

## Problem Difficulty Guidelines

### Beginner
- Single concept demonstration
- < 30 lines of code
- No complex patterns
- Direct, obvious logic

### Intermediate
- Multi-step algorithms
- 30-50 lines of code
- Standard CS algorithms
- Clear algorithmic structure

### Advanced
- NP-complete problems
- 50-100 lines of code
- Membrane division
- Non-trivial patterns

### Expert
- NP-hard optimization
- 50-150 lines of code
- Advanced membrane techniques
- Research-level problems

### Experimental
- Novel applications
- Variable complexity
- May use advanced features
- Research-oriented

## License

All examples are licensed under GPL-3.0, same as PLingua.

```
Copyright (C) 2024  Research Group On Natural Computing
                    http://www.gcn.us.es
```

## Support

- **Questions**: Open an issue on GitHub
- **Bug Reports**: Use issue tracker
- **Discussions**: GitHub Discussions
- **Email**: perezh@us.es

## Acknowledgments

These examples were created to support the PLingua community in learning membrane computing. Thanks to:
- PLingua development team
- Research Group on Natural Computing
- University of Seville
- The membrane computing community

---

**Start your journey**: [Beginner Examples →](beginner/)

**Need help?** Check the [main README](../README.md) or [documentation](../docs/)
