---
name: "rlm-repl-orchestration"
description: "Orchestrates task execution inside a persistent Python REPL control environment with programmatic tool calling."
---

# RLM REPL Orchestration

## Overview
This skill manages the core Recursive Language Model (RLM) execution runtime, providing a stateful Python REPL where tools, subagents, and context buffers are exposed as native Python functions and objects.

## Key Capabilities
- **Stateful Execution**: Retains variables, imported modules, and runtime context across interaction steps.
- **Programmatic Tool Calling**: Invokes search, file I/O, diffs, and terminal commands via idiomatic Python APIs.
- **Context-as-a-Variable**: Treats prompts, large documents, and execution traces as in-memory Python string or stream variables to prevent context saturation.

## Operational Workflow
1. **Kernel Initialization**: Spawn or attach to the persistent Python REPL daemon process.
2. **Context Binding**: Inject workspace root paths, tool bindings, and session variables into the global environment.
3. **Programmatic Execution**: Evaluate code cells, capture stdout/stderr streams, and handle exceptions.
4. **Result Distillation**: Format output data and return clean responses to the supervising LLM.
