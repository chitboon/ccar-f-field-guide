# Domain 4 — Prompt Engineering & Structured Output (20%, ~12 items)

The extraction-and-precision domain. Its spine: reliability comes from
mechanisms (schemas, forced tool calls, prefills), not from adjectives
("be accurate", "only return JSON").

## Objective map (paraphrased from the published exam guide)

- **4.1 Explicit criteria** — prompts that define what is in and out of
  scope, so the model doesn't produce findings in categories where its
  performance is unreliable.
- **4.2 Few-shot prompting** — examples as the strongest format lever.
- **4.3 Structured output enforcement** — tool use with JSON schemas, and
  choosing the right reliability level for the job.
- **4.4 Validation, retry, and feedback loops** — which failures retries
  fix and which they can't.
- **4.5 Batch processing** — when the Batches API is the right trade.
- **4.6 Multi-instance review** — independent passes and how to combine
  them.

## Mental models

**The JSON strictness spectrum — pick by how bad a malformed response is.**
From weakest to strongest guarantee:

1. **Prompt-only formatting** ("return only JSON") — no guarantee at all;
   fine for human-read output, wrong for pipelines.
2. **Prefilled assistant turn** — start the assistant's response with `{`
   or the expected prefix. The model is now *continuing* a JSON document
   rather than choosing to produce one, which constrains the continuation
   without any tool machinery. Strong against prose preamble and markdown
   fences; still no schema validation.
3. **Tool use + JSON schema** — register the schema as a tool's
   `input_schema`; output is schema-conformant by construction.
4. **Tool use + schema + forced `tool_choice`** — the strictest: the model
   *must* call that specific tool, so you get a schema-valid payload every
   time, with no "answered in prose instead" cases.

Decision rule: how downstream-fragile is the consumer? Human eyes → 1–2.
A parser → 3. A parser plus a hard requirement that *some* structured call
happen on every turn → 4. The canonical pipeline pattern: Pydantic model →
`model_json_schema()` → tool `input_schema` →
`tool_choice = {"type":"tool","name":...}`.

**`tool_choice` has four modes — know exactly what each guarantees.**
`auto` (default): tool call *or* prose. `any`: must call *some* tool — a
call is guaranteed, the right tool is not. `{"type":"tool","name":X}`: must
call *that* tool — the structured-output cheat code. `none`: tools are
registered but uncallable. Related pattern for enforced call ordering: force
a named tool on every call and let an orchestrator layer decide *which* name
each turn gets — ordering becomes a programmatic guarantee, not a prompt
request. `disable_parallel_tool_use: true` when only one call per turn is
safe.

**Strict kills syntax errors, not semantic errors.** `{"total": 100,
"line_items": [50, 30]}` is schema-valid and arithmetically wrong. Validate
sums, ranges, and cross-field rules application-side.

**Inclusion *and* exclusion boundaries.** Explicit criteria means both
directions: what to report *and* what never to report. A model told what to
find but not what to skip will generate findings in categories where its
performance is unreliable — the fix is criteria that name out-of-scope
classes ("do not flag X, Y, Z") plus examples of each side. When one
category floods with false positives (a security review flagging 60% of
PRs), disable that category temporarily while its criteria are rewritten
with reportable-vs-acceptable examples; self-reported confidence thresholds
do not repair a broken category. And when a required field is absent from
the source, the model fabricates a plausible value to satisfy the schema —
the defence is nullable fields plus "return null if not directly stated",
plus grounding fields (`source_location`) that make fabrication catchable.

**Retries need information.** Two failure classes, two treatments:
format/schema errors → retry with the original document, the failed output,
and the specific `ValidationError` appended (succeeds within a couple of
tries). Missing-information errors → retries don't help; the data isn't in
the source. Hard ceiling of 1–2 retries, then a human: same prompt, same
failure. A `detected_pattern` label on each finding turns a stream of
outputs into records you can group and count — that's what makes feedback
systematic rather than anecdotal.

**Few-shot beats prose.** Two or three input/output examples lock formats
prose can't reach (decimal commas, date layouts, "what not to flag") and
beat temperature adjustments, which change randomness, not understanding.
Many rule sets in one pass cause attention dilution — split into passes or
add a deterministic validation layer.

**Batches API:** roughly half price, up to a 24-hour window, no latency SLA,
`custom_id` correlation, no multi-turn tool calling. Right for overnight
reports; wrong for a blocking pre-merge gate. On partial failure, resubmit
only the failed `custom_id`s.

**Generator ≠ reviewer.** A session reviewing its own output still holds
the reasoning that produced the defects — use an independent instance with
only the diff and project context. Multi-pass review: 2–3 independent passes
plus a judge; agreement automates, disagreement goes to a human. The
anti-pattern is flagging only issues found by ≥2 passes — that suppresses
uniquely-caught real bugs.

**Drill:** `concept-drills/d4-concept-drill.md`
**Traps:** schema-honesty nullable/enum pattern; batch-vs-sync split
(`guide/trap-patterns.md`)
