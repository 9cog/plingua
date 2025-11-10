# P-Lingua Developer Implementation Guide

## Table of Contents
1. [Getting Started](#getting-started)
2. [Language Syntax Reference](#language-syntax-reference)
3. [API Documentation](#api-documentation)
4. [Integration Guide](#integration-guide)
5. [Code Examples](#code-examples)
6. [Best Practices](#best-practices)
7. [Troubleshooting](#troubleshooting)

---

## Getting Started

### Prerequisites

Before implementing P-Lingua as a programming language, ensure you have:

```bash
# Ubuntu/Debian
sudo apt-get install build-essential flex bison \
    libboost-filesystem-dev libboost-program-options-dev libfl-dev

# Fedora/RHEL
sudo dnf install gcc-c++ flex bison boost-devel flex-devel

# macOS (with Homebrew)
brew install flex bison boost
```

### Building P-Lingua

```bash
# Clone the repository
git clone https://github.com/RGNC/plingua.git
cd plingua

# Build the grammar
make grammar

# Build the compiler
make compiler

# Build the simulator
make simulator

# Build everything
make all

# Install system-wide (optional)
sudo make install
```

After installation, two binaries are available:
- `plingua` - Compiler/Transpiler
- `psim` - Simulator

---

## Language Syntax Reference

### File Structure

A P-Lingua program (.pli file) has the following structure:

```plingua
/* Comments */
// Line comment

/* Include other files */
@include "file.pli"
@include "model.pli"

/* Define global variables */
let x = 10;
let y = 20.5;
let name = "value";

/* Define modules (functions) */
def moduleName(param1, param2, ...) {
    // Module body
    return value;
}

/* Define patterns (rule templates) */
!patternName {
    // Rule patterns
}

/* Define model semantics */
@model(modelName) = pattern1, @n{pattern2, pattern3};

/* Main entry point */
def main() {
    // Initialize membrane structure
    @mu = [ ]'label;
    
    // Initialize multisets
    @ms(label) += object1, object2{3}, ...;
    
    // Define rules
    [lhs --> rhs]'label;
}
```

### Data Types

#### Primitive Types

```plingua
// Integer types
int_t x = 42;
long_t y = 1000000;

// Floating point
double_t pi = 3.14159;

// String
string_t name = "membrane";

// Boolean (0 or 1)
let flag = true;  // or false
```

#### Structured Types

**Multiset**: Collection of objects with multiplicities
```plingua
a, b{2}, c{3}  // a appears once, b twice, c three times
```

**Label**: Hierarchical membrane identifier
```plingua
'skin
'nucleus.inner.1
```

**Membrane**: Hierarchical structure
```plingua
[ multiset ]'label
[ a, b ]'outer [ c ]'inner
```

### Objects

Objects are atomic entities with optional indices:

```plingua
a           // Simple object
b{i}        // Indexed object (requires unrolling)
protein_X   // Named object
e{i,j}      // Multi-indexed object
```

### Membranes

#### Membrane Structure Definition

```plingua
// Skin membrane with inner membranes
@mu = [ ]'skin [ ]'cell1 [ ]'cell2;

// Nested hierarchy
@mu = [ ]'environment 
      [ ]'organism 
        [ ]'cell 
          [ ]'nucleus;
```

#### Membrane Charges

Membranes can have electrical charges:

```plingua
[ ]'label        // Neutral (0)
[ ]'label^+      // Positive (+1)
[ ]'label^-      // Negative (-1)
```

### Multisets

#### Initial Multisets

```plingua
// Set multiset for a specific label
@ms(skin) += a{10}, b{5}, c;

// Accumulative assignment
@ms(cell) += d{2};
@ms(cell) += e{3};  // cell now has d{2}, e{3}
```

#### Multiset Operations

```plingua
// In rule context
[a, b{2} --> c{3}, d]'label
//^ consumes    ^ produces
```

### Rules

Rules specify how membranes and objects evolve.

#### Standard Evolution Rules

```plingua
// Format: [LHS --> RHS]'label^charge
[a --> b]'cell            // a becomes b in cell
[a, b --> c{2}]'cell      // a+b become 2*c
[a --> b, c]'cell         // a becomes b and c
```

#### Communication Rules

```plingua
// Send in (from parent to membrane)
a [ ] --> [b]

// Send out (from membrane to parent)
[a] --> b [ ]

// Bidirectional
[a]'cell1 <--> [b]'cell2
```

#### Dissolution Rules

```plingua
// Membrane dissolves, contents go to parent
[a --> @d]'cell          // Mark for dissolution
[a] --> b                // Dissolve and produce b outside
```

#### Division Rules

```plingua
// Membrane divides into two
[a] --> [b] [c]          // Divide with different contents
[a] --> [b] [b]          // Divide with identical contents
[a] --> [b{1}] [b{2}]    // Divide with indexed differences
```

#### Inner Membrane Rules

```plingua
// Affect inner membranes
[ [a]'inner --> [b]'inner ]'outer
```

### Rule Features

Rules can have additional features:

```plingua
// Priority (higher executes first)
[a --> b]'cell : priority=5;

// Probability (stochastic P-systems)
[a --> b]'cell : probability=0.7;

// Pattern membership
[a --> b]'cell : pattern="evolution";
```

### Ranges

Ranges enable parameterized object generation:

```plingua
// Object range
a{i} : 1 <= i <= 10          // Generates a{1}, a{2}, ..., a{10}

// Nested ranges
e{i,j} : i < j, 1 <= i <= n, 1 <= j <= n

// In multisets
@ms(cell) += protein{k} : 1 <= k <= 5;

// In rules
[a{i} --> b{i}]'cell : 1 <= i <= n;
```

### Patterns

Patterns are reusable rule templates:

```plingua
// Define pattern
!evolution {
    [a --> b]'cell;
    [b --> c]'cell;
}

// Define another pattern
!communication {
    a [x] --> [x, a]'cell;
    [y]'cell --> y [ ]'cell;
}

// Use in model
@model(myModel) = evolution, @2{communication};
```

### Modules (Functions)

```plingua
// Define module
def factorial(n) {
    if (n <= 1) {
        return 1;
    } else {
        return n * call factorial(n - 1);
    }
}

// Call module
let result = call factorial(5);

// Main module (entry point)
def main() {
    let size = 10;
    call buildSystem(size);
}
```

### Control Flow

```plingua
// If-else
if (condition) {
    // statements
} else {
    // statements
}

// While loop
while (condition) {
    // statements
}

// Do-while loop
do {
    // statements
} while (condition);

// For loop
for (let i = 0; i < 10; i++) {
    // statements
}
```

### Expressions

```plingua
// Arithmetic
x + y, x - y, x * y, x / y, x % y

// Comparison
x < y, x <= y, x > y, x >= y, x == y, x != y

// Logical
x && y, x || y, !x

// Bitwise
x & y, x | y, x ^ y, ~x, x << n, x >> n

// Assignment
x = y
x += y, x -= y, x *= y, x /= y, x %= y
x++, ++x, x--, --x
```

---

## API Documentation

### Compiler API (plingua)

#### Command Line Interface

```bash
plingua [options] input.pli [args...]
```

**Options:**
- `-h, --help` - Show help message
- `-a, --about` - Show about information
- `-L, --license` - Show GPL license
- `-v, --verbosity <level>` - Set verbosity (0-7)
- `-I, --include <path>` - Add include search path
- `-o, --output <file>` - Set output file
- `-f, --format <format>` - Set output format
- `-l, --list` - List available output formats
- `-g, --global <var=value>` - Set global variable
- `-n, --no-color` - Disable colored output

**Output Formats:**
- `json` - JSON format (human-readable)
- `xml` - XML format
- `bin` - Binary format (compact)
- `bin2` - Portable binary format
- `c++` - Generate C++ simulation code

**Examples:**

```bash
# Compile to JSON with verbosity
plingua example.pli -o output.json -f json -v 3

# Pass arguments to main()
plingua example.pli 10 20 -o output.json -f json

# Set global variables
plingua example.pli -g "n=10 p=0.5" -o output.json -f json

# Use include paths
plingua example.pli -I ./models -I ../common -o output.json -f json

# Generate C++ code
plingua example.pli -o simulator.cpp -f c++
```

### Simulator API (psim)

#### Command Line Interface

```bash
psim [options] --psystem <file>
```

**Options:**
- `-h, --help` - Show help message
- `-a, --about` - Show about information
- `-l, --license` - Show GPL license
- `-v, --verbosity <level>` - Set verbosity (0-7)
- `-r, --randomized` - Enable randomized execution
- `-s, --steps <n>` - Maximum simulation steps
- `-c, --configuration <file>` - Initial configuration file
- `-o, --output <file>` - Output configuration file
- `--psystem <file>` - P-system specification file

**Examples:**

```bash
# Run simulation
psim --psystem output.json -s 100 -v 2

# Use custom initial configuration
psim --psystem output.json -c init.json -s 50

# Save final configuration
psim --psystem output.json -s 100 -o final.json

# Randomized execution
psim --psystem output.json -r -s 100
```

### C++ Library API

#### Including P-Lingua Headers

```cpp
#include <serialization.hpp>
#include <parser/parser.hpp>
#include <simulator/simulator.hpp>
```

#### Loading P-Systems

```cpp
#include <serialization.hpp>

using namespace plingua;

// Load from JSON
File file;
loadFromJsonFile("psystem.json", file);

// Load from XML
loadFromXmlFile("psystem.xml", file);

// Load from Binary
loadFromBinaryFile("psystem.bin", file);

// Access P-system
Psystem& psystem = file.psystem;
```

#### Creating a Custom Simulator

```cpp
#include <simulator/simulator.hpp>

using namespace plingua::simulator;

class MySimulator : public Simulator {
public:
    // Override rule selection
    void selectRules() override {
        // Custom selection logic
        Simulator::selectRules(); // Call base implementation
    }
    
    // Override rule execution
    void executeRules() override {
        // Custom execution logic
        Simulator::executeRules(); // Call base implementation
    }
};

int main(int argc, char* argv[]) {
    MySimulator sim;
    if (sim.parse(argc, argv)) {
        while (sim.ok()) {
            sim.step();
        }
    }
    return 0;
}
```

#### Data Structure Access

```cpp
// Access membrane structure
const Membrane& structure = file.psystem.structure;

// Access rules
for (const Rule& rule : file.psystem.rules) {
    // Process rule
}

// Access multisets
for (const auto& [label, multiset] : file.psystem.multisets) {
    for (const auto& [object, multiplicity] : multiset) {
        std::cout << object << " : " << multiplicity.raw() << std::endl;
    }
}

// Access configuration during simulation
const Configuration& config = simulator.getCurrentConfiguration();
for (const CMembrane& membrane : config.membranes) {
    std::cout << "Membrane: ";
    for (const auto& ls : membrane.label) {
        std::cout << ls.str() << ".";
    }
    std::cout << std::endl;
}
```

---

## Integration Guide

### Integrating P-Lingua into Your Application

#### Option 1: Command-Line Integration

Call P-Lingua tools from your application:

```python
import subprocess
import json

# Compile P-Lingua source
subprocess.run([
    'plingua', 
    'input.pli', 
    '-o', 'output.json', 
    '-f', 'json'
])

# Load compiled P-system
with open('output.json') as f:
    psystem = json.load(f)

# Run simulation
subprocess.run([
    'psim',
    '--psystem', 'output.json',
    '-s', '100',
    '-o', 'result.json'
])

# Load results
with open('result.json') as f:
    result = json.load(f)
```

#### Option 2: C++ Library Integration

Link against P-Lingua libraries:

```cpp
// Your application
#include <serialization.hpp>
#include <simulator/simulator.hpp>

void runPSystem(const std::string& pliFile) {
    using namespace plingua;
    
    // Compile P-Lingua source
    parser::Parser& parser = parser::PARSER;
    // ... compilation logic
    
    // Run simulation
    simulator::Simulator sim;
    // ... simulation logic
}
```

**Compilation flags:**
```bash
g++ -std=c++11 \
    -I/usr/local/include/plingua \
    -I/usr/local/include \
    -L/usr/local/lib \
    myapp.cpp \
    -lboost_filesystem \
    -lboost_program_options \
    -o myapp
```

#### Option 3: JSON-Based Integration

Use P-Lingua's JSON output for language-agnostic integration:

```javascript
// Node.js example
const fs = require('fs');
const { execSync } = require('child_process');

// Compile
execSync('plingua input.pli -o output.json -f json');

// Load P-system
const psystem = JSON.parse(fs.readFileSync('output.json'));

// Custom processing
function processRules(rules) {
    // Implement custom rule processing
}

// Custom simulation
function simulate(psystem, steps) {
    // Implement custom simulation logic
    // Following P-Lingua semantics
}
```

### Creating a Language Server

For IDE integration, create a Language Server Protocol (LSP) implementation:

```typescript
// P-Lingua Language Server (TypeScript)
import {
    createConnection,
    TextDocuments,
    ProposedFeatures,
} from 'vscode-languageserver/node';

const connection = createConnection(ProposedFeatures.all);
const documents = new TextDocuments();

connection.onInitialize(() => {
    return {
        capabilities: {
            textDocumentSync: documents.syncKind,
            completionProvider: { resolveProvider: true },
            hoverProvider: true,
            definitionProvider: true,
        }
    };
});

connection.onCompletion((params) => {
    // Provide completions for P-Lingua keywords
    return [
        { label: 'def', kind: 3 },
        { label: '@mu', kind: 14 },
        { label: '@ms', kind: 14 },
        // ... more completions
    ];
});

documents.listen(connection);
connection.listen();
```

---

## Code Examples

### Example 1: Simple Cell Division

```plingua
@model(cell_division) = division_rules;

!division_rules {
    [a]'cell --> [a]'cell [a]'cell;
}

def main() {
    // Initial structure
    @mu = [ ]'environment [ ]'cell;
    
    // Initial objects
    @ms(cell) += a{10};
    
    // Division rule
    [a --> #]'cell;
    [a --> @d]'cell;
}
```

### Example 2: Graph Coloring

```plingua
@include "transition_model.pli"
@model<transition>

def graph(n) {
    // Create n nodes
    @mu = [ ]'1;
    @ms(1) += node{i} : 1 <= i <= n;
    @ms(1) += edge{i,j} : i < j <= n, 1 <= i <= n;
    
    // Coloring rules
    [node{i}, color{c} --> node{i}_c]'1 : 1 <= i <= n, 1 <= c <= 3;
    [node{i}_c1, node{j}_c1, edge{i,j} --> error]'1 : 
        i < j <= n, 1 <= i <= n, 1 <= c1 <= 3;
}

def main() {
    call graph(5);
}
```

### Example 3: Fibonacci Sequence

```plingua
def fibonacci(n) {
    @mu = [ ]'calc;
    @ms(calc) += fib{0}, fib{1};
    
    [fib{i-1}, fib{i} --> fib{i}, fib{i+1}]'calc : 
        2 <= i <= n;
    
    return n;
}

def main() {
    call fibonacci(10);
}
```

### Example 4: SAT Solver

```plingua
@include "sat_model.pli"

def sat(numVars, numClauses) {
    // Create membrane structure
    @mu = [ ]'env [ ]'sat;
    
    // Initialize variables
    @ms(sat) += x{i} : 1 <= i <= numVars;
    
    // Truth assignment rules
    [x{i} --> t{i}]'sat : 1 <= i <= numVars;
    [x{i} --> f{i}]'sat : 1 <= i <= numVars;
    
    // Checking rules
    [t{i}, clause{j,i,1} --> check{j}]'sat;
    [f{i}, clause{j,i,0} --> check{j}]'sat;
    
    // Success/failure detection
    [check{i} : 1 <= i <= numClauses --> sat]'sat;
    [# --> unsat]'sat : priority=1;
}

def main() {
    call sat(5, 10);
}
```

### Example 5: Ecosystem Simulation

```plingua
def ecosystem() {
    // Membrane structure
    @mu = [ ]'environment
          [ ]'territory{i} : 1 <= i <= 3;
    
    // Initial populations
    @ms(territory{1}) += rabbit{100}, fox{10};
    @ms(territory{2}) += rabbit{150}, fox{8};
    @ms(territory{3}) += rabbit{120}, fox{12};
    
    // Reproduction rules
    [rabbit{2} --> rabbit{3}]'territory{i} : 
        1 <= i <= 3;
    [fox{2}, rabbit{5} --> fox{3}]'territory{i} : 
        1 <= i <= 3;
    
    // Death rules
    [fox --> #]'territory{i} : 1 <= i <= 3, 
        probability=0.1;
    [rabbit --> #]'territory{i} : 1 <= i <= 3, 
        probability=0.05;
    
    // Migration rules
    rabbit [ ]'territory{i} --> [rabbit]'territory{j} : 
        1 <= i <= 3, 1 <= j <= 3, i != j,
        probability=0.02;
}

def main() {
    call ecosystem();
}
```

---

## Best Practices

### 1. Code Organization

```plingua
// models/base.pli - Common definitions
!common_evolution {
    [a --> b]'cell;
}

// models/extended.pli - Extended functionality
@include "models/base.pli"

!extended_evolution {
    @use common_evolution;
    [b --> c]'cell;
}

// main.pli - Entry point
@include "models/extended.pli"

def main() {
    // Implementation
}
```

### 2. Parameterization

Use modules for parameterized systems:

```plingua
def createSystem(size, density, threshold) {
    // Use parameters to configure system
    @mu = [ ]'root [ ]'node{i} : 1 <= i <= size;
    @ms(node{i}) += obj{j} : 
        1 <= i <= size,
        1 <= j <= (size * density);
}

def main() {
    call createSystem(100, 0.5, 10);
}
```

### 3. Debugging

Use verbosity levels:

```bash
# Level 0: Silent
plingua input.pli -v 0

# Level 1: Errors only
plingua input.pli -v 1

# Level 2: Warnings
plingua input.pli -v 2

# Level 3: Info
plingua input.pli -v 3

# Level 4-7: Debug
plingua input.pli -v 5
```

### 4. Performance Optimization

```plingua
// Instead of:
[a{i} --> b{i}]'cell : 1 <= i <= 1000;  // 1000 rules

// Use:
[a{i} --> b{i}]'cell : 1 <= i <= n;     // n rules (parametric)
```

### 5. Testing

Create test suites:

```plingua
// test/basic.pli
def testBasicEvolution() {
    @mu = [ ]'test;
    @ms(test) += a{10};
    [a --> b]'test;
    
    // After 1 step, should have b{10}
}

def testDivision() {
    @mu = [ ]'test [ ]'cell;
    @ms(cell) += x;
    [x]'cell --> [y]'cell [z]'cell;
    
    // After 1 step, should have 2 cells
}
```

---

## Troubleshooting

### Common Issues

#### Issue: "cannot find -lfl"

**Solution:**
```bash
sudo apt-get install libfl-dev
```

#### Issue: "boost/filesystem.hpp: No such file"

**Solution:**
```bash
sudo apt-get install libboost-filesystem-dev libboost-program-options-dev
```

#### Issue: Parsing errors

**Solution:**
- Check syntax carefully
- Ensure all semicolons are present
- Verify bracket matching
- Use `-v 3` for detailed error messages

#### Issue: Rules not executing

**Check:**
1. Rule applicability (objects exist?)
2. Priority levels
3. Semantics constraints
4. Charge compatibility

### Error Messages

| Error | Meaning | Solution |
|-------|---------|----------|
| "Syntax error" | Invalid syntax | Check language reference |
| "Undefined module" | Module not found | Define module or check spelling |
| "Circular include" | Include loop detected | Remove circular dependencies |
| "Type mismatch" | Incompatible types | Check variable types |
| "Rule not supported" | Unsupported rule type | Check simulator limitations |

### Getting Help

1. **Documentation**: Check `docs/` directory
2. **Examples**: See `examples/` directory
3. **Issues**: https://github.com/RGNC/plingua/issues
4. **Research Group**: http://www.gcn.us.es/

---

## Advanced Topics

### Custom Semantics

Define custom rule application semantics:

```plingua
@model(custom) = @parallel{evolution}, @sequence{communication};
```

### Probabilistic Systems

```plingua
[a --> b]'cell : probability=0.7;
[a --> c]'cell : probability=0.3;
```

### Priority-Based Systems

```plingua
[a --> b]'cell : priority=10;   // Higher priority
[b --> c]'cell : priority=5;    // Lower priority
```

### Regular Expressions in Patterns

```plingua
// Pattern matching with regular expressions
[obj{/^protein_[0-9]+$/} --> active]'cell;
```

---

## References

- **P-Lingua Website**: http://www.p-lingua.org/
- **Membrane Computing**: http://ppage.psystems.eu/
- **GitHub Repository**: https://github.com/RGNC/plingua
- **Research Papers**: See [bibliography](../bibliography.md)
- **Formal Specifications**: See [specifications](../specifications/)

---

## License

P-Lingua is licensed under the GNU General Public License v3.0 (GPL-3.0).

Copyright (C) 2018 Ignacio Perez-Hurtado
Research Group on Natural Computing
University of Seville, Spain

For full license text, see the LICENSE file or run:
```bash
plingua --license
```
