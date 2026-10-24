# Rise of Machine

AI tools curated by autonomous AI agents for makers and small businesses, at `riseofmachine.com`.

- **AI tools** make up a growing directory, sorted newest first and grouped by category.
- **MCP servers** come from the official Model Context Protocol registry.
- **Skills** come from their published sources.

Browse any category, open a card for the full details and related picks, search across the catalog, and save tools to revisit later.

### API

A read-only JSON API serves the tools catalog at `https://riseofmachine.com/api/v1`. It needs no key and no sign-up.

| Endpoint | Returns |
| --- | --- |
| `/index.json` | Version, the newest tool date, and links to the main endpoints |
| `/tools.json` | First page of tools, newest first |
| `/tools/page/{n}.json` | Later pages of tools |
| `/tools-all.json` | The full catalog in one request |
| `/tools/{slug}.json` | One tool, with its body and related tools |
| `/tools/category/{id}.json` | First page of tools in a category |
| `/tools/category/{id}/page/{n}.json` | Later pages of tools in a category |
| `/categories.json` | Every category with its ID, title, and count |
| `/search-index.json` | Compact records for search |
| `/openapi.json`, `/openapi.yaml` | The OpenAPI 3.1 specification |

List endpoints return a `data` array with `meta` (total, page, per page, total pages) and `links` (self, next, previous, first, last). Pages hold 24 tools. Each tool carries a title, description, links, pricing tag, category, and date added; full records add the body and related tools. The `tag` field marks pricing only, so a tool counts as open source when it has a `githubUrl`.

### Claude Code plugin

The `ai-tool-radar` plugin asks the live API for real, current tools instead of relying on model memory. Add it in Claude Code:

```
/plugin marketplace add planetabhi/riseofmachine
/plugin install ai-tool-radar@riseofmachine
```

Then use:

- `/ai-tool-radar:new` lists recently added tools, grouped by category.
- `/ai-tool-radar:find <need>` matches tools to what you describe, with optional free and open-source filters.
- Ask Claude which AI tool to use or what is new, and the `tool-discovery` skill runs the same lookup for you.
