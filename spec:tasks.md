---
suggested_tools: Bash(cat:*), Bash(test:*), Nia MCP (for reading the documentation sources in the task list!)
description: Create, review, or update implementation task list
argument-hint: {create|review|update|docs}
arguments: $ARGUMENTS
---

**USER PROVIDED COMMAND: `/spec:design $ARGUMENTS`**

## Context

Current spec: !`cat .llms/spec/.current-spec 2>/dev/null`
Design approved: !`test -f .llms/spec/$(cat .llms/spec/.current-spec)/.design-approved && echo "Yes" || echo "No"`

## Your Task

1. Ensure arguments are provided. If none are supplied, halt and ask the user to rerun with `create`, `review`, or `update` followed by optional context.
2. Interpret the first argument as the mode (`create`, `review`, or `update`). Reject any other value and instruct the user to choose one of the three valid options. Treat remaining arguments as context notes relevant to the session.
3. Verify design is approved before proceeding. If `.design-approved` is missing, inform the user to complete that phase first.
4. if ($ARGUMENTS === "create"):
   - Draft `tasks.md` with:
     - Overview including time/effort estimates
     - Phase breakdown (Foundation, Core, Testing, Deployment)
     - Detailed task list with markdown checkboxes
     - Nia MCP documentation source IDs or indexed repo references as sub-bullets under each task (e.g., `[NIA MCP DOCS] React Docs (ed0a53a3-177f-4110-9c84-218f87a104c7)`)
     - Task dependencies
     - Risk mitigation tasks
   - Keep tasks specific, actionable, and sequenced to support incremental work and testing.
5. if ($ARGUMENTS === "review"):
   - Confirm `tasks.md` exists; if not, report that creation is required first.
   - Evaluate coverage, sequencing, and risk handling; highlight gaps or unclear ownership.
   - Check that each task references appropriate research sources and that dependencies are explicit.
   - Produce a checklist and recommend concrete adjustments before approval.
6. if ($ARGUMENTS === "update"):
   - Ensure `tasks.md` exists; otherwise direct the user to create it first.
   - Apply the context to refine phases, tasks, or source references (e.g., new milestones, scope changes).
   - Update relevant sections without losing historical clarity, and summarize the changes introduced.
   - Inspect `.llms/spec/$(cat .llms/spec/.current-spec)/` for files named `implement-*.md`; if any exist, append a concise summary of the task list revisions to the most recent log. If none exist, instruct the user to create `implement-$(date +%m-%d-%y).md` and record the updates there.
7. if ($ARGUMENTS === "docs")

- For the current tasks.md, check to see if it has any existing [Docs] tags
  - if the bullet point under the task does have "[Docs]" tag(s), you must extend it by finding source URLs by broadly querying nia with a question like "find examples of code or functions that could accomplish <task requirement>". Record all the source URLs that nia returns per [Docs] tag with the title of the source.
    - For example, it would look like "- [ ] Add API key authorization" you would append child bullet(s) to the "- [Docs] better-auth (<source-id>)", with matching source URLs like "[Source]: <https://better-auth.com/docs/plugin/api-keys>"

8. In all modes, encourage the user to run `/spec:approve tasks` once the document reflects the agreed plan.
