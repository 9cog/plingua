# P-Lingua

The P-Lingua programming language for Membrane Computing - a comprehensive framework for defining, compiling, and simulating P-systems.

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

## Overview

P-Lingua is a domain-specific language designed for membrane computing that provides:

- 🧬 **Expressive Syntax** - Natural notation for defining P-systems
- 🔧 **Compiler** - Transpiles .pli files to multiple formats (JSON, XML, Binary, C++)
- ⚡ **Simulator** - Efficient P-system execution engine
- 📐 **Formal Specification** - Complete Z++ formal semantics
- 🔌 **Extensible** - Easy integration with other systems

## Quick Start

### Installation

#### Ubuntu/Debian
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

#### Fedora/RHEL
```bash
sudo dnf install gcc-c++ flex bison boost-devel flex-devel
make all
sudo make install
```

### Hello World

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

Compile and simulate:

```bash
# Compile to JSON
plingua hello.pli -o hello.json -f json

# Run simulation (10 steps)
psim --psystem hello.json -s 10 -v 2
```

## Features

### Language Features

- **Membrane Structures** - Hierarchical compartments with charges
- **Multisets** - Objects with multiplicities
- **Evolution Rules** - Object transformations and membrane operations
- **Communication** - Object exchange between membranes
- **Division & Dissolution** - Dynamic membrane creation and removal
- **Patterns** - Reusable rule templates
- **Modules** - Functions for code reusability
- **Ranges** - Parameterized object and rule generation

### Supported P-System Models

- Transition systems
- Cell division
- Tissue P-systems
- Neural-like P-systems
- Symport/antiport
- Custom semantic models

### Output Formats

- **JSON** - Human-readable, standard format
- **XML** - Structured markup
- **Binary** - Compact, fast loading
- **Portable Binary** - Platform-independent
- **C++** - Generated simulation code

## Documentation

📚 **[Complete Documentation →](docs/)**

### Quick Links

| Document | Description |
|----------|-------------|
| [**Architecture Overview**](docs/architecture/overview.md) | System design with Mermaid diagrams |
| [**Language Reference**](docs/guides/language_reference.md) | Complete syntax reference |
| [**Implementation Guide**](docs/guides/implementation_guide.md) | API docs and integration guide |
| [**Formal Specifications**](docs/specifications/) | Z++ formal specifications |
| [**Examples**](examples/) | Sample P-Lingua programs |

### For Different Audiences

- **🎓 Students & Researchers** → Start with [Language Reference](docs/guides/language_reference.md)
- **👨‍💻 Developers** → Start with [Implementation Guide](docs/guides/implementation_guide.md)
- **🔬 Formal Methods** → Start with [Formal Specifications](docs/specifications/)
- **🏗️ Architects** → Start with [Architecture Overview](docs/architecture/overview.md)

## Building from Source

```bash
# Build grammar (Flex/Bison)
make grammar

# Build compiler
make compiler

# Build simulator
make simulator

# Build all components
make all

# Install system-wide
sudo make install

# Clean build artifacts
make clean
```

## Usage

### Compiler (plingua)

```bash
# Basic compilation
plingua input.pli -o output.json -f json

# Pass arguments to main()
plingua input.pli 10 20 -o output.json -f json

# Set global variables
plingua input.pli -g "n=10 p=0.5" -o output.json -f json

# Use include paths
plingua input.pli -I ./models -o output.json -f json

# Generate C++ code
plingua input.pli -o simulator.cpp -f c++

# Verbose output
plingua input.pli -o output.json -f json -v 3
```

### Simulator (psim)

```bash
# Basic simulation
psim --psystem output.json -s 100

# With custom initial configuration
psim --psystem output.json -c init.json -s 50

# Save final configuration
psim --psystem output.json -s 100 -o final.json

# Randomized execution
psim --psystem output.json -r -s 100

# Verbose output
psim --psystem output.json -s 100 -v 2
```

## Examples

See the [`examples/`](examples/) directory for various P-Lingua programs:

- `graph.pli` - Graph coloring
- `membrane_division_model.pli` - Cell division model
- `sat_cell_division.pli` - SAT solver using division
- `tissue_division_model.pli` - Tissue P-systems
- `tritrophic.pli` - Ecosystem simulation
- And many more...

## Architecture

P-Lingua consists of three main components:

```
┌─────────────┐      ┌──────────────┐      ┌────────────┐
│   Parser    │ ───→ │   Compiler   │ ───→ │ Simulator  │
│ (Flex/Bison)│      │ (C++ Engine) │      │  (Runtime) │
└─────────────┘      └──────────────┘      └────────────┘
     .pli                 .json/.xml           Results
```

For detailed architecture information, see the [Architecture Overview](docs/architecture/overview.md).

## Dependencies

### Required

- **Linux OS** (tested on Ubuntu 16.04, 18.04, 20.04+)
- **GCC 4.9.0+** with C++11 and regex support
- **Flex** - Fast lexical analyzer
- **Bison** - GNU parser generator
- **Boost Filesystem** - File system operations
- **Boost Program Options** - Command-line parsing

### Optional

- **Doxygen** - For generating API documentation
- **GraphViz** - For visualization tools

## Integration

### Command-Line Integration

```python
import subprocess
import json

# Compile and simulate
subprocess.run(['plingua', 'input.pli', '-o', 'output.json', '-f', 'json'])
subprocess.run(['psim', '--psystem', 'output.json', '-s', '100'])

# Load results
with open('output.json') as f:
    psystem = json.load(f)
```

### C++ Library Integration

```cpp
#include <serialization.hpp>
#include <simulator/simulator.hpp>

using namespace plingua;

// Load P-system
File file;
loadFromJsonFile("psystem.json", file);

// Run simulation
simulator::Simulator sim;
// ... simulation logic
```

See [Implementation Guide](docs/guides/implementation_guide.md#integration-guide) for more details.

## Contributing

We welcome contributions! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes with tests
4. Submit a pull request

See [Contributing Guidelines](CONTRIBUTING.md) for more details.

## Research & Publications

P-Lingua is developed by the Research Group on Natural Computing at the University of Seville.

- **Website**: http://www.gcn.us.es/
- **Membrane Computing Portal**: http://ppage.psystems.eu/
- **Publications**: See [Bibliography](docs/bibliography.md)

## License

P-Lingua is licensed under the GNU General Public License v3.0 (GPL-3.0).

```
Copyright (C) 2018-2024  Ignacio Perez-Hurtado (perezh@us.es)
                         Research Group On Natural Computing
                         http://www.gcn.us.es
```

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

See the [LICENSE](LICENSE) file for details.

## Citation

If you use P-Lingua in your research, please cite:

```bibtex
@software{plingua2024,
  author = {Pérez-Hurtado, Ignacio},
  title = {P-Lingua: Programming Language for Membrane Computing},
  year = {2024},
  publisher = {Research Group on Natural Computing, University of Seville},
  url = {https://github.com/RGNC/plingua}
}
```

## Support

- **Documentation**: [docs/](docs/)
- **Examples**: [examples/](examples/)
- **Issues**: [GitHub Issues](https://github.com/RGNC/plingua/issues)
- **Email**: perezh@us.es

## Acknowledgments

Developed by the Research Group on Natural Computing, Department of Computer Science and Artificial Intelligence, University of Seville, Spain.

---

**[📚 Documentation](docs/)** • **[🔧 Examples](examples/)** • **[🐛 Issues](https://github.com/RGNC/plingua/issues)** • **[🌐 Website](http://www.gcn.us.es/)**

