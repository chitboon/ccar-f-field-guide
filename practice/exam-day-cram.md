# CCAR-F Exam Day Cram Sheet

**Claude Certified Architect – Foundations (CCAR-F)**
60 items (all single-answer — four options, one correct; no multi-select) · 120 min · Pass 720/1000 · Pearson VUE
Weights: **D1 27% · D3 20% · D4 20% · D2 18% · D5 15%**

---

## The Golden Rule Hierarchy

1. **Deterministic > probabilistic.** Money/ordering/compliance → hook, gate, or permissions config — never a prompt instruction.
2. **"Eliminate / guarantee / most reliable" → structural mechanism.** JSON schema via tool use, not "please return valid JSON."
3. **Facts that must survive → pin them explicitly.** Case-facts blocks and scratchpads, not summarization.
4. **Metrics questions → attack the measurement.** Aggregate accuracy hides weak segments; self-reported confidence is uncalibrated.
5. **Annotate, don't arbitrate.** Conflicting credible sources: preserve both + attribution. Never average.
6. **Errors propagate as errors.** Never empty-success; never "complete" with failure buried in prose.

---

## My 5 Mock Misses (read twice)

1. **Fixed structure → prompt chaining, not agents.** Known, defined aspects (security/style/perf review) → sequential focused passes, each with dedicated criteria. Multi-agent decomposition is for *unknown/dynamic* structure.
2. **Built-in tool > Bash.** Content search → `Grep` directly, not `find`+`grep` via Bash. (Paths → `Glob`.)
3. **Edit anchor not unique → Read + Write the whole file.** Don't chain fragile anchored Edits.
4. **Interview pattern** for unfamiliar legacy systems: have Claude question YOU before designing.
5. **Skill `allowed-tools` gates built-ins only** — MCP access is inherited from the parent session. No `allowed-servers` field exists.

---

## D1 — Agentic Architecture (27%)

- Loop control: branch on `stop_reason` — `tool_use` → continue, `end_turn` → stop. Never parse prose. (`pause_turn`, `refusal` exist.)
- `text` + `tool_use` blocks can coexist — text is NOT a completion signal.
- All subagents succeed but output wrong → **coordinator's decomposition/spec is the defect**.
- Subagent context = explicit-only. No inheritance, no shared memory, no CLAUDE.md. Restate everything in the invocation prompt.
- Parallel subagents = N Task calls in ONE response. Coordinator needs `"Task"` in `allowedTools`.
- Spawning/recovery decisions stay with the **coordinator** (hub-and-spoke). No peer-to-peer, no second orchestrator.
- Fork sessions for divergent branches from a shared baseline; fresh session + injected summary when tool results went stale.
- Fixed pipeline misses coverage → coordinator evaluates gaps and conditionally re-delegates.
- Iteration caps = safety boundary, never the primary stop mechanism.

## D2 — Tool Design & MCP (18%)

- Tool misselection → **fix descriptions first** (inputs, examples, "use this vs. that"); then rename; merge/split last.
- Recovery after failure → structured error metadata: `errorCategory` (transient→retry / validation→reformulate / permission→escalate / business→escalate) + `isRetryable`.
- ~4–5 tools per agent. General-purpose tools in specialized agents = scope creep → replace with capability-limited tools.
- High-frequency simple need → one narrow scoped tool locally; complex cases → coordinator.
- **MCP resources = read-only reference data** (catalogs, policies); **tools = actions**. Static data discovered every session → make it a resource.
- Valid empty result ≠ access failure — keep them distinguishable.
- `.mcp.json` (project, committed) with `${ENV_VAR}`; `~/.claude.json` (user). Secrets never committed. CI: the var must be exported into the actual process.
- Glob finds files, Grep finds text.

## D3 — Claude Code & CI (20%)

- CLAUDE.md hierarchy: user (`~/.claude`, not in VCS) → project root → subtree → CLAUDE.local.md. Team standards → project level.
- Commands: `.claude/commands/` (shared) vs `~/.claude/commands/` (personal).
- File-type conventions → `.claude/rules/*.md` with `paths:` globs (works across directories).
- Plan mode for multi-file/architectural; direct execution for clear single-file fixes.
- **Permissions (`settings.json`) = static allow/deny; hooks = conditional/dynamic logic.**
- PreToolUse = gate before; PostToolUse = normalize after. Narrow the matcher to scope hooks.
- CI: `claude -p` (`--print`) — `CLAUDE_HEADLESS`/`--batch`/`--ci` don't exist. `--output-format json` + `--json-schema` for machine-parseable output.
- One fresh session per PR; reused sessions contaminate.
- `max_tokens` = OUTPUT length, not context window.

## D4 — Prompt Engineering & Structured Output (20%)

- Canonical structured output: schema → tool `input_schema` → forced `tool_choice`. Strict kills **syntax**, not **semantics** (arithmetic, wrong-field) → validate app-side.
- `tool_choice`: auto (may prose) / any (some tool) / forced (that tool) / none. "any" guarantees *a* call, not the *right* one.
- Required fields the source lacks → model fabricates. Fix: nullable + "return null if not stated"; catch hallucination with `source_location` grounding fields.
- Retries need feedback: original doc + failed output + specific error. Same prompt → same failure. Retry ceiling 1–2, then human.
- Vague instruction failing → **explicit criteria + concrete examples** (never stronger wording, never temperature).
- Few-shot (2–3 examples) beats prose specs and temperature.
- **Batches API**: ~50% off, up to 24h, no SLA, `custom_id`, no multi-turn tools. Latency-tolerant only; resubmit only failed `custom_id`s.
- "Typically finishes in ~2h" ≠ guarantee. Blocking + SLA → synchronous.
- Many rule sets in one pass → attention dilution → split passes or deterministic validation layer.
- Generator ≠ reviewer: independent review instance; multi-pass agreement → automate, disagreement → human.

## D5 — Context & Reliability (15%)

- Lost-in-the-middle → key findings FIRST + explicit headers. Long synthesis → map-reduce (per-source detail, then reconcile).
- Case-facts block pinned top of every prompt; scratchpad files across phases. `/compact` = fallback.
- Escalate ONLY on: explicit request (immediately) / policy gap / no meaningful progress. NOT sentiment, NOT self-confidence, NOT turn counts.
- Hand off structured summary (who, what, tried, blocked) — never raw transcript.
- Local recovery first in subagents; propagate unresolvable errors with failure type + attempted + partial results.
- Partial results vs retry vs escalate → judge sufficiency vs what was asked. No "always" rules.
- "Could not verify" must be a structurally distinct status from "verified fine".
- Confidence routing: calibrate threshold on labeled validation set; stratified-sample the auto-approved slice to catch high-confidence fabrication.
- Ambiguous identity (3 matching accounts) → ask for another identifier. Never heuristic-pick.

---

## Logistics

- Pearson VUE, online or test center · govt ID matching registration exactly · no second monitor/phone/notes
- Answer EVERYTHING — no guessing penalty. Flag and return; ~2 min/question.
- Results in ~2 business days with per-domain breakdown.
