---
description: Start implementation of the next single task bullet point in the active spec phase.
suggested_tools: Nia MCP (for the documentation IDs provided in the tasks!)
arguments: specific task (optional)
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

### Phase 0: Spec Verification

1. Verify all phases are approved
2. Read all markdown documents in the phase.
2.5. If a user provided an argument for a task, please implement that argument and nothing else:

- "$ARGUMENTS"

3. If a phase is in progress, focus on the first incomplete bullet point in that phase.

- For the phase in progress, read all the files in the .llms/spec/[current-phase-number]-[feature-description] in order to get an understanding of what has been done, what you need to do, and what needs to come next. Then, review all the related files from the last task (found in the implementation log typically)

4. Prepare to prime your context for the next phase: Pertinent docs are identified by `[Docs]` in the spec's task.md file

### Phase 1: Prime Context

1. Confirm the active task's checklist includes `[Docs]` tags. If none are present, run the Nia `list_documentation` tool to surface sources tied to the design/requirements context or adjacent implementations and add the relevant tags before proceeding.
2. Start by issuing this required Nia query: `find the most pages in this documentation to complete this task: $ARGUMENTS`. If no arguments were provided, replace `$ARGUMENTS` with a concise description of the active task. Use the response to shortlist the documentation IDs worth inspecting.
3. For each `[Docs]` tag, issue a broad Nia query such as `find related sources in the docs for code or related references that could accomplish <task requirement>` and capture every URL returned as `[Source] Title (<url>)` child bullets beneath the tag so the references stay traceable.
4. Read and query only the shortlisted docs that appear in the current spec's `tasks.md`, staying focused on the sections relevant to the active task. Ask one broad, doc-level query per source to confirm relevance before drilling down. Pertinent docs are identified by `[Docs]` in the spec's task.md file.
5. For each source that proves relevant, run at least one example-driven query (e.g., "show code implementing <requirement>") to surface concrete patterns or snippets. Capture a one-sentence takeaway per query and prefer summarizing over copying long excerpts; only quote the exact lines you expect to reuse immediately.
6. Maintain a running hit list of the docs touched: note source ID, query, essential snippet references, and open questions. If a doc turns out to be irrelevant, record that so you do not revisit it in the same session.
7. Present a concise `Research Summary` to the user that bundles the takeaways, cites which docs were touched, calls out any remaining gaps, and explicitly states that you have paused to avoid blowing out context.
8. Stop here and wait for user direction. Do not start planning or implementation until the user either greenlights the next phase or requests more research.

### Phase 2: Planning (Begin Only After User Approval)

1. Once the user authorizes you to continue, create a plan using your native plan tool with a minimum of 4 concise but detailed technical steps that focus solely on implementation work.
2. If the user asks for more research, return to Phase 1 before attempting to plan again.

### Phase 3: Implement Single Task in Current Phase

1. For that incomplete task in the current phase, you must use the primed context to ensure you have the correct context to continue working on that bullet point.
2. Before writing code, revisit the example snippets gathered in Phase 1 and note which one you are following. If the task requires a new pattern, pause to run an additional example-focused Nia query and log the result.
3. After analyzing the task, completing background research, and reviewing all related existing files (both project and spec related), begin writing code aligned with the referenced examples.

### Phase 4: Post-Implementation

1. Display current incomplete tasks
2. Create an implementation session log
3. Guide user to:
    - Work on tasks sequentially
    - Update task checkboxes as completed
    - Commit changes regularly
4. Ensure that you adhere to the fact-checking standard that includes querying the docs found in the task list for each task that you begin working on, sequentially. If any implementation detail diverges from the referenced examples, document the rationale and run a targeted Nia query to validate the alternative approach.
5. At the end of your post-implementation summary, in the chat interface, provide a short, conventional commit message for all of the work you just did.
6. Additionally, update a section in the implementation log to reflect the work you just did and cite the Nia queries/examples that informed the implementation.
