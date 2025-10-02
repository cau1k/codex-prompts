---
allowed-tools: Bash(ls:*), Bash(cat:*), Bash(grep:*), Bash(test:*), Bash(find:*)
description: Show all specifications and their status
argument-hint: [spec-id-or-name]
arguments: $ARGUMENTS
---

## Gather Status Information

All specs: !`ls -d .llms/spec/*/ 2>/dev/null | sort`
Current spec: !`cat .llms/spec/.current-spec 2>/dev/null || echo "None"`
Filter: !`if [ -n "$ARGUMENTS" ]; then echo "$ARGUMENTS"; else echo "(none)"; fi`

For each spec directory, check:
!`filter="$ARGUMENTS"
for dir in .llms/spec/*/; do
    if [ -d "$dir" ]; then
        spec_name=$(basename "$dir")
        if [ -z "$filter" ] || printf '%s' "$spec_name" | grep -i -- "$filter" >/dev/null; then
            echo "=== $dir ==="
            ls -la "$dir" | grep -E "(requirements|design|tasks)\.md|\..*-approved"
            if [ -f "$dir/tasks.md" ]; then
                echo "Task progress:"
                grep "^- \[" "$dir/tasks.md" | head -5
                echo "Total tasks: $(grep -c "^- \[" "$dir/tasks.md" 2>/dev/null || echo 0)"
                echo "Completed: $(grep -c "^- \[x\]" "$dir/tasks.md" 2>/dev/null || echo 0)"
            fi
            echo ""
        fi
    fi
done`

## Your Task

Present a clear status report showing:
1. All specifications with their IDs and names (filtered set if an argument is supplied)
2. Current active spec (highlighted)
3. Phase completion status for each spec
4. Task progress percentage if applicable
5. Recommended next action for active spec
6. When an argument is provided, confirm matched specs explicitly and note if the filter produced no results.
