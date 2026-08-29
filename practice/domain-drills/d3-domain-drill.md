# CCAR-F Domain Drill — Domain 3: Claude Code Configuration & Workflows

10 items, one correct answer each. Untimed. Answer all 10 first, then
grade against the key in one pass — item by item, reading each rationale,
including the ones you got right. This is a scenario-based drill for this
domain, not a recall check: every stem carries a concrete situation you
must reason about.

---

**1.** `[task 3.1 · config scope]` A 12-person backend team at Meridian Health clones a shared repository containing 3 microservices. New hires keep missing the project's coding standards because their Claude Code sessions use different defaults. The team lead wants a single file that, once committed, ensures every teammate who clones the repo gets the same Claude configuration rules. Which file should they commit?

A. `~/.claude/CLAUDE.md`
B. `~/.claude/settings.json`
C. `project-root/CLAUDE.md`
D. `project-root/.claude/personal.md`

---

**2.** `[task 3.1 · config reuse with @import]` A platform team maintains coding standards for TypeScript, testing conventions, and API design across 5 microservice repositories. They currently copy-paste the same 3 markdown files into each repo, but edits in one repo silently drift from the others. Which approach best achieves shared standards without duplication?

A. Copy the standards into each repo's README.md and hope engineers read them before starting work.
B. Store standards in `~/.claude/` and reference them in each repo.
C. Use `@import` in a project-level CLAUDE.md to pull from a shared source.
D. Commit the standards only to a wiki page.

---

**3.** `[task 3.2 · commands location]` A team wants a `/deploy` slash command that every engineer on the 12-person team can invoke from any clone of the repository. The command should be version-controlled alongside the codebase. Where should the command file be stored?

A. `~/.claude/commands/`
B. `~/.claude/skills/`
C. `.claude/commands/`
D. `/usr/local/bin/`

---

**4.** `[task 3.2 · command frontmatter]` A team creates a custom `/deploy` command in `.claude/commands/deploy.md`. They want it to prompt the operator for a target environment and to restrict the command to Bash tool calls only. Which frontmatter field restricts the command to a specific tool?

A. `context: fork`
B. `allowed-tools: Bash`
C. `argument-hint: environment`
D. `description: Deploy to production`

---

**5.** `[task 3.3 · path-scoped rules]` At Orbit Finance, the monorepo contains backend payment services under `/services/payments/` and a React analytics dashboard under `/dashboard/analytics/`. The platform team wants backend-specific rules to apply only to files matching `/services/**` while dashboard-specific rules apply only to `/dashboard/**`, regardless of which subdirectory a developer currently has open. Which rule configuration should they use?

A. One global CLAUDE.md with conditional comments that try to handle every directory's rules in a single file.
B. Directory-scoped README.md files in every subfolder.
C. A `.claude/settings.json` file that maps user names to directories.
D. Separate `.claude/rules/*.md` files with `paths:` globs matching the target file patterns.

---

**6.** `[task 3.4 · plan mode]` A platform team must choose between a microservices refactor and a modular monolith for their payment engine. Before any files are changed, they want to compare trade-offs, document the decision, and agree on an architecture. Which workflow is most appropriate?

A. Use plan mode first.
B. Start editing files immediately and decide later.
C. Run a headless CI job to generate the architecture.
D. Create a personal note in `~/.claude/plan.md` that only one engineer can see.

---

**7.** `[task 3.5 · iterative refinement]` A developer is fixing a flaky reporting module in a monthly close process. They want each refinement step to be guided by measurable signals rather than intuition, so they can confirm progress after every small change. Which practice best supports this iterative approach?

A. Rely on the developer's memory of previous failures and intuition about what went wrong.
B. Update code only after a concrete failing test reproduces the bug, and verify each change against the test suite before moving on.
C. Batch three unrelated fixes together to save time and reduce the total number of test runs.
D. Write a lengthy prose description of the expected behavior and review it manually each time.

---

**8.** `[task 3.5 · iterative refinement principle]` Rivera Corp's nightly data pipeline fails intermittently in production but passes reliably on local laptops. The on-call engineer suspects a race condition, a short timeout, and a missing retry. Instead of changing all three suspected causes at once, the engineer reproduces one failure with a targeted test, adjusts a single variable, and reruns the test before moving on. Which principle does this illustrate?

A. Iterative refinement using concrete, measurable signals from targeted tests and one controlled change.
B. Batch all suspected causes together to maximize throughput and minimize overall debugging time spent.
C. Plan mode for making large architectural decisions before any code is written or modified.
D. CI headless execution with structured JSON output for automated downstream processing.

---

**9.** `[task 3.6 · CI flag]` A CI pipeline runs Claude Code to review pull requests. The script needs Claude to print its review results to stdout so the CI system can capture and parse them, without launching the interactive terminal UI. Which flag should the script pass?

A. `--interactive`
B. `-p` or `--print`
C. `--verbose`
D. `--no-color`

---

**10.** `[task 3.6 · CI isolation]` A CI job reviews pull requests with Claude Code. The team wants the review to follow project standards defined in the repository's CLAUDE.md, and they want each run to be isolated from any engineer's personal machine settings. Which setup best achieves this?

A. Use the engineer's local `~/.claude/` settings.
B. Mount the repo in a fresh container with the project CLAUDE.md.
C. Skip CLAUDE.md to avoid stale guidance.
D. Run the review against the main branch instead of the PR branch.

---
