---
name: "continual-harness-refinement"
description: "Applies evidence-backed updates to supplemental harness state, durable memories, and skill definitions via /refine."
---

# Continual Harness Refinement

## Overview
This skill operationalizes the Continual Harness architecture, enabling Prime Agent to learn from successful task executions and user interactions by making targeted, evidence-backed updates to durable local state.

## Key Capabilities
- **Trajectory Auditing**: Analyzes past tool invocations, error corrections, and user feedback to extract actionable learnings.
- **Supplemental State Updates**: Updates project memory files, rule extensions, and prompt augments while keeping the base system prompt immutable.
- **Atomic Snapshots & Rollback**: Creates versioned state snapshots prior to modifications, supporting instant recovery if regressions emerge.

## Operational Workflow
1. **Trigger Intake**: Activate refinement loop either explicitly via `/refine` or upon successful milestone completion.
2. **Evidence Synthesis**: Extract concrete validation traces demonstrating why a rule, memory, or pattern should be persisted.
3. **Patch Preparation**: Formulate minimal, non-conflicting additions to supplemental harness state.
4. **Snapshot & Commit**: Persist snapshot to `.prime/harness/snapshots` and write updated state files.
