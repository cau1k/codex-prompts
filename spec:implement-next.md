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

1. Read and query the docs noted in the current spec's `tasks.md`. Ask more broad queries per source (if passed, for "$ARGUMENTS") with Nia MCP. Pertinent docs are identified by `[Docs]` in the spec's task.md file.
2. For each source, run at least one example-driven query (e.g., "show code implementing <requirement>") to surface concrete patterns or snippets. Log a one-sentence takeaway per query before moving on.
3. Provide a brief summary of how the collected docs and examples will guide your implementation.
4. Create a plan using your native plan tool with a minimum of 4 concise but detailed technical plan steps/bullets that lay out your implementation plan, not other steps like checking docs or other things. These should be purely technical to keep you focused on implementation.

### Phase 2: Implement Single Task in Current Phase

1. For that incomplete task in the current phase, you must use the primed context to ensure you have the correct context to continue working on that bullet point.
2. Before writing code, revisit the example snippets gathered in Phase 1 and note which one you are following. If the task requires a new pattern, pause to run an additional example-focused Nia query and log the result.
3. After analyzing the task, completing background research, and reviewing all related existing files (both project and spec related), begin writing code aligned with the referenced examples.

### Phase 3: Post-Implementation

1. Display current incomplete tasks
2. Create an implementation session log
3. Guide user to:
    - Work on tasks sequentially
    - Update task checkboxes as completed
    - Commit changes regularly
4. Ensure that you adhere to the fact-checking standard that includes querying the docs found in the task list for each task that you begin working on, sequentially. If any implementation detail diverges from the referenced examples, document the rationale and run a targeted Nia query to validate the alternative approach.
5. At the end of your post-implementation summary, in the chat interface, provide a short, conventional commit message for all of the work you just did.
6. Additionally, update a section in the implementation log to reflect the work you just did and cite the Nia queries/examples that informed the implementation.
