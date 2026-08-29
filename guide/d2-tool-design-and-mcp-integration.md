# Domain 2 — Tool Design & MCP Integration (18%, ~11 items)

Two clusters: how tools are described and scoped so the model picks
correctly, and how MCP servers are wired so the client discovers and trusts
them.

## Objective map (paraphrased from the published exam guide)

- **2.1 Tool interface design** — descriptions, input contracts, and naming
  as the levers of selection accuracy.
- **2.2 Structured error responses** — error shapes a model can act on.
- **2.3 Tool distribution** — how many tools an agent should hold, and
  controlling which are callable.
- **2.4 MCP integration** — server scope, authentication via environment
  variables, transports, discovery, and resources vs tools.
- **2.5 Built-in tool selection** — Read/Write/Edit/Bash/Grep/Glob: which
  one a task actually calls for.

## Mental models

**The description is the contract.** The model selects tools by description
text; the name is nearly decoration. Write descriptions like onboarding a
junior engineer: purpose, when to call, when *not* to call, input formats
with examples, what success and failure look like. When tools get
misselected, fix descriptions first, rename second, merge/split last.
Keep roughly 4–5 tools per agent — selection accuracy degrades sharply past
that, and a general-purpose tool inside a specialized agent is scope creep.

**Errors are data, not exceptions.** A raised exception crashes the loop;
a structured error lets the agent recover. Return `is_error: true` with an
error category, a retryable flag, and a message that tells the model what to
do next. Four categories map to four model actions: `transient` → retry,
`validation` → reformulate the call, `permission` → escalate (do not retry),
`business` → escalate. Keep a **valid empty result structurally distinct
from an access failure** — if "no rows" and "not allowed" look identical,
every recovery decision downstream is corrupted. Never swallow exceptions;
an empty success makes the model confidently assert falsehoods.

**MCP scope is a deployment decision, not a style choice.** `.mcp.json` at
the repo root is project scope: committed, cloned with the repo, the right
home for team-wide servers. `~/.claude.json` is user scope: personal,
machine-local, never committed. The two merge at runtime. There is **no
`.claude/mcp.json`** — config placed there is silently ignored.
`.claude/settings.json` holds permissions and hooks; its only MCP keys are
the `enabledMcpjsonServers` / `disabledMcpjsonServers` approval toggles,
never server definitions. Secrets go through `${ENV_VAR}` expansion (valid
in `env`, `args`, `headers`, `url`) so one committed definition carries a
different secret per machine — in CI the variable must be exported into the
actual process. Three transports: stdio (local subprocess, most common),
SSE, and HTTP.

**Discovery is runtime, and you verify it.** Tool discovery happens when the
client connects — `list_tools()` at connection time is what makes a newly
deployed server visible with no client-code change. When a server's tools
don't appear, verify discovery first (list what the client actually sees)
before suspecting the server logic.

**Resources vs tools — know the mechanism, not just the slogan.** Tools are
actions; resources are read-only reference data (catalogs, policies,
schemas). The reason converting static reference content to resources is
*faster* is the round-trip: **every tool call costs a full model reasoning
turn — the model emits a `tool_use` block, the client executes, the result
comes back, and the model runs again. A resource is read directly by the
client with no model turn at all.** Moving exploratory lookups to resource
reads removes entire model turns, which is where the latency win comes
from — not from a faster per-call server response. If you can only recall
"resources are faster" but not why, you will miss the variants.

**Glob vs Grep — the call-site question.** Glob matches *file paths* against
a pattern; it returns names and tells you nothing about contents. Grep
searches *file contents*. "Find every place `processOrder(` is called"
(before a rename or a signature change) is a Grep task — on the function
name and its aliases — followed by Read on each match. Glob cannot answer
"who calls this." And prefer the built-in over the Bash equivalent: content
search is `Grep`, not `find | grep` via Bash. When Edit's anchor text isn't
unique, Read + Write the whole file rather than chaining fragile edits.

**Drill:** `concept-drills/d2-concept-drill.md`
**Traps:** config-scope inverse framing; tool-scoping vs topology
attribution (`guide/trap-patterns.md`)
