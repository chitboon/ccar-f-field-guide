# Domain 5 — Context Management & Reliability (15%, ~9 items)

Small domain, concentrated points. The theme: information degrades unless
you engineer its survival — through context windows, across agent
boundaries, and into human hands.

## Objective map (paraphrased from the published exam guide)

- **5.1 Context preservation** — keeping critical information intact over
  long conversations.
- **5.2 Escalation and ambiguity** — when a system must hand off to a human,
  and when it must not.
- **5.3 Error propagation** — how failures travel across a multi-agent
  system without being lost or misread.
- **5.4 Large-codebase context** — building understanding incrementally
  without exhausting the window.
- **5.5 Human review and confidence calibration** — routing work to
  reviewers on evidence, not vibes.
- **5.6 Provenance and uncertainty** — claims carry their sources; "could
  not verify" is a first-class outcome.

## Mental models

**Placement and pinning beat compression.** Models attend hardest to the top
and bottom of long context (the lost-in-the-middle effect): put key findings
first, mark detail with explicit section headers, and use map-reduce for
long synthesis — per-source detail first, reconcile second. Facts that must
survive (account IDs, amounts, dates, order numbers) go in a **pinned
case-facts block at the top of every prompt**, outside whatever narrative
summary exists — summarization is what *causes* the precision loss, so
"compact better" cannot fix it. Summarize resolved turns into one-line
narrative; keep verbatim history only for the active issue. Prune verbose
tool outputs application-side before appending (the PostToolUse pattern:
returned 40 fields, you use 5, strip 35). `/compact` is a fallback, not a
strategy. When raw upstream volume is what chokes synthesis, fix it at the
source: upstream agents return structured summaries instead of payloads.

**Prompt caching mechanics.** `cache_control` ephemeral breakpoints on the
last stable block; four explicit breakpoints max; writes cost 1.25×, reads
0.1×, five-minute default TTL. The silent trap: a **minimum cacheable size
(1,024 tokens on Sonnet-class, 4,096 on Haiku-class)** — below the floor,
caching is ignored with no warning and counters read zero, which looks like
a hard miss. Verify with `cache_creation_input_tokens` then
`cache_read_input_tokens`.

**Escalation has exactly three legitimate triggers.** (1) An **explicit
request** for a human — honoured immediately, no clarifying question, no
one-more-tool-call. (2) A **policy gap** — policy is silent or the
resolution exceeds a cap. (3) **No meaningful progress** — paths exhausted.
Not triggers: customer sentiment (frustration ≠ complexity — route on the
problem's shape), self-reported confidence (uncalibrated), turn counts.
Hand off a structured summary — who, what, what's been tried, what's blocked
— never the raw transcript. Ambiguous identity (three matching accounts)
means ask for another identifier, never heuristic-pick.

**Errors propagate as errors.** Subagents return failure type + what was
attempted + partial results + alternatives; the coordinator owns recovery
policy, and local recovery comes first. Anti-patterns: swallowed exceptions,
empty-success error reporting, terminating the whole workflow on one
subagent's failure, and burying `is_error` inside "success with metadata".

**Review routing runs on evidence, not randomness.** Route extractions to
reviewers by confidence scores, **document characteristics, and field-level
ambiguity** — a low-confidence total on a handwritten form with an ambiguous
currency field is a review candidate even when the queue is otherwise clean.
Self-reported confidence is a routing key, not truth: calibrate thresholds
against a labeled validation set, segment accuracy by document type *and*
field, and stratified-sample the auto-approved slice to catch
high-confidence fabrication and drift. Aggregate accuracy is the trap
metric — 97% overall can hide 80% on one segment.

**Provenance is structural.** Every claim carries `{claim, evidence excerpt,
source, date}` — no source, no claim; dates let staleness be flagged.
Conflicting credible sources: **annotate, don't arbitrate** — averaging
invents data and heuristic-picking buries the conflict. "Could not verify"
is a structurally distinct status from "verified fine", never a silent
omission.

**Drill:** `concept-drills/d5-concept-drill.md`
**Traps:** aggregate-vs-stratified review; context-preservation-under-
pressure (`guide/trap-patterns.md`)
