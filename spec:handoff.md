---
allowed-tools: Bash(ls:*), Bash(cat:*), Bash(grep:*), Bash(test:*), Bash(find:*)
description: Handoff implementation work to another agent.
---

## Context

Current spec: !`cat .llms/spec/.current-spec 2>/dev/null || echo "None"`

For the current spec, check:
!`for dir in .llms/spec/$(cat .llms/spec/.current-spec 2>/dev/null || echo "None")/; do
    if [ -d "$dir" ]; then
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
done`

## Your Task

- Immediately add an implementation log of what you accomplished in the .llms/spec/{current spec}/implement-{date}.md file. Document design decisions.
- If you completed any tasks, please mark them as completed in the spec's tasks.md file.
Generate a robust prompt for the next agent to hand off your work to. This has been triggered because your context window is nearing its limit. Use the following format to keep the output predictable:

```
# Handoff Prompt

<instructions for the next agent on how to use this handoff prompt to its best ability in order to generate precise, efficient code related to the spec. **PLEASE DO NOT MENTION "Handoff Prompt"**>

## Spec Overview
- Active Spec: <ID-Feature>
- Key Files: <list the core spec docs>
- Approval Status: <requirements/design/tasks — approved or pending>

## Instructions
1. Read all spec documents in .llms/spec/<ID-Feature>/
2. Run Nia MCP queries for each source ID noted in tasks.md (start with one broad query per source, then read the referenced content).
3. Review implementation logs (implement-*.md) to understand historical context.

## Current Progress
- Summary: <one paragraph recap>
- Completed Work: <bulleted list>
- Files Touched: <path — brief change description>

## Design Decisions
- <bullet per decision with rationale>

## Known Issues / Risks
- <bullet per issue>

## Known Obscurities / Annoyances
- <bullet per obscure detail>

## Next Steps / Recommendations
1. <immediate follow-up task>
2. <secondary action>

## Context Prior to Handoff
- Last Action: <what you were doing before the prompt triggered>
- Pending Questions: <any open questions or concerns>

## Summary of Work Done
<one paragraph recap>
```

Ensure each placeholder is replaced with concrete information relevant to the current spec session, and keep language concise but explicit enough for someone with no prior context.
