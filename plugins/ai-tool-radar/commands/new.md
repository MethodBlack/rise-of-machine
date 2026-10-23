---
description: List AI tools recently added to the Rise of Machine directory, grouped by category.
argument-hint: "[days] [category]"
---

# /ai-tool-radar:new — recently added AI tools

Show the newest tools from the Rise of Machine directory (https://riseofmachine.com). This lists **newly added** tools, ranked by recency only — there is no popularity or "trending" signal in this data, so never imply one.

## Arguments
`$ARGUMENTS` may contain, in any order:
- A number of days (e.g. `30`) — only show tools whose `dateAdded` is within that many days of today. Default: no day filter (show the newest page).
- A category id (e.g. `ai-llm`, `automation`, `databases`) — only show tools whose `category` array contains that id. Category ids are kebab-case; match case-insensitively.

If an argument isn't a number and isn't a known category id, treat it as a free hint and mention you ignored it.

## Steps
1. Fetch `https://riseofmachine.com/api/v1/index.json` once. Keep `updated` (the newest tool's `dateAdded`) for the freshness line.
2. Fetch `https://riseofmachine.com/api/v1/tools.json` once — this is page 1, the 24 newest summary records. The API already sorts by `dateAdded` descending, but re-sort defensively by `dateAdded` desc anyway.
3. If a day filter is given and the oldest tool still on the current page is inside the window, fetch `/api/v1/tools/page/2.json`, then `/page/3.json`, and so on, until a page falls entirely outside the window or you reach the last page (`meta.totalPages`). Fetch each page at most once; stop as soon as the window is covered. Without a day filter, do not page past page 1.
4. Apply the day filter and/or category filter to the collected records.
5. To show readable category headings, fetch `https://riseofmachine.com/api/v1/categories.json` once and map each category id to its `title`. Fall back to the raw id if it isn't found.

## Output
- Open with one freshness line: `Newest tool added {updated}.`
- Group the results by category (a tool in multiple categories may appear under each). Use the category `title` as the heading.
- For each tool show: **title** — `tag` (pricing; if empty, write "pricing n/a"), added `dateAdded`, a one-line description, and the link `siteUrl`.
- If nothing matches the filters, say so plainly (e.g. "No tools added in the last N days" / "No tools in category X") and suggest widening the window.
- End with a one-line honest note that this is newly-added, not popular or trending.

## Failure handling
- If any fetch fails, say which endpoint failed and show whatever you already have. Do not invent tools or dates.
- Handle empty `tag` and null `ogImage` / `githubUrl` without erroring.
