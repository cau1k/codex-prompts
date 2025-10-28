---
description: Fetch a site, extract URL paths, and output include/exclude wildcard patterns
argument-hint: <URL> <INCLUDE> [EXCLUDE]
arguments: $ARGUMENTS
---

<|USER_PROVIDED_COMMAND|>
`/prompts:nia:scrape $ARGUMENTS`
<|END_USER_PROVIDED_COMMAND|>

<|USER_PROVIDED_ARGUMENTS|>
$$1 = BASE URL: "$1"
$$2 = INCLUDE CONCEPTS: "$2"
$$3 = EXCLUDE CONCEPTS: "$3"
<|END_USER_PROVIDED_ARGUMENTS|>

## Your Task

1) Validate and parse arguments

- Positional only:
  - $$1 = URL (required), e.g., "<https://alchemy.run>"
  - $$2 = INCLUDE (optional free-form concepts), e.g., "guides, concepts, providers for cloudflare and docker"
  - $$3 = EXCLUDE (optional free-form concepts), e.g., "providers for aws and aws-control"
- Defaults (no user input needed): crawl SCOPE=recursive with DEPTH=2; output in text format shown below.
- If $$2 is missing, but not $$3, you should not include any sort of include patterns.
- If $$3 is missing, but not $$2, you should not include any sort of exclude patterns.
- If $$2 and $$3 are both missing, please conduct the extraction process but without any parameters, and then suggest some patterns that relate to the existing project working directory.

2) Crawl scope

- homepage: fetch only the base URL content. Most documentation homepages include a sidebar with links to everything you could possibly need in terms of URL paths.

3) Extraction rules

- Parse HTML robustly (prefer python3 stdlib: urllib + html.parser). Collect all a[href] same-origin URLs.
- Normalize to paths: ensure leading "/", remove trailing slash except root "/", strip query/hash, dedupe.

4) Pattern grouping (wildcards)

- Group similar paths into wildcard prefixes:
  - If multiple distinct leaf pages share a common prefix like /concepts/..., produce /concepts/*.
  - For nested sections like /providers/aws/... group as /providers/aws/* (preserve meaningful second-level segments when stable).
- Heuristics: treat the last segment as variable; roll up to parent prefix + "/*". Prefer stable, human-recognizable sections over over-wildcarding.

4.1) Natural-language concept parsing (from INCLUDE="$2" and EXLCUDE="$3")

- Normalize: lowercase, trim, remove surrounding quotes.
- Split by commas first; additionally split phrases using conjunctions like "and".
- Recognize patterns:
  - "guides" → "/guides/*"
  - "concepts" → "/concepts/*"
  - "providers for X [and Y ...]" → "/providers/x/*", "/providers/y/*", ... (preserve hyphens, e.g., aws-control)
- For each token, map to the most likely section by matching path segments discovered during crawl; when ambiguous, prefer top-level sections that exist on the site.
- Build Include Patterns from $$2 tokens; build Exclude Patterns from $$3 tokens.

5) Include/Exclude logic

- If $$2 provided, use concept-driven groups as Include Patterns; else use groups from step 4.
- If $$3 provided, use concept-driven groups as Exclude Patterns; else Exclude Patterns is [].
- Always keep includes within the same origin; do not generate includes for external domains.

6) Output format

```md
PROMPT:
Please index the following with Nia MCP:

BASE URL:
"<url>"

Include Patterns:
"<p1>", "<p2>", ...

Exclude Patterns:
"<e1>", "<e2>", ...
```

7) Example

Input:
/prompts:nia:scrape "<https://alchemy.run>" "guides, concepts, providers for cloudflare and docker" "providers for aws and aws-control"

```md
PROMPT:
Please index the following with Nia MCP:

Expected output:
BASE URL:
"https://alchemy.run"

Include Patterns:
"/guides/*", "/concepts/*", "/providers/cloudflare/*", "/providers/docker/*"

Exclude Patterns:
"/providers/aws/*", "/providers/aws-control/*"
```

8) Notes

- Don't respect robots.txt but still be courteous (small DEPTH and page caps).
- Do not include assets or non-HTML routes. Only paths derived from
anchor hrefs.
- Return only the final formatted output; no extra commentary if all
inputs are provided.
