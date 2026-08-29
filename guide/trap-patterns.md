# Trap Patterns — CCAR-F

Fourteen judgement traps, each the shape of a question rather than a fact:
how the trap presents, a worked example, and the discrimination that
resolves it. Every worked example below was written for this file against a
named published exam objective — none is a reconstruction of any live-exam
or third-party item.

How to use: read the trap, cover the resolution, work the example, then
check yourself. If an example feels easy, that is the point — the trap is
that it stops feeling easy when it wears a different scenario.

---

## Seed patterns

### 1. stop_reason timing

**Objective 1.1 — agentic loops.**

**The trap:** the model's *text* sounds finished, and an option treats that
as the stop signal — or a loop condition keys on the wrong `stop_reason`.

**Worked example.** A support agent's loop continues whenever the response
contains text. One turn, the model writes "Let me check that order for you"
and, in the same response, requests `lookup_order`. The loop sees text,
concludes the agent is wrapping up, and exits — the tool never runs. Which
single change fixes it? (A) Add "always finish by confirming tool results"
to the system prompt. (B) Branch the loop on `stop_reason`: continue on
`tool_use`, stop on `end_turn`. (C) Lower the temperature so responses are
more decisive. (D) Cap iterations at 25 so the loop cannot exit early.

**Resolution: B.** `text` and `tool_use` blocks coexist in one response;
prose is never a completion signal. The loop's contract is `stop_reason`.
A and C ask probability to do a deterministic job; D confuses the safety
backstop with the mechanism.

---

### 2. Config-scope inverse framing

**Objective 3.1 — CLAUDE.md hierarchy.**

**The trap:** the scenario is stated backwards — "works on my machine,
teammate doesn't have it" — and the options make you convert between scopes
under pressure.

**Worked example.** A team's convention ("always run the linter before
committing") lives in one senior engineer's `~/.claude/CLAUDE.md`. A new
hire clones the repo and Claude never applies the rule. What's the correct
fix? (A) Ask the new hire to copy the senior engineer's user-level file to
their own machine. (B) Move the rule into the project's committed
`CLAUDE.md`. (C) Add the rule to each engineer's `CLAUDE.local.md`. (D)
Create a slash command that prints the rule on demand.

**Resolution: B.** Team standards belong at project scope because that is
the tier version control distributes. User scope is personal by design — it
never travels with the repo — and A/C scale the mistake to N machines. D
turns an always-on standard into a manual ritual. The general move: whoever
must have the rule, and however they get the code, determines the tier.

---

### 3. Tool-scoping vs topology attribution

**Objectives 2.3 (tool distribution) and 1.2 (multi-agent orchestration).**

**The trap:** an agent misuses a tool, and the options split between "change
the tool surface" and "change the agent structure". The scenario's numbers
tell you which.

**Worked example.** A billing subagent holds 14 tools, including a
general-purpose `run_query`. It periodically issues raw queries against the
wrong table. What's the highest-leverage fix? (A) Add a coordinator that
pre-approves every query the subagent wants to run. (B) Replace `run_query`
with narrow tools (`get_invoice`, `get_payment_status`) and drop the count
to about five. (C) Move billing into the coordinator's own context so it can
watch the queries directly. (D) Add few-shot examples of correct queries to
the subagent prompt.

**Resolution: B.** Selection accuracy degrades as tool count grows, and a
general-purpose tool inside a specialized agent is scope creep — the fix is
a scoped tool surface. A and C change the topology to supervise a problem
the tool design caused; D nudges probability where a capability boundary was
needed. Rule of thumb: if the misuse is *available to happen*, scope it out
before you supervise it.

---

### 4. Aggregate-vs-stratified review

**Objective 5.5 — review workflows and confidence calibration.**

**The trap:** an impressive aggregate metric, and options that accept it.
The correct answer attacks the measurement.

**Worked example.** An extraction pipeline reports 96% field accuracy across
all documents, so leadership proposes removing human review entirely. What
is the strongest objection? (A) Regulatory expectations always require human
review of automated decisions. (B) The aggregate can hide a weak segment —
stratify accuracy by document type and field before deciding. (C) 96% rounds
to 100%, so the remaining errors are negligible. (D) The model's confidence
scores already identify the errors, making review redundant.

**Resolution: B.** 96% overall can be 99% on typed forms and 78% on
handwritten ones. The question is about the *metric's* construction, and B
answers that; A is an absolute that ignores the measurement question; D
trusts self-reported confidence, which is uncalibrated — a routing key, not
truth.

---

### 5. Schema-honesty: nullable fields and honest enums

**Objective 4.3 — structured output enforcement.**

**The trap:** a required field the source sometimes lacks. The model does
not fail loudly — it fabricates a plausible value to satisfy the schema.

**Worked example.** An invoice extractor must populate `due_date` for every
record, but a third of invoices state no due date. Auditors find the system
invented dates on exactly those records. Which schema change prevents the
fabrication? (A) Make `due_date` nullable with the instruction "return null
if the value is not directly stated in the source." (B) Keep it required but
add "do not hallucinate" to the prompt. (C) Add a post-processing step that
deletes suspicious dates. (D) Retry the extraction with a different seed
until the date looks consistent.

**Resolution: A.** A required non-nullable field forces the model to choose
between fabricating and failing schema validation — and schemas enforce
shape, not honesty. Nullable fields plus an explicit null-when-unstated rule
give the model an honest exit. The same pattern applies to enums: an "other"
bucket with a free-text detail field beats forcing every case into the
nearest label.

---

### 6. Batch-vs-sync split

**Objective 4.5 — batch processing strategies.**

**The trap:** the ~50% discount tempts a "batch everything" answer; the
decider is whether anything blocks on the result.

**Worked example.** A platform team wants to cut LLM costs. Two workloads:
nightly summaries of the day's support tickets, and a pre-merge compliance
check that pull requests wait on. Which should move to the Batches API?
(A) Both — the discount applies either way. (B) Neither — batch results are
less accurate. (C) The nightly summaries only; the pre-merge check must stay
synchronous. (D) The pre-merge check only, because it is higher priority.

**Resolution: C.** Batches trade latency for cost: up to a 24-hour window
and no SLA — fine for an overnight report, disqualifying for a blocking
gate. "Typically finishes in about two hours" is not a guarantee. Priority
inverts the trap: the *more* a caller blocks on the result, the *less*
suitable the batch window.

---

### 7. Context preservation under pressure

**Objective 5.1 — managing conversation context.**

**The trap:** precise facts decay across a long conversation, and options
offer better compression. Compression is the cause, not the cure.

**Worked example.** After thirty turns, a claims agent that once cited
"$1,204.60, approved March 14" now says "the approved amount from
mid-March". The team already summarizes old turns. What actually preserves
the figures? (A) Summarize more aggressively so less text competes for
attention. (B) Pin a case-facts block (claim ID, amount, dates) at the top
of every prompt, outside the summarized history. (C) Switch to a model with
a larger context window. (D) Run `/compact` at a fixed interval.

**Resolution: B.** Summarization is precisely what rounds "$1,204.60" into
"about $1,200 from mid-March" — more of it (A, D) accelerates the decay, and
a bigger window (C) postpones it. Facts that must survive are pinned
verbatim, every turn, outside whatever gets summarized.

---

## Post-exam gap patterns

These seven come from a real sitting's score report and debrief — objectives
where recognition outran understanding. Each gets its own entry because each
is a *named, repeatable* trap shape.

### 8. The three-way workflow decision

**Objective 3.4 — plan mode vs direct execution, *and multi-phase
workflow*.**

**The trap:** the options offer three patterns and the scenario quietly
demands the third. Training that only ever contrasted plan mode with direct
execution leaves "multi-phase workflow" as an unlabeled middle zone.

**Worked example.** A bank wants Claude to modernize a fraud-rules engine:
first map the existing rules across 200 files, then get each proposed rule
translation approved by compliance before it is applied, then verify each
applied change against regression fixtures. Why does neither pure plan mode
nor direct execution fit? (A) Plan mode cannot produce plans for 200 files.
(B) The work is a sequence of discrete phases — mapping, approved
translation, verified application — each needing its own checkpoint or
approval gate before the next begins. (C) Direct execution is always wrong
for enterprise codebases. (D) Multi-phase workflows run faster than either
alternative.

**Resolution: B.** Plan mode is a single plan→approve→execute cycle; this
task's risk is spread across stages that each gate the next, which is the
defining shape of a multi-phase workflow. The selection criteria are task
scope, risk level, and human-approval requirements — read the scenario for
where the approvals live.

---

### 9. Choosing the structured-output rung

**Objective 4.3 — selecting among tool use + schema, prompt-based
formatting, and prefilled responses by required strictness.**

**The trap:** "the output must be JSON" with options spanning the whole
strictness spectrum. Picking too weak a rung breaks the pipeline; picking
the strongest rung "to be safe" can be the over-engineered distractor when
the consumer is a human.

**Worked example.** A reporting job feeds its output to a strict parser, and
the team wants the cheapest change that guarantees the model's reply *begins
as* JSON — no preamble, no code fences — without registering any tools.
Which technique fits? (A) Add "respond only in JSON" to the prompt. (B)
Prefill the assistant turn with `{` so the model continues a JSON document.
(C) Register a tool whose `input_schema` matches the report and force
`tool_choice` to it. (D) Parse whatever comes back and retry on failure.

**Resolution: B.** Prefilling constrains the continuation — the model is
completing a document that already started as JSON — without tool machinery.
A is the weakest rung (an instruction, not a constraint). C is a stronger
rung than the requirement states (nothing said the payload must validate
against a schema, only that it begins as JSON). Match the rung to the
consumer's fragility.

---

### 10. MCP scope, secrets, and discovery as one question

**Objective 2.4 — integrating MCP servers: scope selection, environment
expansion, discovery verification.**

**The trap:** three decisions that are usually drilled separately arrive as
one scenario — where the server definition lives, where the secret lives,
and how you confirm the tools are actually there.

**Worked example.** A team server must reach every developer on clone, but
each developer authenticates with a personal token. After setup, one
developer reports the server's tools missing. Which configuration is
correct, and what is the first diagnostic? (A) Server in `~/.claude.json`
with the token inline; restart the IDE. (B) Server in the committed
`.mcp.json` with `"env": {"TOKEN": "${MCP_TOKEN}"}`; verify what the client
actually discovered via its tool listing. (C) Server in
`.claude/settings.json` with the token in `env`; check the file's YAML.
(D) Server in `.mcp.json` with each developer's token committed per branch;
re-clone.

**Resolution: B.** Shared server → project scope; personal secret →
environment expansion, never committed; missing tools → verify discovery
(what the client listed at connection time) before debugging the server.
A strands the server on one machine and commits-adjacent the secret; C puts
a server definition where settings files never load them; D commits secrets.

---

### 11. Inclusion *and* exclusion criteria

**Objective 4.1 — prompt criteria with explicit boundaries.**

**The trap:** a prompt that says what to find, and options that only
strengthen the positive instruction. The flood comes from the missing
negative boundary.

**Worked example.** A contract-review prompt lists five clause types to
flag. In production the reviewer also flags routine boilerplate — clauses
it scores poorly on — drowning the legal team in false positives. What
change addresses the root cause? (A) Restate the five clause types in
stronger language. (B) Add explicit exclusion criteria naming the
categories not to report, with examples of acceptable instances. (C) Raise
the confidence threshold for reporting. (D) Lower the temperature to reduce
creative flagging.

**Resolution: B.** A model told what to find but not what to skip will
generate findings in categories where its performance is unreliable —
exclusion criteria are how you stop it doing work it shouldn't. A and D
adjust intensity, not scope; C treats self-reported confidence as
calibrated, which it isn't.

---

### 12. Glob finds paths, Grep finds callers

**Objective 5.4 — systematic codebase exploration.**

**The trap:** a change-propagation scenario ("rename this function", "add a
parameter") with a Glob option that *sounds* thorough. Glob can enumerate
every file that might matter and still answer none of "who calls this".

**Worked example.** Before renaming `calculateDiscount` and adding a
`region` parameter, an engineer must enumerate every call site across the
monorepo. Which approach is correct? (A) Glob `**/*.ts` to list all source
files, then read each one in full. (B) Grep for `calculateDiscount` (and its
aliases) across the repo, then Read each matched file around the hit. (C)
Glob `**/calculateDiscount*` to find files named for the function. (D) Ask
the model to recall where the function is used from the files already in
context.

**Resolution: B.** Call sites are *content*, and Glob matches only paths —
C is the pure form of the trap. A works in principle but reads the entire
repo to answer a search question. D trusts recall over search. The
discipline: Grep to enumerate, Read to verify, and only then Edit. (Do it
once by hand in a real repo — the distinction sticks the first time you run
it, not the tenth time you read it.)

---

### 13. Fast scripted startup: `--bare` earns its keep

**Objective 3.6 — CI/CD integration and non-interactive invocation.**

**The trap:** a scripted invocation is slow, and the options offer bigger
machines or caching. The cost is auto-discovery — and you skip it only
because you're supplying the prompt content yourself.

**Worked example.** A nightly job runs `claude -p` 400 times with the same
fixed corporate-standards prompt, and each invocation spends noticeable time
discovering skills, commands, and settings it will never use. What is the
correct fix? (A) Run the job on a larger runner. (B) Add `--bare` and inject
the standards text with `--system-prompt-file` (or `--append-system-prompt`).
(C) Cache the first run's output and replay it. (D) Set `CLAUDE_HEADLESS=1`
to skip discovery.

**Resolution: B.** The discovery pass exists to assemble context the prompt
needs; when the script already knows that content, `--bare` skips discovery
and the file/append flags supply the text directly. D names a variable that
does not exist. A and C treat the symptom. The underlying examinable idea:
*know what you are paying for at startup, and what replaces it when you skip
it.*

---

### 14. Why resources are faster — the round-trip mechanism

**Objective 2.4 — MCP integration: resources vs tools.**

**The trap:** "expose it as a resource" is easy to recognize as the answer
and easy to choose for the wrong reason ("resources are cached" / "the
server responds faster"). Options in this family test whether you know the
mechanism.

**Worked example.** An agent's data-gathering phase makes a dozen
exploratory tool calls to fetch reference schemas, and the phase is slow.
A colleague says converting those lookups to MCP resource reads speeds it up
"because resources are pre-fetched at server startup". Is the reasoning
right? (A) Yes — pre-fetching is where the speed comes from. (B) No — the
win is that a resource is read directly by the client with no model turn,
while each tool call costs a full model round-trip (request, execution,
result, another reasoning pass). (C) No — resources are slower but more
reliable. (D) Yes — and additionally resources bypass the context window.

**Resolution: B.** Every tool call is a model reasoning round-trip; a
resource read happens client-side with no model turn at all. Removing
exploratory tool calls removes turns, and the turns are the latency. If you
chose the right answer on a pattern match ("resources feel efficient"), the
test is whether you can produce this sentence unprompted — a correct option
chosen without the mechanism is luck that doesn't repeat.

---

*Cross-reference: the decision rules behind several of these are condensed
in `practice/detailed-cheatsheet.md`; per-domain depth is in the five
`guide/d1–d5` pages.*
