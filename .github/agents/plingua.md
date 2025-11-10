---
# Fill in the fields below to create a basic custom agent for your repository.
# The Copilot CLI can be used for local testing: https://gh.io/customagents/cli
# To make this agent available, merge this file into the default repository branch.
# For format details, see: https://gh.io/customagents/config

name: plingua
description: pli language
---

# P-Lingua

The P-Lingua language for Membrane Computing

## Dependencies
Linux OS (tested on Ubuntu 16.04 and Ubuntu 18.04)
GCC 4.9.0 or higher (with support for regex)
Flex
Bison
libboost-filesystem-dev
libboost-program-options-dev
sudo apt-get install build-essential flex bison libboost-filesystem-dev libboost-program-options-dev

## Compiling
make grammar
make compiler
make simulator
sudo make install

