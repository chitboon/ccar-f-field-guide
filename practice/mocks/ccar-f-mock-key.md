# CCAR-F Mock — key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. D** — stop_reason tool_use means the model issued tool_use blocks and is waiting for results, so the loop must execute the tools and call the model again. Parsing prose for completion cues is unreliable — the phrase 'done' does not override the authoritative stop_reason signal. Treating prose as complete ignores pending tools, and returning to the user breaks the loop. *(task 1.1; concept: stop_reason_vs_prose; item `mock-d1-01`)*

---

**2. B** — stop_reason end_turn means the model has finished its response and is not waiting for tool results. The authoritative signal is stop_reason, not the prose content. Mentioning 'next steps' in analysis does not override end_turn. Re-prompting wastes a turn, and executing tools without tool_use blocks is undefined behavior. *(task 1.1; concept: stop_reason_vs_prose; item `mock-d1-02`)*

---

**3. A** — Subagents run with isolated context and do not inherit the parent's conversation history; needed facts must be passed explicitly in the subagent prompt. A missing Task tool would prevent spawning rather than causing repeated questions; window exhaustion and stop_reason are unrelated mechanisms. *(task 1.2; concept: subagent_context_isolation; item `mock-d1-03`)*

---

**4. A** — Subagents run with isolated context and do not inherit the parent's conversation history; needed facts like shipment IDs must be passed explicitly in the subagent prompt. A missing Task tool would prevent reporting, not cause repeated questions. Context window exhaustion and stop_reason confusion are unrelated mechanisms. *(task 1.2; concept: subagent_context_isolation; item `mock-d1-04`)*

---

**5. C** — Subagents run with isolated context and do not inherit the parent's conversation history. The subagent has no knowledge of the 20 prior messages unless explicitly passed in its launch prompt. Temperature, context window, and independent verification are not the root cause. *(task 1.3; concept: subagent_context_isolation; item `mock-d1-05`)*

---

**6. B** — Subagents run in isolation and report back through the Task tool. They do not automatically share findings with each other, the coordinator cannot read their internal chain-of-thought, and writing to shared files is not the standard communication mechanism. *(task 1.3; concept: subagent_context_isolation; item `mock-d1-06`)*

---

**7. A** — A programmatic gate enforces the rule deterministically before execution, independent of prompt wording. More detailed prompts and temperature zero are still probabilistic and can be talked past. Post-call reversal is remediation, not prevention. *(task 1.4; concept: programmatic_gate_vs_prompt; item `mock-d1-07`)*

---

**8. C** — Hard-coded gates evaluate deterministic conditions before a tool runs, so reworded prompts cannot override them. Prompts are softer controls that can be talked past; server-side storage does not make prompts immune to manipulation; gates do not primarily provide explanations. *(task 1.4; concept: programmatic_gate_vs_prompt; item `mock-d1-08`)*

---

**9. C** — A pre-call hook that checks the amount deterministically prevents the tool call from executing when the threshold is exceeded. More prompt examples, temperature zero, and post-call audits are all probabilistic or remediation-only controls. *(task 1.4; concept: programmatic_gate_vs_prompt; item `mock-d1-09`)*

---

**10. A** — A pre-call hook runs deterministic code to normalize or validate input immediately before the tool executes. It is not probabilistic, it is not post-call validation, and it does not decompose tasks. *(task 1.5; concept: hook_data_normalization; item `mock-d1-10`)*

---

**11. B** — A PostToolUse hook normalizes heterogeneous tool output deterministically, before the model reasons over it. It does not affect the model's own output format, the latency is negligible, and the model still needs to understand dates for its reasoning. *(task 1.5; concept: hook_data_normalization; item `mock-d1-11`)*

---

**12. A** — The hook resolves the structural ambiguity deterministically, so the model correctly distinguishes warnings from errors. Token reduction and speed are incidental benefits, and hooks do not provide rule-modification capabilities. *(task 1.5; concept: hook_data_normalization; item `mock-d1-12`)*

---

**13. D** — Dynamic decomposition generates subtasks from newly discovered information, which matches a cascading failure where each fix reveals the next problem. A fixed chain assumes all steps are known in advance, per-file subagents lose cross-file context, and session resumption preserves context but does not create new tasks. *(task 1.6; concept: dynamic_decomposition; item `mock-d1-13`)*

---

**14. A** — When the steps are known in advance and identical across inputs, a fixed sequential chain is simpler and more predictable. Dynamic planning pays off for open-ended investigation, per-file subagents lose cross-file context, and one combined pass dilutes attention. *(task 1.6; concept: dynamic_decomposition; item `mock-d1-14`)*

---

**15. B** — Named session resumption continues the existing conversation with preserved context. Starting fresh loses prior context, forking creates an independent branch rather than continuing the original analysis, and /compact only shortens history rather than continuing it. *(task 1.7; concept: fork_vs_resume; item `mock-d1-15`)*

---

**16. D** — Forking creates independent branches from the baseline while leaving the original untouched, enabling parallel evaluation against the same findings. Resuming applies patches serially to one branch, fresh sessions discard the baseline, and /compact only trims history. *(task 1.7; concept: fork_vs_resume; item `mock-d1-16`)*

---

**17. C** — Parameter descriptions are the model's only source of the constraints and applicability signals it needs to reason about what a valid input looks like. Latency, documentation generation, and payload validation are separate, unrelated concerns. *(task 2.1; concept: tool_description_quality; item `mock-d2-01`)*

---

**18. B** — Concrete examples fix the root cause by clarifying the model's reasoning about what the parameter accepts. Temperature, confirmation steps, and renaming all address symptoms rather than the ambiguity itself. *(task 2.1; concept: tool_description_quality; item `mock-d2-02`)*

---

**19. B** — Structured error codes signal the specific failure mode, letting the model choose a targeted recovery action. Payload size, execution speed, and logging quality are unrelated to why the signal is useful. *(task 2.2; concept: structured_error_signal; item `mock-d2-03`)*

---

**20. D** — The two codes signal different root causes: RATE_LIMIT is transient and worth retrying after the indicated delay, while AUTH_EXPIRED is a credential refresh issue that retrying without refreshing cannot resolve. Treating them identically wastes retries or escalates unnecessarily. *(task 2.2; concept: structured_error_signal; item `mock-d2-04`)*

---

**21. C** — Removing the tool from the registry is a deterministic, structural control. Prompt warnings, temperature, and confirmation dialogs are all still probabilistic and can be talked around. *(task 2.3; concept: tool_choice_vs_scoping; item `mock-d2-05`)*

---

**22. A** — Structurally removing adjust_dosage is the only approach that holds regardless of phrasing. More detailed warnings, post-hoc logging, and self-confirmation are all prompt-level controls the model could be talked past. *(task 2.3; concept: tool_choice_vs_scoping; item `mock-d2-06`)*

---

**23. B** — Removing the tool from the registry is a deterministic structural control that the model cannot bypass. Prompt instructions, post-call reversal, and temperature changes are all probabilistic or remediation-only. *(task 2.3; concept: tool_choice_vs_scoping; item `mock-d2-07`)*

---

**24. B** — Scoping to what the task needs keeps the tool surface small and avoids tool-selection confusion. Exposing everything adds noise, naming-only is too narrow to be a general rule, and rotation serves no discrimination purpose. *(task 2.4; concept: mcp_scope_selection; item `mock-d2-08`)*

---

**25. D** — The three servers that map directly to the task's needs are patient records, pharmacy data, and medical reference. Every-server access includes billing modification capability the agent should not have, and either single-purpose option alone leaves a genuine capability gap. *(task 2.4; concept: mcp_scope_selection; item `mock-d2-09`)*

---

**26. D** — Built-in Grep covers the full repository with pattern matching, which is exactly what finding every call site requires. The custom script's partial index misses the directories outside /src/services/. The blanket 'never' and the file-size or text-only restrictions aren't real limitations of the built-in tool. *(task 2.5; concept: builtin_tool_selection; item `mock-d2-10`)*

---

**27. C** — Built-in tools handle generic file operations well; YAML schema validation is domain-specific and belongs in a custom tool. All-custom and all-built-in both misplace domain logic, and skipping steps leaves the migration incomplete. *(task 2.5; concept: builtin_tool_selection; item `mock-d2-11`)*

---

**28. C** — A project-level CLAUDE.md at the repository root travels with the codebase and applies to everyone who clones it. Options A and B are user-level locations that only configure the local machine. Option D places a personal file inside the project scope, which contradicts the goal of shared, version-controlled rules. *(task 3.1; concept: config_scope; item `mock-d3-01`)*

---

**29. A** — @import lets a project-level CLAUDE.md assemble shared standards from a common source while remaining version-controlled with the repo. Options A and D create drift-prone copies or out-of-band documentation. Option B ties the standards to one user's machine, so teammates do not receive them. *(task 3.1; concept: config_scope; item `mock-d3-02`)*

---

**30. C** — Project-specific commands belong in .claude/commands/ at the repository root so they are committed and shared. Options A and B are user-level directories for personal tooling. Option D is a system path unrelated to Claude's command discovery. *(task 3.2; concept: commands_and_skills; item `mock-d3-03`)*

---

**31. B** — allowed-tools restricts which tools the command may invoke, which is exactly what limiting to Read and Grep achieves. context: fork runs in isolation but does not restrict tools. argument-hint prompts for input. description is metadata. *(task 3.2; concept: commands_and_skills; item `mock-d3-04`)*

---

**32. C** — Separate .claude/rules/*.md files can use paths: frontmatter globs to apply conventions by file pattern. A single CLAUDE.md cannot distinguish which rule applies to which path. README files are not automatically treated as rules. Settings.json maps users, not files. *(task 3.3; concept: path_scoped_rules; item `mock-d3-05`)*

---

**33. D** — Separate rules files with paths: globs apply the correct convention to each directory automatically. A single CLAUDE.md cannot distinguish which rule applies to which path. A user-level rule applies globally, not per-directory. Deleting the legacy directory is not a configuration choice. *(task 3.3; concept: path_scoped_rules; item `mock-d3-06`)*

---

**34. A** — Plan mode is designed for large architectural decisions with competing approaches before execution begins. Editing immediately invites rework, CI jobs do not facilitate deliberation, and a personal note keeps the plan on one machine. *(task 3.4; concept: plan_mode; item `mock-d3-07`)*

---

**35. D** — A single, well-scoped change with an obvious fix does not need plan mode's exploration overhead. Plan mode, a repository-wide search, and forcing a strategy choice are all disproportionate to a change this small and clear. *(task 3.4; concept: plan_mode; item `mock-d3-08`)*

---

**36. C** — This illustrates iterative refinement: each loop is tied to a concrete failing test and one controlled change. Batching would obscure which fix mattered, plan mode is for up-front choices, and CI execution is tooling, not methodology. *(task 3.5; concept: iterative_refinement; item `mock-d3-09`)*

---

**37. B** — Iterative refinement depends on concrete failing tests as measurable signals. Option B establishes the signal and closes the loop before the next change. Memory and prose are vague, and batching obscures which change resolved the flake. *(task 3.5; concept: iterative_refinement; item `mock-d3-10`)*

---

**38. B** — The -p/--print flag runs Claude Code headlessly and emits output to stdout, which is suitable for CI. Option A enables the interactive UI that CI cannot drive. Options C and D affect logging detail or color but not headless execution. *(task 3.6; concept: ci_integration; item `mock-d3-11`)*

---

**39. D** — A fresh container with the project's CLAUDE.md provides isolated, reproducible context and applies shared standards. Option A relies on personal configuration that teammates may not share. Option C removes the standards file. Option B reviews the wrong code state. *(task 3.6; concept: ci_integration; item `mock-d3-12`)*

---

**40. C** — Domain 4.1 emphasizes replacing vague qualifiers with explicit categorical criteria. Option C converts 'significant' into concrete clinical severity categories. Option A keeps vagueness, option D lacks explicit bounds, and option B sets an arbitrarily narrow scope that would miss non-dosage safety signals. *(task 4.1; concept: explicit_criteria; item `mock-d4-01`)*

---

**41. C** — Explicit categorical criteria with required evidence fields and quantitative thresholds eliminate ambiguity in what counts as 'high-risk.' Option A preserves subjectivity, option B avoids classification entirely, and option D narrows scope using an unstated proxy rather than risk criteria. *(task 4.1; concept: explicit_criteria; item `mock-d4-02`)*

---

**42. D** — Few-shot examples demonstrating desired edge-case handling are more effective than additional rules alone. Option D supplies concrete pattern references for the exact ambiguity that rules cannot resolve. Multiplying rules adds brittleness, truncating loses context, and removing rules removes guidance. *(task 4.2; concept: few_shot; item `mock-d4-03`)*

---

**43. A** — Few-shot examples that demonstrate reasoning for ambiguous cases directly show the desired pattern, which prose alone often fails to convey consistently. More prose, lower temperature, and splitting the prompt do not address the actual gap between instruction and consistent behavior. *(task 4.2; concept: few_shot; item `mock-d4-04`)*

---

**44. B** — A clean schema needs clear required and nullable distinctions plus enums for closed sets. Option A adds parse-breaking wrappers, option C makes output non-deterministic, and option D defeats the purpose of structured output. *(task 4.3; concept: schema_design; item `mock-d4-05`)*

---

**45. B** — An enum enforces a closed set of valid values and ISO-8601 provides machine-parseable timestamps, which the downstream parser can categorize reliably. Option A introduces markdown wrappers that break parsing, option C removes type constraints, and option D chooses a non-JSON representation. *(task 4.3; concept: schema_design; item `mock-d4-06`)*

---

**46. D** — Making the field nullable removes the pressure to satisfy a required field when the source data genuinely does not exist. A longer description, more context, or a different type do not remove the incentive to fabricate a value where none exists. *(task 4.3; concept: schema_design; item `mock-d4-07`)*

---

**47. C** — Retry is appropriate when the source contains the information but the model failed structurally due to a confounding element. Option C addresses the structural failure by directing focus to the readable region. Higher temperature randomizes, halting is wrong because the data exists, and removing pages destroys medical records. *(task 4.4; concept: retry_validation; item `mock-d4-08`)*

---

**48. B** — Passing the source PDF, the malformed output, and the exact arithmetic mismatch lets the model target the actual mistake on retry. An unrelated document, a vague instruction, or lowering the confidence threshold do not address the specific error found. *(task 4.4; concept: retry_validation; item `mock-d4-09`)*

---

**49. A** — Batch API suits latency-tolerant bulk jobs like scanning millions of emails, while synchronous API suits real-time gates needing immediate feedback. Synchronous for the archive wastes resources, batch for real-time is too slow, and reversing the pairing is backwards. *(task 4.5; concept: batch_vs_sync; item `mock-d4-10`)*

---

**50. B** — Independent review by a separate instance catches subtle defects that self-review misses due to confirmation bias. Adding more instructions to the same instance repeats the bias, removing the checklist eliminates review, and temperature changes generation randomness rather than verification quality. *(task 4.6; concept: self_review_vs_independent; item `mock-d4-11`)*

---

**51. A** — Independent review with evidence-based justification overcomes the confirmation bias inherent in self-review. Higher temperature adds randomness without independent scrutiny, reducing weakens coverage, and training the generator preserves the same blind spot. *(task 4.6; concept: self_review_vs_independent; item `mock-d4-12`)*

---

**52. A** — Structured facts support precise, repeated lookups without re-parsing prose each time. Ease of reading, brevity, and transcript completeness are all secondary to the need for exact, reusable values during a long investigation. *(task 5.1; concept: structured_facts_vs_summarization; item `mock-d5-01`)*

---

**53. C** — A clarifying question resolves the ambiguity directly. Guessing risks misalignment with intent, escalating skips a step the model can handle itself, and an error is unhelpful when the request is answerable. *(task 5.2; concept: escalation_ambiguity; item `mock-d5-02`)*

---

**54. D** — An explicit customer request for a human should be honored immediately rather than overridden with an investigation the customer did not ask for. Trying to resolve it first, arguing against the request, and confidence-score gating all override a plainly stated preference. *(task 5.2; concept: escalation_ambiguity; item `mock-d5-03`)*

---

**55. D** — A structured error lets the coordinator understand the impact and decide how to proceed. Silently continuing hides the problem, a raw trace is hard for the coordinator to parse, and a vague note carries no actionable information. *(task 5.3; concept: error_propagation_multiagent; item `mock-d5-04`)*

---

**56. A** — The two failures need different handling — rate-limit is transient and retryable, permission is persistent and needs a config fix. Collapsing to binary success/failure loses this distinction, and escalating everything skips summarization the coordinator can do. *(task 5.3; concept: error_propagation_multiagent; item `mock-d5-05`)*

---

**57. C** — Targeted search for entry points and structure gives fast orientation without wasting effort. Reading everything is inefficient at scale, deferring entirely to the user skips work the model can do itself, and modifying before understanding risks breaking something unseen. *(task 5.4; concept: codebase_exploration; item `mock-d5-06`)*

---

**58. A** — Searching for the dominant pattern in the target directory grounds the change in evidence already present in the codebase. Asking the user first, picking a style by appearance, and rewriting everything before investigating all skip the available self-service search. *(task 5.4; concept: codebase_exploration; item `mock-d5-07`)*

---

**59. D** — An aggregate figure can mask uneven performance across segments, so stratified breakdowns reveal whether specific areas are weaker. Stability over time, comparison to another tool, and matching a prior report do not surface hidden segment-level variation. *(task 5.5; concept: aggregate_vs_stratified; item `mock-d5-08`)*

---

**60. C** — Provenance is what makes a claim independently checkable and prevents unverifiable statements from being passed off as established fact. Sounding authoritative, added length, and formatting convention are all surface effects, not the actual reason provenance matters. *(task 5.6; concept: claim_source_provenance; item `mock-d5-09`)*

---
