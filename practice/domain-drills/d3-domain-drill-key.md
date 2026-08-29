# CCAR-F Domain Drill — Domain 3: key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. C** — A project-level CLAUDE.md at the repository root travels with the codebase and applies to everyone who clones it. Options A and B are user-level locations that only configure the local machine. Option D places a personal file inside the project scope, which contradicts the goal of shared, version-controlled rules. *(task 3.1; concept: config_scope; item `d3d-01`)*

---

**2. C** — `@import` lets a project-level CLAUDE.md assemble shared standards from a common source while remaining version-controlled with the repo. Options A and D create drift-prone copies or out-of-band documentation. Option B ties the standards to one user's machine, so teammates do not receive them. *(task 3.1; concept: config_scope; item `d3d-02`)*

---

**3. C** — Project-specific commands belong in `.claude/commands/` at the repository root so they are committed and shared. Options A and B are user-level directories for personal tooling. Option D is a system path unrelated to Claude's command discovery. *(task 3.2; concept: commands_and_skills; item `d3d-03`)*

---

**4. B** — `allowed-tools` restricts which tools the command may invoke, which is exactly what limiting to Bash achieves. `context: fork` runs in isolation but doesn't restrict tools. `argument-hint` prompts the user for input. `description` is metadata. *(task 3.2; concept: commands_and_skills; item `d3d-04`)*

---

**5. D** — Separate `.claude/rules/*.md` files can use `paths:` frontmatter globs to apply conventions by file pattern across the entire repository. Option A conflates all rules into one global file without pattern targeting. Option C maps users rather than files. Option B scatters policy into README files that Claude does not automatically treat as rules. *(task 3.3; concept: path_scoped_rules; item `d3d-05`)*

---

**6. A** — Plan mode is designed for large architectural decisions and competing approaches before execution begins. Option B skips the decision point and invites rework. Option C automates output but does not facilitate collaborative architectural deliberation. Option D keeps the plan on a single user's machine. *(task 3.4; concept: plan_mode; item `d3d-06`)*

---

**7. B** — Iterative refinement depends on concrete examples and actual failing tests as measurable signals. Option B establishes the failing signal and closes the loop before the next change. Options A and D rely on vague prose or memory, while option C batches interacting fixes and obscures which change resolved what. *(task 3.5; concept: iterative_refinement; item `d3d-07`)*

---

**8. A** — This illustrates iterative refinement: each loop is tied to a concrete failing test and one controlled change. Option B would obscure which fix mattered. Option C is for up-front architectural choices, not incremental debugging. Option D describes CI tooling, not the debugging methodology shown. *(task 3.5; concept: iterative_refinement; item `d3d-08`)*

---

**9. B** — The `-p`/`--print` flag runs Claude Code headlessly and emits output to stdout, which is suitable for CI. Option A enables the interactive UI that CI cannot drive. Options C and D affect logging detail or color but not headless execution. *(task 3.6; concept: ci_integration; item `d3d-09`)*

---

**10. B** — A fresh container with the project's CLAUDE.md provides isolated, reproducible context and applies shared standards. Option A relies on personal configuration that teammates may not share. Option C removes the standards file. Option D reviews the wrong code state. *(task 3.6; concept: ci_integration; item `d3d-10`)*

---
