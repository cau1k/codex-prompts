---
description: Format a table of all links provided by the user, add any new URLs, index them, update the `indexing-jobs.md` table. Following renaming format.
suggested_tools: {"Nia MCP": {"reason": "for the documentation IDs provided in the tasks"}, "jina MCP": {"reason": "for checking the URLs and path patterns if a source index fails"}}
required_arguments: [REQUIRED USER INPUT] 
---

## Context

Current indexing jobs: !`cat .llms/nia/docs/indexing-jobs.md`

## Table Format

```markdown
| Resource URL        | Type               | Rename To | Renamed   | Status                            | Project Relation (for NIA) | Path Spec         | Data Source ID  | Notes   |
| ------------------- | ------------------ | --------- | --------- | --------------------------------- | -------------------------- | ----------------- | --------------- | ------- |
| {user provided url} | {docs/github repo} | {string}  | {boolean} | {pending/queued/completed/failed} | {string}                   | {nia url_pattern} | {nia source ID} | {notes} |
```

## Your Task

- (Phase 1) From `[USER INPUT]`:
    1. If there are any pending indexing jobs, please ask the user to confirm if they want these docs or repos to be indexed.
    2. Add new sources from `[USER INPUT]` to the table provided by the user. Mark all empty table cells with `--`. If the user did not provide a list of URLs, please follow up with them, then continue.
    3. Adhere to the `## Table Format` section provided above when adding new sources to the table.
    4. Use the `nia.index_documentation` tool to index the newly added pending Resource URLs in the table. Ensure url_patterns are correctly added for the newly added sources.

- (Phase 2) Post job cleanup:
    1. Verify (read) all indexing jobs are prepared in the `indexing-jobs.md` markdown table.
    2. If there are any pending indexing jobs, please ask the user to confirm if they want these docs or repos to be indexed.
    3. If an indexing job is in progress, check the status of the indexing job using the source ID with nia MCP.
    4. If it is completed, start the renaming process (with `nia.rename_resource`) and update the table.

- (Phase 3) Status report:
    1. Report the status of the indexing jobs to the user.
    2. If any failed, use the jina mcp to read the sitemap (if it's an independend source, not if it's a github repo, docs.rs, npm, etc (usually websites with docs.[domain] or [domain]/docs))
        - if it is not an independent source, use jina to briefly find paths that could refine the indexing job.
    3. Prompt the user with suggestions to fix the failed indexing jobs based on the sitemap.
