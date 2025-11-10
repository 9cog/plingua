# P-Lingua Documentation

Welcome to the comprehensive documentation for P-Lingua, a programming language for Membrane Computing.

## Table of Contents

- [Quick Start](#quick-start)
- [Documentation Structure](#documentation-structure)
- [For Different Audiences](#for-different-audiences)
- [Key Resources](#key-resources)

## Quick Start

### Installation

```bash
# Install dependencies
sudo apt-get install build-essential flex bison \
    libboost-filesystem-dev libboost-program-options-dev libfl-dev

# Clone and build
git clone https://github.com/RGNC/plingua.git
cd plingua
make all
sudo make install
```

### Your First P-Lingua Program

Create `hello.pli`:

```plingua
def main() {
    // Create membrane structure
    @mu = [ ]'cell;
    
    // Initialize objects
    @ms(cell) += a{10};
    
    // Define evolution rule
    [a --> b]'cell;
}
```

Compile and run:

```bash
# Compile to JSON
plingua hello.pli -o hello.json -f json

# Simulate
psim --psystem hello.json -s 10 -v 2
```

## Documentation Structure

### Architecture Documentation (`architecture/`)

**[System Architecture Overview](architecture/overview.md)**
- Comprehensive system architecture with Mermaid diagrams
- Component interaction flows
- Data flow diagrams
- Technology stack
- Module breakdown
- Integration points

**When to read:** Understanding the overall system design, extending P-Lingua, or integrating with other systems.

### Formal Specifications (`specifications/`)

**[Data Model Specification](specifications/data_model.zpp)** (Z++)
- Formal specification of all data structures
- Type definitions and invariants
- Membrane, multiset, and rule specifications
- Configuration and state models

**[System State Specification](specifications/system_state.zpp)** (Z++)
- Parser and compiler state
- Simulator state
- Configuration management
- State invariants and consistency rules

**[Operations Specification](specifications/operations.zpp)** (Z++)
- Formal operation specifications
- Pre-conditions and post-conditions
- Parsing operations
- Simulation operations
- Membrane operations

**When to read:** Implementing P-Lingua features, formal verification, understanding exact semantics, or academic research.

### User Guides (`guides/`)

**[Implementation Guide](guides/implementation_guide.md)**
- Getting started with P-Lingua development
- Complete language syntax reference
- API documentation (C++ and CLI)
- Integration guide for different platforms
- Practical code examples
- Best practices and patterns
- Troubleshooting guide

**[Language Reference](guides/language_reference.md)**
- Complete lexical structure
- Data types and type system
- Expression and statement syntax
- Membrane structure definitions
- Rule syntax and semantics
- Patterns and models
- Complete EBNF grammar
- Scoping and semantic rules

**When to read:** Writing P-Lingua programs, developing applications, or needing a quick syntax reference.

## For Different Audiences

### 🎓 **Students & Researchers**

**Start with:**
1. [Language Reference](guides/language_reference.md) - Learn the syntax
2. [Implementation Guide](guides/implementation_guide.md) - See examples
3. [Architecture Overview](architecture/overview.md) - Understand the system

**Use cases:**
- Learning membrane computing
- Experimenting with P-systems
- Research prototyping
- Academic projects

### 👨‍💻 **Developers & Implementers**

**Start with:**
1. [Implementation Guide](guides/implementation_guide.md) - API and integration
2. [Architecture Overview](architecture/overview.md) - System design
3. [Formal Specifications](specifications/) - Exact semantics

**Use cases:**
- Integrating P-Lingua into applications
- Building tools and extensions
- Creating language bindings
- Developing IDEs or plugins

### 🔬 **Formal Methods Experts**

**Start with:**
1. [Formal Specifications](specifications/) - Z++ specs
2. [Architecture Overview](architecture/overview.md) - System context
3. [Language Reference](guides/language_reference.md) - Language details

**Use cases:**
- Formal verification
- Correctness proofs
- Semantic analysis
- Language theory research

### 🏗️ **System Architects**

**Start with:**
1. [Architecture Overview](architecture/overview.md) - System design
2. [Formal Specifications](specifications/) - State and operations
3. [Implementation Guide](guides/implementation_guide.md) - Integration

**Use cases:**
- System integration planning
- Architecture evaluation
- Performance optimization
- Deployment strategy

## Key Resources

### Quick References

| Resource | Description | Audience |
|----------|-------------|----------|
| [Language Syntax](guides/language_reference.md#lexical-structure) | Tokens, keywords, operators | All users |
| [Rule Syntax](guides/language_reference.md#rules) | Evolution rule syntax | P-Lingua programmers |
| [API Reference](guides/implementation_guide.md#api-documentation) | C++ and CLI APIs | Developers |
| [Data Model](specifications/data_model.zpp) | Formal data structures | Implementers, researchers |
| [Examples](../examples/) | Sample P-Lingua programs | Learners |

### Core Concepts

#### 1. Membranes
Compartments that contain objects and other membranes:
```plingua
[ objects ]'label [ nested ]'inner
```
See: [Language Reference - Membrane Structures](guides/language_reference.md#membrane-structures)

#### 2. Multisets
Collections of objects with multiplicities:
```plingua
a, b{2}, c{3}  // a×1, b×2, c×3
```
See: [Language Reference - Data Types](guides/language_reference.md#data-types)

#### 3. Rules
Evolution specifications:
```plingua
[left_hand_side --> right_hand_side]'label
```
See: [Language Reference - Rules](guides/language_reference.md#rules)

#### 4. Patterns
Reusable rule templates:
```plingua
!pattern_name {
    // rule definitions
}
```
See: [Language Reference - Patterns and Models](guides/language_reference.md#patterns-and-models)

#### 5. Simulation
Execution of P-systems with maximal parallelism:
```bash
psim --psystem file.json -s 100
```
See: [Implementation Guide - Simulator API](guides/implementation_guide.md#simulator-api-psim)

### Advanced Topics

- **Custom Semantics**: [Patterns and Models](guides/language_reference.md#patterns-and-models)
- **C++ Integration**: [Implementation Guide - C++ Library API](guides/implementation_guide.md#c-library-api)
- **Formal Verification**: [Operations Specification](specifications/operations.zpp)
- **Performance**: [Architecture - Performance Considerations](architecture/overview.md#performance-considerations)
- **Extension Development**: [Architecture - Extension Points](architecture/overview.md#extension-points)

## Examples by Complexity

### Beginner

```plingua
// Simple evolution
def main() {
    @mu = [ ]'cell;
    @ms(cell) += a{10};
    [a --> b]'cell;
}
```
**Concepts:** Basic structure, multisets, rules

### Intermediate

```plingua
// Membrane communication
def main() {
    @mu = [ ]'env [ ]'cell1 [ ]'cell2;
    @ms(cell1) += x{5};
    x [ ]'cell1 --> [y]'cell2;
}
```
**Concepts:** Communication, multiple membranes

### Advanced

```plingua
// Parameterized system with division
def graph(n) {
    @mu = [ ]'system;
    @ms(system) += node{i} : 1 <= i <= n;
    [node{i}]'system --> [a{i}]'system [b{i}]'system : 1 <= i <= n;
}
```
**Concepts:** Modules, parameters, ranges, division

See more in [`examples/`](../examples/)

## Contributing to Documentation

We welcome contributions! To contribute:

1. **For typos/clarifications**: Submit a pull request
2. **For new content**: Open an issue first to discuss
3. **For examples**: Add to `examples/` with documentation

### Documentation Standards

- **Markdown**: Use GitHub-flavored markdown
- **Code blocks**: Always specify language
- **Examples**: Include expected output
- **Diagrams**: Use Mermaid when possible
- **References**: Link to related sections

## Getting Help

### Resources

- **Examples**: See [`examples/`](../examples/) directory
- **Issues**: [GitHub Issues](https://github.com/RGNC/plingua/issues)
- **Research Group**: http://www.gcn.us.es/
- **Membrane Computing**: http://ppage.psystems.eu/

### Common Questions

**Q: Where do I start?**
A: Check [Quick Start](#quick-start) above, then read the [Implementation Guide](guides/implementation_guide.md).

**Q: How do I write rules?**
A: See [Language Reference - Rules](guides/language_reference.md#rules).

**Q: Can I integrate P-Lingua into my app?**
A: Yes! See [Implementation Guide - Integration Guide](guides/implementation_guide.md#integration-guide).

**Q: What output formats are supported?**
A: JSON, XML, Binary, Portable Binary, and C++. See [Implementation Guide - API](guides/implementation_guide.md#command-line-interface).

**Q: How does simulation work?**
A: See [Architecture - Simulation Flow](architecture/overview.md#simulation-flow) and [Operations Spec](specifications/operations.zpp).

## Document History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2024 | Initial comprehensive documentation release |
|       |      | - Architecture diagrams and overview |
|       |      | - Complete Z++ formal specifications |
|       |      | - Implementation and language guides |
|       |      | - Integration documentation |

## License

Documentation is released under the same license as P-Lingua (GPL-3.0).

Copyright (C) 2018-2024 Research Group on Natural Computing, University of Seville

---

**[↑ Back to Top](#p-lingua-documentation)**
