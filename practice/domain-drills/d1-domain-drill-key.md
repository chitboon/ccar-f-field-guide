# CCAR-F Domain Drill — Domain 1: key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. A** — `stop_reason` `tool_use` means the model issued tool_use blocks and is waiting for results, so the loop must execute the tools and call the model again. Parsing prose for completion cues is unreliable, treating prose as complete ignores pending tools, and returning to the user breaks the loop. *(task 1.1; concept: stop_reason_vs_prose; item `d1d-01`)*

---

**2. B** — Subagents run with isolated context and do not inherit the parent's conversation history; needed facts must be placed explicitly in the subagent prompt. A missing Task tool would prevent spawning rather than causing repeated questions; window exhaustion and stop_reason are unrelated mechanisms. *(task 1.2; concept: subagent_context_isolation; item `d1d-02`)*

---

**3. C** — Delegation should use a focused work order that includes only the context the subagent needs, not the whole transcript. Sharing the entire transcript risks information overload, querying the parent violates isolation, and copying the system prompt does not transfer conversation facts. *(task 1.3; concept: subagent_context_isolation; item `d1d-03`)*

---

**4. A** — A programmatic gate enforces the rule deterministically before execution, independent of prompt wording. A system-prompt reminder is only probabilistic, lowering temperature does not guarantee policy, and natural-language override defeats the control. *(task 1.4; concept: programmatic_gate_vs_prompt; item `d1d-04`)*

---

**5. C** — Hard-coded gates evaluate deterministic conditions before a tool runs, so reworded prompts cannot override them. Prompts are softer controls that can be talked past; server-side storage does not make prompts immune to manipulation; gates do not primarily provide explanations. *(task 1.4; concept: programmatic_gate_vs_prompt; item `d1d-05`)*

---

**6. A** — A pre-call hook runs deterministic code to normalize or validate input immediately before the tool executes. It is not probabilistic, it is not post-call validation, and it does not decompose tasks. *(task 1.5; concept: hook_data_normalization; item `d1d-06`)*

---

**7. B** — Dynamic decomposition generates subtasks from newly discovered information, which matches an investigation where hypotheses emerge as logs are analyzed. Prompt chaining assumes all steps are known in advance, session resumption preserves context but does not create tasks, and forking is about branching sessions rather than decomposing work. *(task 1.6; concept: dynamic_decomposition; item `d1d-07`)*

---

**8. B** — The bot decides next steps based on what it discovers, which is dynamic decomposition. A fixed chain would not adapt, a gate blocks rather than decomposes, and context isolation describes visibility rather than step selection. *(task 1.6; concept: dynamic_decomposition; item `d1d-08`)*

---

**9. C** — Named session resumption continues the existing conversation with preserved context. Forking creates an independent branch, starting fresh loses prior context, and /compact only shortens history rather than continuing it. *(task 1.7; concept: fork_vs_resume; item `d1d-09`)*

---

**10. B** — Forking creates independent branches from the baseline while leaving the original untouched, and each fork can be explored separately for later comparison. Resuming would continue the original session rather than isolate the experiments, fresh sessions would discard the shared baseline context, and /compact only trims history. *(task 1.7; concept: fork_vs_resume; item `d1d-10`)*

---
