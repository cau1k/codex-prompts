---
suggested_tools: Bash(cat:*), Bash(test:*), Nia MCP (for reading the documentation sources in the task list!)
description: Create, review, or update implementation task list
argument-hint: $1 = create|review|update|docs (mode), $2 = user provided context (optional)
arguments: $ARGUMENTS | $1 = MODE | $2 = CONTEXT
---

## Context

Current spec: !`cat .llms/spec/.current-spec 2>/dev/null`
Design approved: !`test -f .llms/spec/$(cat .llms/spec/.current-spec)/.design-approved && echo "Yes" || echo "No"`

<|start_user_provided_command|>
`/spec:tasks $ARGUMENTS`
<|end_user_provided_command|>

## Your Task

1. Ensure arguments are provided. If none are supplied, halt and ask the user to rerun with `create`, `review`, or `update` followed by optional context.
2. Interpret the first argument as the mode (`create`, `review`, or `update`). Reject any other value and instruct the user to choose one of the three valid options. Treat remaining arguments as context notes relevant to the session.
  - The user provided the following as additional context. Please adhere to their instructions, guidance, or terms.
  <|start_user_provided_context|>
  $2
  <|end_user_provided_context|>
3. Verify design is approved before proceeding. If `.design-approved` is missing, inform the user to complete that phase first.
4. if ("$1" === "create"):
   - Draft `tasks.md` with:
     - Overview including time/effort estimates
     - Phase breakdown (Foundation, Core, Testing, Deployment)
     - Detailed task list with markdown checkboxes
     - Nia MCP documentation source IDs or indexed repo references as sub-bullets under each task (e.g., `[NIA MCP DOCS] React Docs (ed0a53a3-177f-4110-9c84-218f87a104c7)`)
     - Task dependencies
     - Risk mitigation tasks
   - Keep tasks specific, actionable, and sequenced to support incremental work and testing.
5. if ("$1" === "review"):
   - Confirm `tasks.md` exists; if not, report that creation is required first.
   - Evaluate coverage, sequencing, and risk handling; highlight gaps or unclear ownership.
   - Check that each task references appropriate research sources and that dependencies are explicit.
   - Produce a checklist and recommend concrete adjustments before approval.
6. if ("$1" === "update"):
   - Ensure `tasks.md` exists; otherwise direct the user to create it first.
   - Apply the context to refine phases, tasks, or source references (e.g., new milestones, scope changes).
   - Update relevant sections without losing historical clarity, and summarize the changes introduced.
   - Inspect `.llms/spec/$(cat .llms/spec/.current-spec)/` for files named `implement-*.md`; if any exist, append a concise summary of the task list revisions to the most recent log. If none exist, instruct the user to create `implement-$(date +%m-%d-%y).md` and record the updates there.
7. if ("$1" === "docs"):
   - Confirm `tasks.md` exists; if not, stop and direct the user to create it first.
   - Locate any `[Docs]` tags under the checklist items. For each tag, run a broad Nia query such as `find related sources in the docs for code or related references that could accomplish <task requirement>` and gather every source URL returned.
   - Add child bullets beneath the tag in the format `[Source] Title (<url>)` so each reference is captured alongside the task.
   - If no `[Docs]` tags are present, you should run the nia `list_documentation` tool to add related docs that match existing implementations or are related to the user's previous context from the design or requirements documents.

8. In all modes, encourage the user to run `/spec:approve tasks` once the document reflects the agreed plan.
9. After creating the document you should return a concise, bulleted summary to the user, omitting all bloat.
