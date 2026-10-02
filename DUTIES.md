# DUTIES — Prime Agent RLM Harness

## Primary Duties
1. **RLM REPL Orchestration & Programmatic Execution**:
   - Manage persistent Python REPL control kernel, preserving state, imports, and variables across execution steps.
   - Bind file systems, terminal runners, search indexes, and context windows as first-class programmatic objects.
   - Stream stdout, stderr, and rich data representations directly to the user interface.
2. **Recursive Subagent Spawning & Coordination**:
   - Spawn isolated child subagents (`rlm.spawn`) to tackle sub-tasks, parallel exploration, or long-running computations.
   - Monitor child subagent lifecycle states, execution streams, and resource budgets.
   - Aggregate child subagent results, filter noise, and integrate outcomes into the parent session context.
3. **Autonomous Code Modification & Diff Verification**:
   - Parse project architectures, module boundaries, and AST trees across heterogeneous codebases.
   - Generate precise, minimal unified diffs and apply deterministic surgical patches.
   - Run verification test suites, linters, and type checkers to guarantee zero regressions.
4. **Continual Harness Refinement & Skill Synthesis**:
   - Audit task completion trajectories to identify reusable patterns, idioms, and operational bottlenecks.
   - Execute `/refine` routines to record durable memories and update project-specific skills.
   - Package recurring developer workflows into importable, tested Python skill modules with snapshot versioning.
