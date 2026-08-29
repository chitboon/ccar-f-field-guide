# CCAR-F Concept Drill — Domain 1: key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. A** — Parsing prose for 'done' ignores the tool_use stop_reason, so the loop can terminate with pending tool results. The authoritative continuation signal is stop_reason, not assistant text; the other options describe unrelated failures. *(task 1.1; concept: stop_reason_vs_prose; item `cc-07`)*

---

**2. C** — A pre-call hook gated on an unforgeable approval flag enforces the rule deterministically. More examples and temperature zero are still probabilistic; post-call reversal is remediation, not prevention. (Single-answer conversion of the two-control original: hook interception and the unforgeable flag are one mechanism here.) *(task 1.5; concept: programmatic_gate_vs_prompt; item `cc-15s`)*

---

**3. B** — Resuming preserves valid context while targeting re-analysis at the changed files. Starting over wastes prior work; forking creates parallel branches rather than continuing the original analysis; a summary lacks fresh file contents. *(task 1.7; concept: fork_vs_resume; item `cc-01`)*

---

**4. D** — Subagents run with isolated context and do not inherit parent history, so required facts must be placed explicitly in the subagent prompt. A missing Task tool would prevent delegation entirely; stop_reason and context-window size are unrelated mechanisms. *(task 1.3; concept: subagent_context_isolation; item `cc-03`)*

---

**5. A** — When the steps are known in advance and identical across inputs, a fixed sequential chain is simpler and more predictable than adaptive decomposition. Dynamic planning pays off for open-ended investigation; one combined pass dilutes attention across files. *(task 1.6; concept: fixed_chain_vs_dynamic_decomposition; item `d1-n1`)*

---

**6. B** — Resumption preserves the valid prior context while targeting the changed files. Forking would compare baselines rather than update verdicts; a fresh start wastes prior work; ignoring failures is unsafe. *(task 1.7; concept: fork_vs_resume; item `cc-11`)*

---

**7. D** — Subagents run isolated and need explicit handoff context — the country must be repeated in each subagent prompt. A missing Task tool prevents spawning rather than causing repeated questions; window exhaustion and shared context are the wrong mechanisms. *(task 1.2; concept: subagent_context_isolation; item `cc-12`)*

---

**8. C** — Forking creates independent branches from a shared baseline, preserving the original session. Resuming applies strategies serially to one branch; a fresh start re-pays the exploration cost; copying directories is not the session mechanism. *(task 1.7; concept: fork_vs_resume; item `cc-02`)*

---

**9. A** — The three subtasks are all refrigeration-flavored road topics; air freight was never assigned to anyone. Subagents can only execute the scope they are given, so blame the decomposition, not the downstream agents, which performed correctly. *(task 1.2; concept: narrow_decomposition_blind_spots; item `d1-n2`)*

---

**10. B** — Programmatic gating enforces the ordering before execution; prompt instructions are probabilistic and have a non-zero failure rate exactly when it matters. More examples, temperature changes, and self-review all lack deterministic enforcement. *(task 1.4; concept: programmatic_gate_vs_prompt; item `cc-06`)*

---

**11. C** — A PostToolUse hook normalizes heterogeneous tool output deterministically, before the model reasons over it. Prompt warnings and per-comparison conversion are probabilistic; waiting on three maintainers leaves the agent broken meanwhile. *(task 1.5; concept: hook_data_normalization; item `d1-n3`)*

---

**12. D** — The authoritative signal is stop_reason: tool_use means the model is awaiting tool results and the loop must continue. Waiting, higher iteration caps, and repeated sentinel strings are workarounds that never address the root cause. *(task 1.1; concept: stop_reason_vs_prose; item `cc-16`)*

---
