---
description: Start implementation from approved tasks
suggested_tools: Nia MCP (for the documentation IDs provided in the tasks!)
argument-hint: [phase-number OR specific task]
arguments: $ARGUMENTS
---

## Context

Current spec: !`cat .llms/spec/.current-spec 2>/dev/null`
Tasks approved: !`test -f .llms/spec/$(cat .llms/spec/.current-spec)/.tasks-approved && echo "Yes" || echo "No"`
List and then read all documents in the spec `ls -al .llms/spec/$(cat .llms/spec/.current-spec)`

```bash
for file in .llms/spec/$(cat .llms/spec/.current-spec)/*; do
    echo "--- File: $file ---"
    echo "--- Content: $(cat $file) ---"
done
```

## Current Tasks

!`if [ -f ".llms/spec/$(cat .llms/spec/.current-spec)/tasks.md" ]; then
    echo "=== Phase Overview ==="
    grep "^## Phase" ".llms/spec/$(cat .llms/spec/.current-spec)/tasks.md"
    echo ""
    echo "=== Incomplete Tasks ==="
    grep "^- \[ \]" ".llms/spec/$(cat .llms/spec/.current-spec)/tasks.md" | head -20
fi`

## Your Task

1. Verify all phases are approved. If any approval marker is missing, pause and tell the user which phase still needs attention.
2. Resolve the working phase:
   - When `$ARGUMENTS` contains a number, map it to the corresponding `## Phase <number>` heading in `tasks.md`.
   - If no argument is provided, select the first phase that still has unchecked tasks.
   - If the requested phase cannot be found or has no remaining work, explain the issue and stop.
3. Within the chosen phase, identify the first unchecked task (`- [ ]`) and treat it as the active work item.
4. Read every specification document plus any files referenced by the active task. Follow the fact-checking standard by querying the cited sources with Nia MCP before coding.
5. Display the list of incomplete tasks, highlighting the active item and summarizing its dependencies or related docs.
6. Create or append to the current implementation session log with:
   - Phase number and task summary
   - Research sources consulted
   - Planned implementation approach
7. Guide the user to work tasks sequentially, update checkboxes, and commit changes regularly.
8. After completing the task, recap the work, note follow-up next steps, and remind the user to mark the task complete in `tasks.md`.
9. Provide a conventional commit message in chat covering the session’s work, and include a `Co-authored-by: @codex` footer in the log entry when applicable.

Only begin implementation once the targeted task is fully understood and the context is primed.
