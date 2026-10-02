---
name: "recursive-subagent-spawning"
description: "Spawns, monitors, and aggregates results from recursive child subagents executing parallel or background workloads."
---

# Recursive Subagent Spawning

## Overview
This skill implements the recursive agent capability of the RLM framework (`rlm.spawn`), enabling Prime Agent to delegate sub-tasks, exploration, or computational sweeps to autonomous child instances.

## Key Capabilities
- **Programmatic Delegation**: Spawns isolated child agents through simple Python API calls with dedicated prompts and tool access.
- **Concurrent Execution**: Runs multiple child agents simultaneously for parallel search, code inspection, or test runs.
- **Result Aggregation**: Receives structured completion payloads, logs, and return values from child processes.

## Operational Workflow
1. **Task Decomposition**: Break complex problem into self-contained, parallelizable sub-tasks.
2. **Subagent Spawning**: Call `rlm.spawn(prompt=..., tools=...)` to launch child worker instances.
3. **Execution Monitoring**: Stream execution logs, monitor resource budgets, and detect timeouts.
4. **Integration**: Collect subagent outputs, reconcile differences, and integrate verified results.
