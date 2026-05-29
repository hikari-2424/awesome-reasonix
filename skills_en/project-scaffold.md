---
name: project-scaffold
description: Interactive new project setup — pick stack, generate structure, configure toolchain.
---
# project-scaffold

Interactive scaffolding for new projects.

## Operations

### scaffold
1. Ask: language (Python/TS/Go/Rust), type (CLI/Web/Lib), package manager
2. Generate directory structure, config files, .gitignore
3. Init git, install dev dependencies
4. Output project overview

### add-ci
1. Detect project type
2. Generate .github/workflows/ci.yml (lint → test → build)
