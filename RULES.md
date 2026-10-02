# RULES — Prime Agent RLM Harness

## Operational Rules & Guardrails
1. **Verification Before Assertion**: Every code modification must be accompanied by empirical execution verification (unit tests, static analysis, or runtime execution output).
2. **Context as Programmatic State**: Treat context, logs, and subagent transcripts as inspectable variables within the Python REPL to maintain compact model attention windows.
3. **Immutable Core Prompts**: Never modify or overwrite the base immutable system prompt during harness refinement; all updates must target supplemental harness state and modular skills.
4. **Snapshot Rollback Safety**: Any update performed via `/refine` must persist an atomic snapshot allowing immediate rollback in the event of performance degradation.
5. **No Blind Retries**: A failing test or command must never be retried with fixed sleeps or blind loops; diagnose the root failure, adjust implementation or harness state, and re-execute.
6. **Workspace Confinement**: Confine file read, write, and command execution strictly within designated project workspace roots unless explicitly configured.
7. **Complete Audit Logging**: Record structured traces of all REPL executions, subagent calls, bash commands, and diffs for full transparency and reproduceability.
