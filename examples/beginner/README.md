# Beginner PLingua Examples

This directory contains introductory PLingua examples designed for newcomers to membrane computing.

## What You'll Learn

These examples introduce the fundamental concepts of P-systems:

1. **Object Evolution** - How objects transform according to rules
2. **Multisets** - Working with objects that have multiplicities
3. **Membrane Structures** - Creating hierarchical compartments
4. **Communication** - Moving objects between membranes
5. **Indexed Objects** - Using objects with parameters
6. **Object Creation/Destruction** - Managing object lifecycles
7. **Rule Priorities** - Controlling execution order

## Examples

| File | Description | Key Concepts |
|------|-------------|--------------|
| `01_simple_evolution.pli` | Simplest P-system with one rule | Basic syntax, object evolution |
| `02_multiset_operations.pli` | Multiple objects and rules | Multisets, parallel evolution |
| `03_nested_membranes.pli` | Hierarchical structure | Membrane nesting, compartments |
| `04_communication.pli` | Moving objects between membranes | Send in/out operations |
| `05_counter.pli` | Simple counting mechanism | Indexed objects, ranges |
| `06_object_creation.pli` | Creating and destroying objects | Object lifecycle |
| `07_priority_rules.pli` | Rule execution order | Priorities, sequential execution |

## How to Run

To compile and simulate any example:

```bash
# Compile to JSON
plingua beginner/01_simple_evolution.pli -o output.json -f json

# Run simulation for 5 steps
psim --psystem output.json -s 5 -v 2
```

## Prerequisites

- Understanding of basic programming concepts
- No prior knowledge of membrane computing required

## Next Steps

After mastering these examples, move on to:
- **Intermediate** examples for algorithmic problem-solving
- **Advanced** examples for complex computational problems
