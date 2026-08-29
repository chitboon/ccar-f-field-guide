# Domain 1 — Agentic Architecture & Orchestration (27%, ~16 items)

The heaviest domain. Most of its items reduce to one question: *where does
control live — in the model's prose, or in the structure around it?*

## Objective map (paraphrased from the published exam guide)

- **1.1 Agentic loops** — the loop is code you write around the model; drive
  it off `stop_reason`, not off anything the model says.
- **1.2 Multi-agent orchestration** — coordinator–subagent topology:
  decomposition, delegation, synthesis, and where failures belong.
- **1.3 Subagent invocation** — what context a subagent does and does not
  get, and the mechanics of spawning.
- **1.4 Enforcement and handoff** — making required steps actually happen;
  structured handoffs between agent and human.
- **1.5 Hooks** — intercepting tool calls programmatically (Agent SDK hooks)
  to enforce policy or normalize data.
- **1.6 Task decomposition** — fixed chains vs dynamic decomposition;
  matching the strategy to the workflow's shape.
- **1.7 Session state** — resumption, forking, and when stale context means
  start fresh.

## Mental models

**The agent is the loop, not the model.** The platform primitive is
`messages.create` plus a branch on `stop_reason`: `tool_use` → execute, append
`tool_result`, loop; `end_turn` → stop. Six values exist (`end_turn`,
`tool_use`, `max_tokens`, `stop_sequence`, `pause_turn`, `refusal`) — handle
the unusual ones deliberately; never blind-retry. Text in a response is not a
completion signal: `text` and `tool_use` blocks coexist. Iteration caps are a
safety backstop, never the primary stop mechanism. Parsing prose to decide
"it's done" is how production agents go feral.

**Hub-and-spoke is the only tested topology.** The coordinator owns
decomposition, dynamic subagent selection, error handling, and synthesis.
Subagents never call peers; there is no second orchestrator; recovery policy
lives at the single control point. Subagents get **fresh context** — no
inherited history, no shared memory — so everything they need is restated in
the invocation prompt, and handoffs are structured (`{claim, evidence,
source, confidence}`), never daisy-chained transcripts. Parallel work means
N `Task` calls in one assistant response; the coordinator's `allowedTools`
must include `Task`, and leaf subagents' must not (recursion). The
signature failure pattern: **all subagents succeed, but the output is wrong
→ the coordinator's decomposition or spec is the defect** — the subagents
executed what they were told.

**Deterministic beats probabilistic.** If a required step is skipped some
percentage of the time, the fix is a programmatic gate — a hook or an
enforced tool-ordering prerequisite — never a stronger prompt, never more
examples. Money, ordering, and compliance do not belong to probability.
PreToolUse hooks gate *before* execution (block a policy-violating refund and
hand back a structured error so the model re-plans); PostToolUse hooks
normalize *after* execution, before the model sees the result (timestamps,
status codes, stripping unused fields). A hook that fails must fail closed.

**Match decomposition to structure.** Known, fixed steps (security pass,
style pass, perf pass) → prompt chaining: sequential focused passes with
dedicated criteria each. Unknown, exploratory structure ("investigate",
"map this") → dynamic decomposition with a coordinator that evaluates
coverage gaps and re-delegates. Goal-oriented delegation beats procedural
micromanagement.

**Sessions:** `--resume` continues a named session with full history;
forking branches a shared baseline for parallel A/B exploration. If prior
tool results went stale (files changed underneath), start a fresh session
with an injected structured summary — do not resume over stale
`tool_result` blocks.

**Drill:** `concept-drills/d1-concept-drill.md`
**Traps:** stop_reason timing; tool-scoping vs topology attribution
(`guide/trap-patterns.md`)
