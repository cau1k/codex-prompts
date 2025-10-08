---
allowed-tools: Bash(cat:*), Bash(test:*), Bash(touch:*), Write
description: Create, review, or update requirements specification
argument-hint: create|review|update
arguments: $ARGUMENTS
---

**USER PROVIDED COMMAND: `/spec:requirements $ARGUMENTS`**

## Context

Current spec: !`cat .llms/spec/.current-spec 2>/dev/null || echo "No active spec"`
Spec directory contents: !`ls -la .llms/spec/$(cat .llms/spec/.current-spec 2>/dev/null)/ 2>/dev/null || echo "Spec not found"`

<|start_user_provided_task|>
`/spec:requirements $ARGUMENTS`
<|end_user_provided_task|>

## Your Task

1. Ensure arguments are provided. If none are supplied, halt and ask the user to rerun with `create`, `review`, or `update` followed by optional context.
2. Interpret the first argument as the mode (`create`, `review`, or `update`). Reject any other value and instruct the user to choose one of the three valid options. Treat remaining arguments as context notes relevant to the session.
3. if ($ARGUMENTS === "create"):
   - Confirm `requirements.md` exists or create it.
   - Begin background research using the available MCP tools and, when appropriate, follow the Tool Chaining Protocol in `AGENTS.md`.
   - Populate `requirements.md` using `~/.codex/templates/requirements.md`, covering:
     - Feature overview
     - User stories with acceptance criteria
     - Functional requirements (P0, P1, P2)
     - Non-functional requirements
     - Constraints and assumptions
     - Out of scope items
     - Success metrics
4. if ($ARGUMENTS === "review"):
   - Verify `requirements.md` exists; if it does not, report that creation is required first.
   - Read and summarize each section, surfacing gaps, ambiguities, or missing background research.
   - Produce a checklist referencing the template above and highlight any unmet criteria or risks.
   - Provide clear, actionable recommendations to close the gaps.
5. if ($ARGUMENTS === "update"):
   - Confirm `requirements.md` exists; if not, inform the user to create it first.
   - Apply the provided context to focus on the areas that need changes (e.g., new scope, revised constraints).
   - Edit the document to incorporate the updates while keeping template sections intact and consistent.
   - Summarize the changes made and note any follow-up actions required before approval.
6. In all modes, remind the user to run `/spec:approve requirements` once the document is ready for sign-off.
7. After creating the document you should return a concise, bulleted summary to the user, omitting all bloat.

Use the Write tool to create or revise `requirements.md` as needed.
