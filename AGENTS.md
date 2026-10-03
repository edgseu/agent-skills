# Context hygiene — headroom + context-mode (global, all agents)

Both MCP servers are installed globally and are complementary, not competing:
- context-mode (prevention): sandbox execution + FTS5 index. Raw data never enters context.
- headroom (safety net): compresses whatever large output still reaches the model; retrieve via hash.

## Default-mandatory routing

1. THINK IN CODE: analyze/count/filter/compare/search/parse/transform data by writing
   code via `ctx_execute` / `ctx_execute_file` (`console.log()` only the answer).
   Never pull raw logs, CSVs, issue lists into context.
2. GATHER ONCE: `ctx_batch_execute(commands, queries)` — one call, many commands,
   auto-indexed. Follow up with `ctx_search(queries: [...])` — all questions in ONE call.
3. WEB: `ctx_fetch_and_index(url, ...)` then `ctx_search`. No `curl`/`wget` in shell,
   no inline `fetch()` in shell, no direct fetch of large pages into context.
4. SHELL stays small: shell only for `git, ls, mkdir, rm, mv, cd, npm/pip install`.
   Anything with >20 lines of output goes through the sandbox.
5. RETRIEVE, don't re-read: compressed markers (`hash=...`) resolve via
   `headroom_retrieve` (headroom) or `ctx_search` (context-mode).
   Never re-fetch the raw source unless search says to.
6. MEMORY FIRST: after resume/clear/compact, `ctx_search(sort: "timeline")`
   before asking the user what was decided.

Tool-name note: depending on the client these appear prefixed
(`ctx_execute`, `context-mode_ctx_execute`, `mcp__context-mode__ctx_execute`).
Same tools — use them.

## Override (logged, not silent)

If a `ctx_*` tool is missing or broken in this session, write one line —
`Override: <tool> unavailable because <reason>, fallback <X>` —
and continue with the closest native tool. Never loop on a blocked command;
never silently ignore these rules.

## Agent skills

### Issue tracker

Local-markdown issues under `.scratch/<effort>/issues/<NN>-<slug>.md` and specs under `.scratch/<effort>/spec.md`.

### Triage vocabulary

Default five canonical roles recorded as `Status:` lines in ticket files (`needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`). Documented in `CONTEXT.md`.

### Domain docs

Single-context: `CONTEXT.md` at repo root for domain vocabulary and triage roles, plus `docs/adr/` for architectural decision records.
