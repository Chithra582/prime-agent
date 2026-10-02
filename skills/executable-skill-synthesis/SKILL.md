---
name: "executable-skill-synthesis"
description: "Discovers, drafts, tests, and packages recurring developer workflows into importable Python skill packages."
---

# Executable Skill Synthesis

## Overview
This skill transforms repeated multi-step workflows into reusable, importable Python packages that Prime Agent and other developers can immediately utilize in subsequent sessions.

## Key Capabilities
- **Pattern Identification**: Detects repetitive bash/REPL command chains and code transformation sequences.
- **Python Module Packaging**: Encapsulates routines into clean Python modules with typed function signatures and docstrings.
- **Automated Validation**: Synthesizes and executes unit tests inside the REPL to verify skill reliability before registration.

## Operational Workflow
1. **Workflow Extraction**: Capture sequence of tools, parameters, and assertions from session history.
2. **Code Generation**: Author Python skill module and export metadata in the project's skill registry.
3. **Sandbox Testing**: Run automated verification tests in an isolated REPL session.
4. **Registration**: Expose newly minted skill in the agent's available tool catalog.
