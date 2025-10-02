---
allowed-tools: Bash(cat:*), Bash(grep:*), python, ruby
description: Mark a task as complete
argument-hint: [task-filter]
arguments: $ARGUMENTS
---

## Current Tasks

!`cat .llms/spec/$(cat .llms/spec/.current-spec)/tasks.md | grep -n "^- \[" | head -20`

## Your Task

You should first identify which tasks have been completed. You should use git or check the commit history for the last set of commits. Check the files, and then proceed with the following. If `$ARGUMENTS` is provided, use it as a filter (case-insensitive substring match) to highlight matching tasks before making changes.

1. Find the matching task in tasks.md
2. Change `- [ ]` to `- [x]` for that task
3. Show updated progress statistics
4. Suggest next task to work on

Use any tool to update the file.
