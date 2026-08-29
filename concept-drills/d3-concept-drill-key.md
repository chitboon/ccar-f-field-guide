# CCAR-F Concept Drill — Domain 3: Claude Code Configuration & Workflows: key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. C** — Instructions in ~/.claude/CLAUDE.md are personal to that user; the rest of the team must be applying a project-level file the hire lacks, or one that lacks the checklist. The fix is moving shared standards into the project CLAUDE.md, which travels with the repository. *(task 3.1; concept: config_scope_sharing; item `d3-01`)*

---

**2. A** — Direct execution suits simple, well-scoped changes with a known fix location. The other three involve architectural decisions, multiple valid approaches, or multi-file blast radius — exactly the conditions plan mode exists to de-risk before changes are made. *(task 3.4; concept: plan_mode_selection; item `d3-08`)*

---

**3. D** — The deciding feature is the hard stop *between* environments: multi-phase workflows are built from discrete phases with checkpoints and approvals, while plan mode is a single plan-then-approve-then-execute cycle with no mid-execution gate. Direct execution and autonomous loops provide no approval structure at all. *(task 3.4; concept: three_way_mode_selection; item `d3-09`)*

---

**4. B** — Project-scoped commands live in .claude/commands/ and are version-controlled with the repo. The user-level directory is personal; CLAUDE.md carries instructions, not command definitions; there is no commands array in the config file. *(task 3.2; concept: project_vs_user_commands; item `d3-04`)*

---

**5. C** — Path-scoped rules apply conventions by file pattern regardless of directory location — the precise requirement when matching files are spread across the tree. Directory CLAUDE.md files mean 40 copies to maintain, a root section relies on inference, and a skill requires someone to remember to invoke it. *(task 3.3; concept: path_glob_vs_directory_config; item `d3-07`)*

---

**6. D** — --bare skips the skill/command/settings discovery that dominates startup cost, and the system-prompt flags supply the required prompt content directly, without paying discovery to produce it. The other flags change output format or session selection, not startup discovery. *(task 3.6; concept: bare_startup_with_injected_prompt; item `d3-12`)*

---

**7. A** — context: fork executes the skill inside an isolated sub-agent, so verbose exploration stays out of the main conversation. allowed-tools restricts which tools run, not where output lands; argument-hint shapes inputs; user scope changes ownership, not isolation. *(task 3.2; concept: context_fork_isolation; item `d3-05`)*

---

**8. B** — Concrete input/output examples communicate the expected transformation more reliably than further prose refinement — examples pin the interpretation where prose leaves room. Temperature and restatement address variance in wording, not in understanding. *(task 3.5; concept: concrete_examples_over_prose; item `d3-10`)*

---

**9. C** — @import keeps CLAUDE.md modular: shared standards live in focused files and each package pulls in exactly what its maintainers own. User level breaks sharing, skills are on-demand rather than always-applied, and /memory only reports what loaded. *(task 3.1; concept: import_modularity; item `d3-02`)*

---

**10. A** — The -p/--print flag runs non-interactively and exits after printing; --output-format json with --json-schema yields structured, machine-parseable findings suitable for posting as PR comments. The other options name environment variables and flags that do not exist, or shell workarounds that do not change Claude Code's mode. *(task 3.6; concept: headless_structured_output; item `d3-11`)*

---

**11. B** — Skills are invoked on demand for task-specific workflows; CLAUDE.md is loaded every session. Universal standards therefore belong in CLAUDE.md — a skill only works if someone remembers to call it. Verbosity argues for context: fork, parameters argue for argument-hint, and single-user scope argues for the personal directory. *(task 3.2; concept: on_demand_vs_always_loaded; item `d3-06`)*

---

**12. D** — /memory exists precisely for this diagnosis: it shows the memory files in force so hierarchy mistakes (user vs project vs directory) surface immediately. /compact reduces context size, --resume continues a session, and .mcp.json configures MCP servers, not memory. *(task 3.1; concept: memory_command_diagnosis; item `d3-03`)*

---
