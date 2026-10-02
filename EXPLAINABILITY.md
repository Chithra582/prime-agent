# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **Prime Agent RLM Harness** (`prime-agent`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** Prime Agent RLM Harness (`prime-agent`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / Autonomous Software Engineering  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

The Prime Agent RLM Harness is an autonomous software engineering and research assistant built on the Recursive Language Model (RLM) paradigm and the Continual Harness architecture. Unlike conventional chat-loop coding assistants that rely on repeated context re-prompting and brittle tool wrappers, Prime Agent embeds a persistent Python REPL control environment as its primary cognitive substrate. The agent treats prompts as programmatic variables, tools and recursive subagents as native function calls, and session refinements as evidence-backed durable state updates.

### 1. Decision Architecture

The user instruction intake, programmatic REPL execution, subagent delegation, verification feedback, and harness refinement pipeline operates across a deterministic, five-stage architecture:

```
User Task / Engineering Objective / Research Goal
    │
    ▼
[Stage 1: Intent Analysis & Task Decomposition]
    │  - Parses natural language objectives and identifies repository constraints
    │  - Evaluates computational complexity and sub-task parallelism potential
    │  - Selects execution mode: direct REPL evaluation vs. recursive subagent delegation
    ▼
[Stage 2: REPL Environment & Variable Binding]
    │  - Instantiates or attaches to persistent Python REPL kernel
    │  - Binds filesystem tools, workspace AST trees, and prompt variables into namespace
    │  - Sanitizes shell command strings and enforces path containment rules
    ▼
[Stage 3: Programmatic Execution & Subagent Orchestration]
    │  - Executes code cells, runs tests, and applies surgical diff patches
    │  - Spawns recursive child subagents (`rlm.spawn`) for parallel exploration
    │  - Streams stdout/stderr outputs and monitors execution timeouts
    ▼
[Stage 4: Empirical Verification & Behavioral Validation]
    │  - Executes automated test suites, linters, and runtime health probes
    │  - Validates exit codes ($R_{\text{exit}} = 0$) and diff correctness
    │  - Evaluates regression risks and rolls back unstable modifications
    ▼
[Stage 5: Continual Harness Refinement & Trajectory Commit]
    │  - Audits execution trajectory against user intent and performance benchmarks
    │  - Synthesizes evidence-backed memory updates and skill packages via `/refine`
    │  - Persists versioned state snapshots to `.prime/harness/snapshots`
    ▼
Verified Software Deliverable & Durable Harness State Snapshot
```

### 2. Decision Logic & Routing Formulations

Prime Agent evaluates subagent delegation, execution safety, and harness refinement eligibility using deterministic mathematical formulations:

1. **Subagent Delegation Index ($S_{\text{delegate}}$)**:
   $$S_{\text{delegate}}(T) = (w_c \cdot C_{\text{complexity}}) + (w_p \cdot P_{\text{parallel}}) + (w_d \cdot D_{\text{depth}})$$
   where:
   - $C_{\text{complexity}} \in [0, 1]$ measures estimated cognitive and dependency load of task $T$.
   - $P_{\text{parallel}} \in \{0, 1\}$ indicates whether sub-tasks can execute asynchronously without lock contention.
   - $D_{\text{depth}} \in [0, 1]$ represents workspace file hierarchy depth.
   - Weights: $w_c = 0.45, w_p = 0.35, w_d = 0.20$ ($\sum w_i = 1.0$).
   - When $S_{\text{delegate}}(T) \ge 0.70$, the orchestrator delegates sub-tasks via `rlm.spawn`.

2. **Empirical Verification Score ($V_{\text{task}}$)**:
   $$V_{\text{task}} = (w_t \cdot T_{\text{pass}}) + (w_l \cdot L_{\text{clean}}) + (w_b \cdot B_{\text{build}})$$
   where:
   - $T_{\text{pass}} \in [0, 1]$ is the ratio of passing unit/integration tests.
   - $L_{\text{clean}} \in \{0, 1\}$ indicates zero lint, formatting, or type errors.
   - $B_{\text{build}} \in \{0, 1\}$ denotes successful clean build exit code.
   - Weights: $w_t = 0.50, w_l = 0.25, w_b = 0.25$.
   - A task is declared successful only when $V_{\text{task}} = 1.0$.

### 3. Thresholding & Refusal Decision Criteria

Prime Agent enforces strict operational boundaries to prevent damage to host systems and code integrity:
- **Refusal to Alter Immutable Base Prompts**: Modifications targeting base system prompts or constitutional guardrails are strictly rejected (`ERR_IMMUTABLE_PROMPT_VIOLATION`).
- **Refusal of Destructive Remote Git Operations**: Destructive commands such as `git push --force` or branch deletion on protected branches are blocked (`ERR_DESTRUCTIVE_GIT_FORBIDDEN`).
- **Execution Timeout Ceilings**: Bash commands exceeding 120 seconds or REPL cells exceeding 60 seconds are terminated via SIGTERM/SIGKILL (`WARN_EXECUTION_TIMEOUT_EXCEEDED`).
- **Refusal of Unverified Diff Commits**: Code patches that fail compilation, syntax checks, or unit tests cannot be marked complete (`ERR_VERIFICATION_GATE_FAILED`).

### 4. Fallback Decision Mechanism

Continuous operational reliability is guaranteed through multi-tier fault recovery cascades:
- **Model Provider Cascade**: When foundation model providers encounter rate limits (HTTP 429) or service outages (HTTP 503), the engine cascades across Anthropic, OpenAI, and local/open-weight endpoints via Prime AI router.
- **REPL Kernel Recovery**: If a Python REPL process crashes due to out-of-memory or C-extension segfaults, the harness restarts the kernel, restores last known state snapshots, and re-executes.
- **Atomic Harness Rollback**: If a newly refined skill or supplemental prompt causes performance degradation, `/refine` triggers an automatic rollback to the preceding snapshot in `.prime/harness/snapshots`.

### 5. Human-in-the-Loop Governance

Human operators retain full authority and operational oversight over Prime Agent:
- **Interactive TUI & CLI Controls**: Users can pause, inspect variable namespaces, review subagent hierarchies, and terminate tasks at any moment.
- **Differential Review Prompting**: All non-trivial file modifications produce reviewable unified diffs prior to repository commits.
- **Transparent Execution Logs**: Every REPL command, shell execution, subagent transcript, and verification trace is written to auditable session logs.

---

## The Data It Uses

Prime Agent maintains strict adherence to data minimization, privacy boundaries, and workspace containment standards.

### 1. Ingested Input Data

The agent processes only operational assets necessary to fulfill software development and research workflows:
- **User Directives**: Natural language goals, issue descriptions, feature requests, and CLI arguments.
- **Repository Source Code**: Project files, directory trees, dependency manifests (`package.json`, `pyproject.toml`), and commit histories.
- **Runtime Execution Streams**: Terminal outputs, compiler messages, test runner reports, and REPL evaluation results.

### 2. Configuration & Reference Data

- **Harness State Files**: Supplemental prompt fragments, project idioms, and durable memories stored under `.prime/harness/`.
- **Skill Definitions**: Importable Python modules and YAML specifications located in the project's skills catalog.
- **Security & Linters Rules**: Repository linter policies (`biome.json`, `eslint.json`), test policies, and security guidelines.

### 3. Base Model & Inference Lineage

- **Deterministic Core Engines**: TypeScript/Node.js runtime orchestrator, Python REPL kernel, and Git VCS adapters operate deterministically without model variance.
- **Frontier LLM Lineage**: Utilizes high-capability reasoning models (e.g., Claude 3.5 Sonnet, GPT-4o, and specialized coding models) for code generation, architectural analysis, and subagent reasoning.
- **Zero Training on Proprietary Code**: User code, private keys, and proprietary repository logic are never transmitted for foundation model pretraining or continual weight updates.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against prompt injection through untrusted codebase inputs, indirect injection via malicious test fixtures, and supply-chain tampering.
- **Local Workspace Storage**: All state, session logs, and skill definitions reside locally on the developer's workstation or specified private cloud container.
- **Automated Secret Scrubbing**: Environment variables, API tokens, and private SSH keys are scrubbed from exported execution traces.
- **Zero Commercial Monetization**: Developer source code, execution transcripts, and harness memory files are never sold, aggregated, or shared with third parties.

---

## Limitations

Understanding the operational constraints and boundary conditions of Prime Agent ensures safe, productive deployment.

### 1. Interactive GUI & Desktop Automation
- **Limitation**: Prime Agent operates within headless terminal, REPL, and file environments; it cannot directly drive native GUI desktop applications.
- **Mitigation**: Headless browser automation (Playwright/Puppeteer) or mock headless drivers are deployed for frontend and web-based testing.

### 2. Closed-Source Binary Reverse Engineering
- **Limitation**: The agent relies on source text and AST representations; it cannot perform deep reverse engineering of stripped closed-source machine binaries.
- **Mitigation**: Users must provide corresponding source code, symbols, or decompiled representations.

### 3. Non-Deterministic Multi-Threaded Race Conditions
- **Limitation**: Intermittent, low-frequency concurrency bugs that cannot be reliably reproduced in test harnesses can evade automated validation gates.
- **Mitigation**: Prime Agent runs repeated randomized seed test sweeps and enforces deterministic concurrency analysis tools where available.

### 4. Out-of-Band Hardware and Network Dependencies
- **Limitation**: Tasks requiring specialized external hardware rigs, FPGA flashing, or private internal network VPNs cannot be autonomously validated.
- **Mitigation**: The agent mocks external physical interfaces and flags verification gates requiring physical human sign-off.

### 5. Multi-Million-Line Monorepo Context Windows
- **Limitation**: Ingestion of entire multi-gigabyte codebases exceeds single-pass model context windows.
- **Mitigation**: Prime Agent employs recursive AST indexing, targeted symbol search, and recursive subagent delegation to partition large workspaces.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & routing formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested user directives, source code & runtime streams | Section 1 | Verified |
| - Configuration, harness state & skill definitions | Section 2 | Verified |
| - Base model lineage & deterministic REPL engines | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Interactive GUI & desktop automation | Section 1 | Verified |
| - Closed-source binary reverse engineering | Section 2 | Verified |
| - Non-deterministic multi-threaded race conditions | Section 3 | Verified |
| - Out-of-band hardware and network dependencies | Section 4 | Verified |
| - Multi-million-line monorepo context windows | Section 5 | Verified |
