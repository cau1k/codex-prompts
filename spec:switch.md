---
allowed-tools: Bash(ls:*), Bash(echo:*), Bash(test:*)
description: Switch to a different specification
argument-hint: <spec-id-or-name>
arguments: $ARGUMENTS
---

## Available Specifications

!`ls -d .llms/spec/*/ 2>/dev/null | sort`

## Context

<|start_user_provided_command|>
`/spec:switch $ARGUMENTS`
<|end_user_provided_command|>

## Your Task

1. If no argument is provided, list all available specs and exit without making changes.
2. If an argument is provided, attempt to resolve it to a spec directory by:
   - Checking for an exact directory match (e.g., `001-feature-name`).
   - If not found, searching for directories whose ID prefix or feature name contains the provided text (case-insensitive).
3. If no matches are found, report the failure and list nearby spec names for reference.
4. If multiple matches are found, present the list and halt so the user can rerun with a more specific argument.
5. Once a single directory is resolved, update `.llms/spec/.current-spec` with that directory name.
6. Show the status of the newly active spec and recommend the next action based on outstanding approvals or tasks.
