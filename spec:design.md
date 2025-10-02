---
allowed-tools: Bash(cat:*), Bash(test:*), Bash(ls:*), Write, Nia MCP (all tools approved)
description: Create, review, or update technical design specification
argument-hint: create|review|update [context]
arguments: $ARGUMENTS
---

## Context

Current spec: !`cat .llms/spec/.current-spec 2>/dev/null`
Requirements approved: !`test -f .llms/spec/$(cat .llms/spec/.current-spec)/.requirements-approved && echo "Yes" || echo "No"`
Current directory: !`ls -la .llms/spec/$(cat .llms/spec/.current-spec)/ 2>/dev/null`

## Your Task

1. Ensure arguments are provided. If none are supplied, halt and ask the user to rerun with `create`, `review`, or `update` followed by optional context.
2. Interpret the first argument as the mode (`create`, `review`, or `update`). Reject any other value and instruct the user to choose one of the three valid options. Treat remaining arguments as context notes relevant to the session.
3. Verify requirements are approved (look for `.requirements-approved`). If not approved, notify the user to complete and approve requirements before proceeding.
4. For `create` mode:
   - Assemble background research with Nia MCP as needed, following the Tool Chaining Protocol when complexity warrants.
   - Produce `design.md` using `~/.codex/templates/design.md`, covering:
     - Architecture overview (diagrams encouraged)
     - Technology stack decisions
     - Data model and schema
     - API design
     - Security considerations
     - Performance considerations
     - Deployment architecture
     - Technical risks and mitigations
5. For `review` mode:
   - Confirm `design.md` exists; if absent, report that creation must happen first.
   - Read and critique each section, highlighting misalignments with requirements or missing research.
   - Produce a checklist against the template above and call out high-risk areas or open questions.
   - Recommend concrete updates required before approval.
6. For `update` mode:
   - Ensure `design.md` exists; otherwise instruct the user to create it first.
   - Apply the provided context to focus revisions (e.g., new constraints, tech changes).
   - Update relevant sections while keeping the document cohesive and noting any new risks or dependencies.
   - Summarize modifications and flag follow-up work if needed.
7. In all modes, cite documentation via Nia MCP when making or validating decisions, and encourage the user to run `/spec:approve design` once ready for sign-off.

Use the Write tool to create or revise the design document.
