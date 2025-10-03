---
allowed-tools: Bash(touch:*), Bash(test:*), Bash(cat:*)
description: Approve a specification phase
argument-hint: requirements|design|tasks
arguments: $ARGUMENTS
---

**USER PASSED COMMAND: `/spec:approve $ARGUMENTS`**

## Context

Current spec: !`cat .llms/spec/.current-spec 2>/dev/null`
Spec directory: !`ls -la .llms/spec/$(cat .llms/spec/.current-spec)/ 2>/dev/null`

## Your Task

1. Determine the target phase:
   - If `$ARGUMENTS` is empty, select the first phase without an approval marker in this order: requirements → design → tasks. If all are approved, report that no action is needed.
   - If `$ARGUMENTS` is provided, use it verbatim after validating it matches one of `requirements`, `design`, or `tasks`.
2. Verify the corresponding phase file exists (requirements.md, design.md, or tasks.md). If missing, halt and report the issue.
3. Create the approval marker: `.$ARGUMENTS-approved` using the touch command.
4. Inform the user about next steps and recommend invoking the next spec prompt immediately:
   - After requirements → run `/spec:design create` to continue the workflow
   - After design → run `/spec:tasks create` to define the implementation plan
   - After tasks → begin implementation work (e.g., `/spec:implement-next`)
5. If an invalid phase name is provided, list valid options and do not create a marker.

Use the touch command to create approval markers.
