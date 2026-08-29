# CCAR-F Detailed Cheat Sheet

**Claude Certified Architect – Foundations** · 60 items · 120 min · pass at 720/1000 · Pearson VUE (online or test centre)
Format: every item is **single-answer — four options, one correct** (no multi-select). Distractors are closely spaced: usually the plausible prompt-level or over-engineered alternative to the correct mechanism.
Weights: **D1 27% · D3 20% · D4 20% · D2 18% · D5 15%** → D1+D3+D4 = 67% of the exam.

How to use: Section 1 is the decision rules that answer most questions. Sections 2–6 are per-domain deep dives, with mechanism-level "how do I actually do this" detail where the exam tests it. Section 7 is a real candidate's error log, reframed as the confusions worth pre-empting. Section 8 is antipatterns, Section 9 logistics. ~1 hour read.

---

## 1. The Golden Rules (answer 70% of questions with these)

1. **Deterministic beats probabilistic.** Money, ordering, compliance, refunds → hook, gate, or permissions config. Never a prompt instruction, never few-shot. Prompts advise; hooks enforce. If 12% of cases skip a required step, the fix is a programmatic prerequisite gate — not a stronger prompt.
2. **"Eliminate / guarantee / most reliable" → structural mechanism.** JSON schema via tool use with forced `tool_choice`, not "please return valid JSON." Schema-conformant by construction beats prompt self-checking every time.
3. **Facts that must survive → pin them explicitly.** Case-facts blocks at the top of every prompt, scratchpad files across phases. Summarization is what *causes* precision loss — `$847.50 on March 3rd` decays into "the overcharge from last month."
4. **Metrics questions → attack the measurement.** 97% aggregate accuracy hides 80% on one segment. Stratify by document type AND field. Self-reported confidence is uncalibrated — a routing key, not truth.
5. **Annotate, don't arbitrate.** Conflicting credible sources → preserve both with attribution. Never average (invents data), never heuristic-pick (buries the conflict).
6. **Errors propagate as errors.** Never empty-success. Never "complete" with failure buried in prose. Never swallow exceptions in tools.
7. **Vague instruction failing → explicit criteria + concrete examples.** Never stronger wording, never temperature. Few-shot (2–3 examples) beats prose specs.
8. **Generator ≠ reviewer.** The session that wrote the code retains generation reasoning — use an independent review instance.
9. **Escalate ONLY on:** explicit request (immediately, no clarifying question), policy gap, or no meaningful progress. NOT sentiment. NOT self-reported confidence. NOT turn counts.
10. **The coordinator owns everything** in multi-agent: decomposition, spawning, recovery, synthesis. No peer-to-peer subagents, no second orchestrator.

---

## 2. D1 — Agentic Architecture & Orchestration (27%)

### The agent loop
- The agent is not the model. **The agent is the loop around the model.** The model requests; your code decides and executes.
- Platform primitive: `client.messages.create()` + **branch on `stop_reason`**. Everything else is layers on this.
- **`stop_reason` enum (6 values):** `end_turn` (genuinely done → stop), `tool_use` (execute tools, append `tool_result`, loop), `max_tokens`, `stop_sequence`, `pause_turn` (resumable long turn — handle deliberately), `refusal` (streaming policy intervention).
- **Never parse prose to detect completion.** "It said thanks, so it's done" is how production agents go feral.
- **`text` and `tool_use` blocks can coexist in one response** — text is NOT a completion signal.
- Iteration caps = safety backstop, **never the primary stop mechanism**.
- Non-`end_turn`/`tool_use` stop reasons: handle deliberately, never blindly retry.

### Multi-agent (hub-and-spoke is the ONLY topology tested)
- Coordinator owns: decomposition, dynamic subagent selection, error handling, synthesis. **Subagents never call peers directly.**
- **Subagents get FRESH context** — no inheritance, no shared memory, no CLAUDE.md, no conversation history. Restate everything in the invocation prompt. Context isolation is structural, not a prompt instruction — subagent tool blocks never appear in the coordinator's message array.
- All subagents succeed but output is wrong/incomplete → **the coordinator's decomposition/spec is the defect.** Subagents executed their narrow assignments as specified; the defect lies in what they were told to do.
- Spawning: the **`Task` tool** — coordinator's `allowedTools` **must include `"Task"`**; leaf subagents' `allowedTools` deliberately exclude `Task` (prevents recursion).
- **Parallel subagents = N Task calls in ONE assistant response** — not multiple turns.
- Structured handoff, not prose: each subagent returns `{claim, evidence, source, confidence}`. **Never daisy-chain full conversation logs** between subagents (token cost scales superlinearly, signal collapses).
- Split when: subtasks have independent success criteria, context bloat hurts, or you want per-agent tool scope. **Don't split** when agents share state you'd have to serialize, or they'd just chat at each other.
- Recovery policy lives with the coordinator (single control point). No separate "error-handling agent" issuing restart commands. Subagents do **local recovery first**; only unresolvable errors propagate up with failure type + what was attempted + partial results.
- Fixed pipeline misses coverage → coordinator evaluates gaps and conditionally re-delegates.

### Task decomposition & sessions
- **Fixed, known steps → prompt chaining** (sequential focused passes, each with dedicated criteria — e.g. security/style/perf review). **Unknown/dynamic structure → agents / dynamic decomposition.** "Analyze each X then Y" → chain. "Investigate/explore" → dynamic.
- Goal-oriented delegation beats procedural micromanagement — over-decomposed rigid scripts break on environment shifts.
- **`--resume`** continues a named session (full history). **`fork_session`** branches for parallel A/B exploration from a shared baseline.
- **If prior tool results went stale (files changed) → FRESH session + injected structured summary.** Don't resume with stale `tool_result` blocks. On resume, filter old tool_results so the agent re-fetches live data.
- Unfamiliar legacy system → **interview pattern**: have Claude question YOU before designing.
- Pick the smallest model that does the job (Haiku-class runs the loop at a fraction of Sonnet cost).

---

## 3. D2 — Tool Design & MCP (18%)

### Tool descriptions
- **The tool description is the contract, not the name.** The model selects tools by description text — the single biggest lever for tool-use accuracy. Write it like onboarding a junior engineer: purpose, when to call, when NOT to call, input formats with examples, success/failure shapes.
- **Tool misselection → fix descriptions FIRST** (inputs, examples, "use this vs. that"). Then rename. Merge/split last.
- Second lever: split generic tools into purpose-specific ones (`analyze_document` → `pull_line_items` / `flag_anomalies` / `rate_document_risk`).
- **~4–5 tools per agent max.** Selection accuracy degrades sharply past that. General-purpose tools in specialized agents = scope creep → replace with capability-limited tools. 18 tools "for flexibility" = wrong.
- High-frequency simple need → one narrow scoped tool locally; complex cases → coordinator.
- **Edit anchor not unique → Read + Write the whole file.** Don't chain fragile anchored Edits.

### Glob vs Grep — the call-site discipline (mechanism, not slogan)
- **Glob matches file PATHS against a pattern** (`**/*.ts`). It returns names and says nothing about contents.
- **Grep searches file CONTENTS.** "Find every place `processOrder(` is called" — before a rename or signature change — is a **Grep** task: search the function name (and its aliases/derived forms), then Read each match, then Edit.
- Glob cannot answer "who calls this", even with a perfect pattern. Listing every source file to read them all is the expensive way to do Grep's job.
- **Built-in tool > Bash equivalent** — content search → `Grep` directly, not `find`+`grep` via Bash. Paths → `Glob`.
- This one rewards having actually run it once: do a single real Grep-for-call-sites pass in any repo and the distinction stops being declarative.

### Structured tool errors
- **Errors return as `tool_result` content, never as raised exceptions.** An exception crashes the loop; a structured error lets the agent recover. Empty-string-on-failure is almost as bad (model assumes success).
- Shape: `is_error: true` + `errorCategory` + `isRetryable` + a message telling the model what to DO next.
- **Four categories → four model actions:** `transient` → retry · `validation` → reformulate the call · `permission` → escalate, don't retry · `business` (policy, e.g. refund cap) → escalate, don't retry.
- Identical "Operation failed" for timeouts/invalid amounts/fraud blocks = the anti-pattern this fixes.
- Never `except Exception: pass` in a tool — swallowed errors make the model confidently assert falsehoods.
- **Valid empty result ≠ access failure** — keep them distinguishable.

### MCP configuration
- **`.mcp.json` at repo root** = project scope (committed, team-wide). **`~/.claude.json`** = user scope (personal, never committed). Merged at runtime. **Scope selection rule: who must have the server decides the tier** — team tooling → project; personal/experimental → user.
- **There is NO `.claude/mcp.json`** — putting config at that path is silently ignored. `.claude/settings.json` carries permissions/hooks; its only MCP keys are `enabledMcpjsonServers`/`disabledMcpjsonServers` approval toggles, never server definitions.
- **Three transports:** stdio (`command`+`args`, local subprocess, most common), SSE (`type:"sse"`, `url`, `headers`), HTTP (`type:"http"`).
- **`${ENV_VAR}` expansion** in `env`, `args`, `headers`, `url` — secrets in shell env, never committed. One committed server definition then carries a different secret per machine. CI: the var must be exported into the actual process.
- **Discovery is runtime and you verify it:** `list_tools()` at connection time is what makes a newly deployed server visible with no client code change. Tools missing after deploy → list what the client actually discovered BEFORE debugging the server.
- **MCP resources vs tools — the round-trip mechanism.** Tools = actions; resources = read-only reference data (catalogs, policies, DB schemas). Why resources are faster: **every tool call costs a full model reasoning round-trip** (model emits `tool_use` → client executes → result returns → model runs again); **a resource is read directly by the client with no model turn at all.** Converting exploratory lookups to resource reads removes whole model turns — that, not a faster per-call server response, is where the latency win comes from.
- FastMCP server = three decorators (`@mcp.tool`, `@mcp.resource`, `@mcp.prompt`) + `mcp.run(transport="stdio")`.

---

## 4. D3 — Claude Code Configuration & Workflows (20%)

### CLAUDE.md hierarchy (4 tiers, unioned — no single winner)
1. **User `~/.claude/CLAUDE.md`** — personal defaults, every project, **NOT in VCS** (the classic "new teammate missing instructions" trap).
2. **Project `./CLAUDE.md`** — team contract, checked in. Team standards belong HERE.
3. **Subtree `<subdir>/CLAUDE.md`** — loads on demand when files in that subtree are read.
4. **Local `CLAUDE.local.md`** — gitignored personal tweaks.

- Files above working dir load in full at launch; subdirectory files load on demand. Conflicts are model-interpreted — write non-conflicting rules or state precedence explicitly.
- **`/memory`** shows which files are loaded — first diagnostic when behavior diverges between teammates.
- **`@import` syntax** (`@./testing.md`) keeps root CLAUDE.md modular.
- **File-type conventions spread across the codebase → `.claude/rules/*.md` with YAML `paths:` globs** (e.g. all `**/*.test.tsx` regardless of directory). Loads only when a matching file is read. Beats scattering subtree CLAUDE.md files.
- **CLAUDE.md vs Skill:** CLAUDE.md = always-loaded universal standards. Skill = on-demand task-specific workflow. 400-line CLAUDE.md → split workflows into skills.

### Commands & skills
- Slash commands: `.claude/commands/<name>.md` (project, shared via VCS) vs `~/.claude/commands/<name>.md` (personal). User types `/name`.
- Skills: `.claude/skills/<name>/SKILL.md` or `~/.claude/skills/<name>/SKILL.md`. Model discovers and invokes when relevant — **the description is the search query**; write it like a help-desk ticket title.
- **Three SKILL.md frontmatter keys:** `context: fork` (isolated subagent context so verbose output doesn't pollute main conversation), `allowed-tools` (restricts tool surface — **gates built-in tools only; MCP access is inherited from the parent session, and there is NO `allowed-servers` field**), `argument-hint`.
- Personal override of a team skill: user-scope copy, not editing the project file.

### Hooks vs permissions
- **Permissions (`settings.json`) = static allow/deny. Hooks = conditional/dynamic logic.**
- **PreToolUse = gate BEFORE execution** (policy enforcement, e.g. $500 refund cap — block and return a structured error so the model re-plans). **PostToolUse = normalize AFTER execution, before model sees result** (Unix→ISO timestamps, status-code mapping, strip 35 of 40 unused fields).
- Narrow the matcher to scope hooks. A hook error must **fail closed**: structured error to the model, log loudly. Silent hook failure = policy quietly evaporates.
- Defense in depth: prompt → tool description → hook. Bouncer, not a sign on the wall.

### The three-way workflow decision (plan mode is only one of THREE options)
- **Direct execution:** single-file bug fix, clear stack trace, low risk, no approval needed.
- **Plan mode:** ONE plan → approve → execute cycle. Multi-file/architectural changes, competing approaches, needs a single human sign-off before edits. Complexity stated in the requirements → pick plan mode upfront; "start direct, switch later" is the anti-pattern. Pair with the **Explore subagent** for verbose discovery.
- **Multi-phase workflow:** a SEQUENCE of discrete phases with a checkpoint between stages (explore → plan → implement → review, or a pipeline where each stage gates the next). Choose it when risk is spread across stages, each stage needs its own verification or human approval, or later phases depend on what earlier phases discover.
- **Selection criteria = task scope + risk level + human-approval requirements.** Read the scenario for where the approvals live: one approval before execution → plan mode; approvals/verifications BETWEEN stages → multi-phase.

### CI & scripted startup
- **CI = `claude -p "<prompt>"` (`--print`).** Non-interactive, stdout, exits. **`CLAUDE_HEADLESS` env var and `--batch`/`--ci` flags DO NOT EXIST** — they're planted distractors.
- `--output-format json` + `--json-schema <schema>` = machine-parseable CI output. `--allowedTools "Read,Edit,Bash"` restricts surface.
- **Fast scripted startup: `--bare`.** Non-interactive invocations pay for auto-discovery of skills, commands, and the full settings hierarchy on every call. When the script already knows the prompt content it needs (e.g. fixed corporate-standards text), `--bare` skips discovery and **`--system-prompt-file` / `--append-system-prompt`** inject that content directly — discovery is what would have produced it, so skipping it only works because you supply it yourself.
- **One fresh session per PR** — reused sessions contaminate. Independent review instance (generator ≠ reviewer). CLAUDE.md is auto-read by CI-invoked Claude Code — put review criteria there.
- `max_tokens` = OUTPUT length, not context window.

---

## 5. D4 — Prompt Engineering & Structured Output (20%)

### The JSON strictness spectrum (know all four rungs and the decision rule)
1. **Prompt-only formatting** ("return only JSON") — no guarantee; fine for human-read output, wrong for pipelines.
2. **Prefilled assistant turn** — open the assistant's reply with `{` or the expected prefix; the model is now CONTINUING a JSON document, which kills preamble and code fences without any tool machinery. No schema validation.
3. **Tool use + JSON schema** — Pydantic model → `model_json_schema()` → tool `input_schema`; output schema-conformant by construction.
4. **Tool use + schema + forced `tool_choice`** — `tool_choice = {"type":"tool","name":"extract_invoice"}`; the model MUST call that tool, so a schema-valid payload arrives every turn.
- **Decision rule:** how fragile is the consumer? Human eyes → rung 1–2. A parser → 3. A parser plus a hard guarantee that a structured call happens on every turn → 4. Don't buy a stronger rung than the consumer needs — over-engineering is a distractor too.

### `tool_choice` — four modes, four guarantees
- **`auto`** (default): may call a tool OR answer in prose.
- **`any`**: MUST call SOME tool — guarantees *a* call, not the *right* one.
- **`{"type":"tool","name":X}`**: MUST call THAT tool — the structured-output cheat code.
- **`none`**: tools registered but uncallable.
- **Enforced call-ordering pattern:** force a named tool on every call and let the orchestrator layer pick WHICH name each turn gets — ordering becomes programmatic, not a prompt request.
- `disable_parallel_tool_use: true` = one tool per turn, when ordering matters.

### Strictness limits
- **Strict kills JSON SYNTAX errors, not SEMANTIC errors.** `{"total": 100, "line_items": [50, 30]}` is schema-valid and arithmetically wrong → validate sums/rules application-side.

### Hallucination & retry
- **Required fields the source lacks → model fabricates plausible values to satisfy the schema.** Fix: nullable fields (`Optional[X] = None`) + **"Return null if the value is not directly stated in the source. Do not infer."** — single highest-ROI instruction. Catch hallucination with `source_location` grounding fields. Enum variant: an "other" bucket + free-text detail field beats forcing every case into the nearest label.
- **Two failure classes, two treatments:** format/schema errors → append original doc + failed output + specific `ValidationError` as a user turn and retry (succeeds within 2–3 tries). Missing-information errors → retries DON'T help; the data isn't in the source.
- **Hard retry ceiling: 1–2, then human.** Same prompt → same failure. Infinite retry on missing info = budget leak. Fail loud.
- A `detected_pattern` label on each finding turns outputs into records you can group and count — systematic feedback instead of anecdote.
- Regex beats the model for deterministic formats (phone, ISO dates). Nested schemas: 2–3 levels fine; past that, split into two extraction calls.

### Few-shot, criteria & boundaries
- **Few-shot (2–3 input/output examples) beats prose specs and temperature.** Locks formats prose can't reach: decimal commas, DD/MM/YYYY, "what NOT to flag."
- "Be accurate" is not a prompt — it's a wish. Replace with explicit categorical criteria + acceptance checklist + concrete examples.
- **Criteria run BOTH directions: inclusion AND exclusion.** A prompt that says what to find but not what to skip gets findings in categories where the model is unreliable. Name the out-of-scope classes ("do not report X, Y") with examples of acceptable instances.
- False-positive flood (security review flags 60% of PRs): disable the category temporarily + rewrite its criteria with reportable-vs-acceptable examples. Self-reported confidence thresholds don't fix a broken category.
- Many rule sets in one pass → attention dilution → split passes or add a deterministic validation layer.

### Batches API
- **~50% off, up to 24h window, no SLA, `custom_id` correlation, NO multi-turn tool calling.**
- Latency-tolerant only: overnight reports YES, blocking pre-merge checks NO. "Typically finishes in ~2h" ≠ guarantee. Blocking + SLA → synchronous.
- On batch failure: resubmit only failed `custom_id`s.

### Review & confidence routing
- **Multi-instance review (high-stakes):** 2–3 independent passes + judge pass; agreement → automate, disagreement → human. ~3× cost, catches systematic bias. **Anti-pattern: flagging only issues found by ≥2 passes** — suppresses uniquely-caught real bugs.
- Confidence routing: self-reported `confidence` field, route against a threshold (course uses 0.7). High → automated, medium → spot-check, low → human queue.
- **Not calibrated — treat as routing key, not truth.** Calibrate the threshold against a **labeled validation set**. Segment accuracy by document type AND field. **Stratified-sample the auto-approved slice** to catch high-confidence fabrication and drift.

---

## 6. D5 — Context Management & Reliability (15%)

### Context management
- **Lost-in-the-middle:** models attend hardest to top and bottom. Key findings FIRST + explicit section headers. Long synthesis → map-reduce (per-source detail, then reconcile).
- **Case-facts block pinned at the TOP of every turn** (account ID, amounts, dates, order IDs), outside summarized history. Mark it `cache_control` ephemeral.
- **Summarize resolved turns** into one-line narrative; keep verbatim history ONLY for the active issue. Summarization loses precise transactional details — that's why the case-facts block exists.
- **Prune verbose tool outputs application-side, before appending** (PostToolUse pattern). Tool returned 40 fields, you used 5 → strip 35. The model can't do this for you.
- **`/compact` = fallback, not a strategy.**
- Large codebases: start broad (CLAUDE.md/README) then pinpoint; **Grep for content, Glob for paths** (see §3 for the call-site discipline); Read before Edit; `.scratchpad.md` of findings instead of re-reading source; delegate verbose discovery to an Explore subagent.
- 155K tokens of raw content choking synthesis → **fix at the SOURCE: upstream agents return structured summaries**, don't trim downstream.

### Prompt caching
- `cache_control={"type":"ephemeral"}` on the last tool/system block ("cache everything up to here"). Max 4 explicit breakpoints.
- Verify: `cache_creation_input_tokens` on call 1 (write), `cache_read_input_tokens` on call 2 (hit).
- **Silent minimum-size floor: 1024 tokens (Sonnet 4.x) / 4096 (Haiku 4.5).** Below the floor caching is silently ignored — no warning, counters read 0, looks like a hard miss. Count tokens first.
- Pricing: writes 1.25× base input, reads 0.1× (≈90% cheaper). 5-min TTL default; 1h TTL at 2× base.

### Escalation (heavily tested)
- **Three legitimate triggers ONLY:**
  1. **Explicit customer request** ("I want a human") — honor immediately, no clarifying question, no one-more-tool-call.
  2. **Policy gap/exception** — policy silent on the case, or resolution exceeds cap.
  3. **Inability to make meaningful progress** — multi-system failure, paths exhausted.
- **NOT triggers:** sentiment (frustration ≠ complexity — route on problem shape, not emotional temperature), self-reported confidence (poorly calibrated), turn counts, classifier-models-on-historical-tickets (over-engineered).
- Escalation decision quality bad → **explicit criteria in system prompt + few-shot examples of escalate vs resolve.** Root cause is unclear decision boundaries.
- **Hand off a structured summary, never the raw transcript:** who, what, what's been tried, what's blocked.

### Error propagation & provenance
- Subagents return structured error context: failure type + what was attempted + partial results + alternative approaches. Generic "search unavailable" hides the retry/reformulate/give-up decision.
- **Anti-patterns:** swallowing errors as empty success; terminating the whole workflow on one subagent failure; burying `is_error` in "success with metadata."
- Local recovery first; coordinator owns recovery policy.
- **Provenance is a required output field:** `{claim, evidence excerpt, source, publication date}`. No source, no claim. Include dates so stale data gets flagged.
- **Conflicting credible sources: annotate, don't arbitrate.** Averaging invents data; heuristic-picking buries the conflict.
- **Ambiguous identity (3 matching accounts) → ask for another identifier.** Never heuristic-pick.
- **"Could not verify" must be a structurally distinct status from "verified fine."**
- Partial results vs retry vs escalate → judge sufficiency vs what was asked. No "always" rules.

---

## 7. Error Log — the confusions worth pre-empting

One candidate's real miss pattern, kept because every miss had the same shape (the honesty caveat: this is a single candidate's log, not a statistical sample). When torn between two answers, ask: **"which one makes the failure impossible rather than less likely?"** Pick that one.

1. **Required step skipped 12% of the time** → programmatic prerequisite gate, not a stronger system prompt, not few-shot, not a routing classifier.
2. **"Eliminate JSON syntax errors"** → tool use with JSON schema. Prompt self-checks reduce but never eliminate.
3. **Precision decay across long conversations** → pinned case-facts block. `/compact` and better summarization are the cause refined, not the cure.
4. **"What is wrong with this 97%-accurate pipeline?"** → the aggregate masks weak segments; stratify. Don't reach for a policy absolute ("always retain human review") when the question asks about a measurement flaw.
5. **Fixed review structure (security/style/perf)** → prompt chaining, not multi-agent. Dynamic decomposition is for *unknown* structure.
6. **Content search** → built-in `Grep`, not `find`+`grep` via Bash. Paths → `Glob`.
7. **Edit anchor not unique** → Read + Write the whole file; don't chain fragile anchored Edits.
8. **Unfamiliar legacy system** → interview pattern: have Claude question YOU before designing.
9. **Skill `allowed-tools`** gates built-ins only — MCP access is inherited from the parent session; no `allowed-servers` field exists.
10. **The two traps that score 0% are silent third options:** "multi-phase workflow" alongside plan mode (§4), and "prefilled responses" on the JSON strictness spectrum (§5). Options you never drilled are invisible even when correct.

---

## 8. Antipatterns Checklist (the exam marks these WRONG)

- Parsing natural language to detect completion.
- Iteration cap as the primary stop mechanism.
- Sentiment-based escalation; self-reported-confidence-based escalation.
- Swallowed exceptions; empty-success error reporting; workflow-wide termination on one subagent failure.
- Daisy-chaining full conversation logs between subagents; subagents calling peers; a second error-handling orchestrator.
- One agent with 18 tools; general-purpose `fetch_url` where a scoped tool belongs.
- Arbitrating or averaging conflicting sources instead of annotating.
- Self-review by the generating session; ≥2-of-3 agreement gating in multi-pass review.
- Secrets committed in `.mcp.json`; MCP server defs in `.claude/settings.json`; expecting `.claude/mcp.json` to load.
- Monolithic 500-line CLAUDE.md for workflow-specific guidance (→ skills).
- Retrying when the info is absent from the source; no retry ceiling.
- Resuming a session whose tool results went stale (→ fresh session + injected summary).
- `CLAUDE_HEADLESS`, `--batch`, `--ci` flags (don't exist).
- "Always"/"regardless" policy answers when the question asks about a measurement flaw.
- Glob patterns to answer a content question ("who calls this"); prompt-only JSON for a pipeline that needs a guarantee.

---

## 9. Exam-Day Logistics & Tactics

- **Pearson VUE**, online proctored or test center. Government ID matching registration name **exactly**. Clean room, no second monitor, no phone, no notes.
- 60 items in 120 min = **~2 min/question**. All single-answer, four options, one correct — no multi-select, so never hunt for a second defensible option.
- **Answer EVERYTHING — no guessing penalty.** Flag unsure ones, finish the pass, return with remaining time.
- Results in ~2 business days with per-domain breakdown.
- $125 fee. Retakes allowed with a 14-day wait.
- When course material and Anthropic docs disagree, Anthropic wins.
- Read each scenario, recognize which of the 6 archetypes it is (support agent, code gen, multi-agent research, dev productivity, CI, structured extraction) → that tells you the domain emphasis.
- Distractors are *plausible-but-wrong*: usually the prompt-only or over-engineered alternative to the correct mechanism.
