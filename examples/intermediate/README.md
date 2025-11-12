# Intermediate PLingua Examples

This directory contains intermediate-level PLingua examples that demonstrate algorithmic problem-solving using membrane computing.

## What You'll Learn

These examples show how to implement classic algorithms and computational patterns:

1. **Sequential Computation** - Multi-step calculations
2. **Recurrence Relations** - Computing sequences
3. **Conditional Logic** - Decision-making in rules
4. **Data Structures** - Representing arrays and sequences
5. **Iterative Algorithms** - Loops and repetition
6. **Mathematical Operations** - Complex calculations

## Examples

| File | Description | Algorithm | Complexity |
|------|-------------|-----------|------------|
| `01_factorial.pli` | Compute n! | Sequential multiplication | O(n) |
| `02_fibonacci.pli` | Generate Fibonacci sequence | Recurrence relation | O(n) |
| `03_prime_checker.pli` | Check if number is prime | Trial division | O(√n) |
| `04_string_reversal.pli` | Reverse a string | Index mapping | O(n) |
| `05_bubble_sort.pli` | Sort an array | Bubble sort | O(n²) |
| `06_gcd.pli` | Greatest common divisor | Euclidean algorithm | O(log n) |
| `07_power.pli` | Compute a^b | Repeated multiplication | O(b) |

## How to Run

Example compilation and simulation:

```bash
# Compile factorial example
plingua intermediate/01_factorial.pli -o factorial.json -f json

# Run simulation for 10 steps with verbose output
psim --psystem factorial.json -s 10 -v 2

# Try with different parameters
plingua intermediate/01_factorial.pli 7 -o factorial7.json -f json
```

## Key Concepts

### Indexed Objects
Objects can have indices to represent structured data:
```plingua
arr{0,5}    // Array element: arr[0] = 5
fib{3,2}    // Fibonacci: F(3) = 2
```

### Conditional Rules
Rules can include conditions:
```plingua
[a{x} --> b{x}]'1 : x > 5, x < 10;
```

### Multi-Step Algorithms
Use counters and state objects to track progress through algorithm steps.

## Prerequisites

- Completion of beginner examples
- Basic understanding of algorithms (sorting, recursion, etc.)
- Familiarity with algorithm complexity

## Next Steps

After mastering these examples, move on to:
- **Advanced** examples for NP-complete problems
- **Expert** examples for complex optimization
