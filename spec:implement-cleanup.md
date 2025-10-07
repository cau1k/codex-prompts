---
description: Run cleanup, linting, and build validation for the active spec implementation
suggested_tools: Nia MCP (for troubleshooting docs and build guidance)
argument-hint: User-provided context
arguments: $ARGUMENTS
---

## Context

Current spec: !`cat .llms/spec/.current-spec 2>/dev/null`
Repo status: !`git status --short`

<|start_user_provided_task|>
"$ARGUMENTS"
<|end_user_provided_task|>

## Your Task

When this prompt is invoked, treat it as a recovery / stabilization protocol. Follow these steps sequentially:

1. **Clarify Context**
   - Summarize the current implementation goal and the error state you are trying to resolve.
   - Identify the files touched in this session (`git status`) and note which ones likely affect the failure.

2. **Environment Sanity Checks**
   - Verify dependencies are installed (`package manager install` equivalent) without modifying lockfiles unless explicitly required.
   - Inspect tooling versions if relevant (e.g., `node -v`, `rustc --version`).

3. **Linting & Static Analysis**
   - Run the project linters/formatters appropriate for the stack. Prefer project scripts (e.g., `pnpm lint`) over raw tool invocations.
   - Capture lint output summaries. If failures occur, list the top issues and point to the corresponding files/lines.

4. **Build / Compile Verification**
   - Execute the standard build or test command (`pnpm build`, `cargo check`, etc.)
   - If the project has multiple packages, target the ones affected by recent changes first.
   - Use `timeout` wrappers for long-running commands as needed.

5. **Iterative Fix Loop**
   - For each failure (lint or build), gather diagnostics:
     - Examine error logs and stack traces.
     - Query Nia MCP for troubleshooting guidance or example fixes, especially when errors point to unfamiliar APIs or configuration issues.
     - Apply focused code or config corrections, keeping changes minimal.
   - Re-run the failing command after each fix. Loop until clean or blockers remain.

6. **Validation Summary**
   - Document which commands were run and their outcomes (pass/fail with key messages).
   - List the files modified during cleanup and describe the changes.
   - Note any remaining issues that require user attention.

7. **Implementation Log Update**
   - Append a cleanup entry to `.llms/spec/<current-spec>/implement-<date>.md` detailing the recovery steps, commands executed, and resolutions achieved.
   - Highlight any TODOs or follow-up tasks discovered during cleanup.

8. **Git commit**
   - Please conventionally commit all the changes, including the work done prior. Do not commit any changes made during this "cleanup" phase after linting and formatting.
   - If it is a large commit, you should document any design decisions or quirks in the body of the the commit.

8. **Next Actions for User**
   - Recommend the next prompt or command the user should run (e.g., `/spec:implement-next` to resume feature work).
   - If unresolved errors remain, provide concise reproduction steps and suggested investigation paths.

Keep changes tightly scoped to stabilization. Do not introduce new features or refactors unless they directly resolve the current failure.
