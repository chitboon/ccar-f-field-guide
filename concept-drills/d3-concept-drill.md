# CCAR-F Concept Drill — Domain 3: Claude Code Configuration & Workflows

12 items, one correct answer each. Untimed. Answer all 12 first, then
grade against the key in one pass — item by item, reading each rationale,
including the ones you got right. This is a fast recall check for this domain,
not an exam simulator: the domain drills and mocks carry the scenario register.

---

**1.** `[task 3.1 · CLAUDE.md hierarchy]` A new hire's Claude Code never applies the team's review checklist, while everyone else's does. The checklist lives in the tech lead's ~/.claude/CLAUDE.md. What is the root cause?

A. The new hire's model version predates support for checklist-style instructions.
B. The checklist file has grown past the length CLAUDE.md can load at startup.
C. User-level configuration applies only to that user and is not shared via version control.
D. The new hire needs to run /compact before the project instructions will load.

---

**2.** `[task 3.4 · plan mode vs direct execution]` Which task is the best candidate for direct execution rather than plan mode?

A. Adding a null check to one validation function, with a stack trace at the exact line.
B. Migrating a validation library whose imports touch 45 files across the repository.
C. Restructuring a monolithic application into a set of separately deployed services.
D. Choosing between two caching architectures with different infrastructure requirements and costs.

---

**3.** `[task 3.4 · multi-phase workflow]` A database schema migration must run as: explore the schema, migrate staging, pause for a human sign-off, then migrate production. The work is well understood, but no changes may ship between phases without approval. Which working mode fits best?

A. Plan mode: produce one plan, get it approved once, then execute the whole thing end to end.
B. Direct execution with a very detailed prompt describing all four steps in advance.
C. A single autonomous loop with a high iteration cap and an extremely strict system prompt.
D. A multi-phase workflow with explicit checkpoints and human approval between phases.

---

**4.** `[task 3.2 · slash command scoping]` Where should a /review slash command live so that every developer on the team gets it automatically when they clone or pull the repository?

A. In ~/.claude/commands/ on each machine, distributed through the onboarding docs.
B. In the .claude/commands/ directory inside the project repository.
C. In the project CLAUDE.md file, under a clearly marked Commands heading.
D. In .claude/config.json, registered as an entry in a commands array.

---

**5.** `[task 3.3 · path-specific rules]` Test files sit beside the code they test across 40 directories (cart.test.ts alongside cart.ts); every one must follow a single naming and structure convention. What is the most maintainable configuration?

A. A CLAUDE.md in every directory containing tests, each one repeating the same convention text for that area.
B. One root CLAUDE.md section headed 'Testing conventions' that Claude must infer is relevant to a given file.
C. A .claude/rules/ file with a paths glob like **/*.test.tsx, loading only on matching files.
D. A project skill named testing-conventions that developers invoke before writing any tests.

---

**6.** `[task 3.6 · scripted CLI startup]` A CI step runs claude -p 400 times a day and each call is slow, because startup auto-discovers every skill, command, and settings layer — though the job only ever needs one fixed corporate-standards prompt. Which flags address this?

A. Run claude -p twice per job, so the second invocation can reuse warm startup state.
B. Resume one long-lived named session with --resume so startup happens only once.
C. Add --output-format json so startup can skip the rendering work it does not need.
D. Use --bare to skip auto-discovery, with --system-prompt-file or --append-system-prompt to inject the standards text.

---

**7.** `[task 3.2 · skill isolation]` A codebase-analysis skill dumps 3,000 words of exploration into the main conversation every time it runs, burying the actual task context. Which SKILL.md frontmatter option fixes this?

A. context: fork, so the skill runs in an isolated sub-agent context and only its result returns.
B. allowed-tools, restricted to Read only, so the skill produces less output on each run it makes.
C. argument-hint, so the skill asks the developer for a narrower scope before it starts.
D. Moving the skill into ~/.claude/skills/ so that it loads in a lighter per-user context.

---

**8.** `[task 3.5 · iterative refinement]` A data-migration prompt has been rewritten in careful prose four times, and the output transformation is still applied inconsistently. What is the most effective next step?

A. Lengthen the prose description until every alternative interpretation is explicitly excluded.
B. Provide two or three concrete input/output example pairs showing the transformation.
C. Set the temperature to zero so the model stops varying its interpretation.
D. Ask the model to restate the requirement in its own words before executing.

---

**9.** `[task 3.1 · modular CLAUDE.md]` A monorepo's root CLAUDE.md has grown to 2,000 lines covering twelve packages, and most edits touch only one. Claude's answers are getting slower and occasionally follow the wrong package's conventions. What structure fixes this?

A. Move the whole file to user level so it loads once per machine, not per session.
B. Delete the file and convert each package's conventions into a separately invoked skill.
C. Split the standards into per-topic files and @import the relevant ones from each package's CLAUDE.md.
D. Keep the single file but have developers run /memory at the start of each session.

---

**10.** `[task 3.6 · non-interactive CI invocation]` Which invocation produces machine-parseable pull-request findings in CI without hanging waiting for input?

A. claude -p with --output-format json and a --json-schema describing the findings shape.
B. claude with stdin redirected from /dev/null so the process never blocks waiting for input.
C. claude --batch so the review job queues instead of blocking the whole pipeline.
D. CLAUDE_HEADLESS=true claude with the review prompt passed as its sole argument.

---

**11.** `[task 3.2 · skill vs CLAUDE.md]` When is a skill the wrong vehicle for team guidance, compared with putting the same content in CLAUDE.md?

A. When the guidance produces verbose output that could crowd the main conversation context.
B. When the guidance must apply in every session — skills load on demand, CLAUDE.md always loads.
C. When the guidance needs parameters that a developer supplies at invocation time.
D. When the guidance is used heavily by one developer but rarely by the rest.

---

**12.** `[task 3.1 · diagnosing loaded memory]` Claude Code behaves inconsistently across two sessions in the same repository, and you suspect different memory files are loading in each. Which command confirms what is actually loaded?

A. /compact, which rebuilds the memory index from the current directory tree.
B. --resume, which reloads the session together with its original memory set.
C. Re-reading .mcp.json, where the memory precedence order is declared.
D. /memory, which lists which memory files are loaded in the session.

---
