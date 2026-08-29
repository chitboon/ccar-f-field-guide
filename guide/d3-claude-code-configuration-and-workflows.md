# Domain 3 — Claude Code Configuration & Workflows (20%, ~12 items)

The configuration domain: where instructions live, how they load, and how
work is staged. Most items are really asking *which layer owns this
behaviour* — memory, command, skill, hook, permission, or CI flag.

## Objective map (paraphrased from the published exam guide)

- **3.1 CLAUDE.md hierarchy** — the memory tiers, their scope, and keeping
  them modular.
- **3.2 Slash commands and skills** — custom commands, skill frontmatter,
  and scoping.
- **3.3 Path-specific rules** — conditional loading of conventions by file
  type or location.
- **3.4 Plan mode vs direct execution vs multi-phase workflow** — a
  **three-way** selection by task scope, risk, and approval needs.
- **3.5 Iterative refinement** — progressive improvement loops and concrete
  failure signals.
- **3.6 CI/CD integration** — non-interactive invocation, structured output
  for pipelines, and scripted startup performance.

## Mental models

**Four memory tiers, unioned — no single winner.** User `~/.claude/CLAUDE.md`
(personal defaults, every project, **not in VCS** — the classic "new
teammate missing the instructions" trap) → project `./CLAUDE.md` (the team
contract; commit it) → subtree `<subdir>/CLAUDE.md` (loads on demand when
files there are read) → `CLAUDE.local.md` (gitignored personal tweaks).
Conflicts are model-interpreted, so write non-conflicting rules or state
precedence explicitly. `/memory` shows which files actually loaded — the
first diagnostic when behaviour diverges between teammates. `@import` keeps
a root file modular. Conventions tied to a file type across many directories
belong in `.claude/rules/*.md` with YAML `paths:` globs — they load only
when a matching file is read, beating forty scattered subtree files.

**CLAUDE.md vs skill vs command.** CLAUDE.md is always-loaded universal
standards; a skill is an on-demand task-specific workflow (a 400-line
CLAUDE.md wants splitting into skills); a command is a user-invoked macro.
Commands live in `.claude/commands/` (project, shared via VCS) or
`~/.claude/commands/` (personal). Skills live in `.claude/skills/<name>/SKILL.md`;
the model discovers them by the **description** — write it like a help-desk
ticket title. Three frontmatter keys matter: `context: fork` (run the skill
in an isolated subagent context so verbose exploration never lands in the
main conversation), `allowed-tools` (restricts **built-in** tools only —
MCP access is inherited from the parent session, and there is no
`allowed-servers` field), and `argument-hint`. Personal override of a team
skill = a user-scope copy, never an edit to the project file.

**Permissions are static; hooks are dynamic.** `settings.json` allow/deny
rules are fixed policy. Hooks carry conditional logic. A hook error must
fail closed: structured error to the model, loud logging — silent hook
failure is policy quietly evaporating.

**Plan mode vs direct execution vs multi-phase workflow — a three-way
decision, not a binary.** This objective is explicitly about choosing among
three patterns by task scope, risk level, and human-approval requirements:

| Pattern | Shape | Choose when |
|---|---|---|
| Direct execution | Do it now | Single-file fix, clear stack trace, low risk, no approval needed |
| Plan mode | One plan → approve → execute cycle | Multi-file or architectural change, competing approaches, needs a single human sign-off before any edits |
| Multi-phase workflow | A sequence of discrete phases with checkpoints between them (explore → plan → implement → review) | Large scope where risk is spread across stages and each stage needs its own verification or approval gate before the next begins |

The distinction that matters: plan mode is *one* planning cycle gating
execution; a multi-phase workflow is *several* bounded phases, each with its
own checkpoint — you pick it when a single upfront plan cannot absorb the
risk, or when later phases depend on what earlier phases discover. "Start
direct, switch later" is the anti-pattern when complexity is stated in the
requirements; pair heavy discovery with the Explore subagent.

**CI is `claude -p`.** The `--print` flag makes the invocation
non-interactive: stdout, exit code, no hangs. `--output-format json` with
`--json-schema <schema>` yields machine-parseable output for pipelines;
`--allowedTools` restricts the surface. One fresh session per PR — reused
sessions contaminate review. `CLAUDE_HEADLESS`, `--batch`, `--ci` **do not
exist**; they are planted distractors.

**Scripted startup performance: `--bare`.** A non-interactive call pays for
auto-discovery of skills, commands, and the full settings hierarchy on every
invocation. When the actual need is a fixed prompt (e.g. corporate standards
text) run fast in a script, `--bare` skips that discovery, and you inject
the required content directly with `--system-prompt-file` or
`--append-system-prompt`. The pattern: discovery is what would have produced
the prompt context, so skipping it only works because you're supplying that
content yourself.

**Iterative refinement** converges fastest on concrete failure signals:
test-driven loops where each round produces a shared, specific failure beat
sequential fix-lists, and several interacting defects are best handled as
one coherent redesign, not three passes that rework the same code. For
unfamiliar legacy systems, the interview pattern — have Claude question you
before designing — surfaces the constraints you didn't know to state.

**Drill:** `concept-drills/d3-concept-drill.md`
**Traps:** config-scope inverse framing (`guide/trap-patterns.md`)
