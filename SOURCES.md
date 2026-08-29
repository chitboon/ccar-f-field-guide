# Sources — Claude Certified Architect – Foundations (CCAR-F)

## Official exam guide
- Version: 1.0 (July 2026 edition)
- Local path: `_exam-guides/ccar-f/` — not committed, see the restructure plan §4
- What this guide takes from it: the five domain names and weights, and the
  objective inventory once the guide is written. All objective descriptions
  will be paraphrases, never the published wording. No sample item or answer
  from the exam guide appears in this repository.

## Anthropic Academy courses

The following Academy courses informed the guide's concept coverage and
the domain-drill and mock item authoring. Objective descriptions are
paraphrased, never the published wording.

- `claude-code-101/` — Subagents, Context management, the
  explore→plan→code→commit workflow, MCP, Skills, Hooks, CLAUDE.md,
  Code review
- `introduction-to-subagents/` — task delegation, context isolation,
  coordinator responsibilities
- `building-with-claude-api/` — Agents and tools, Agents and workflows,
  Workflows vs agents, Routing/Parallelization/Chaining, Tool use, Tool
  schemas, Prompt engineering, Structured data, System prompts, Temperature,
  Evaluation workflows
- `introduction-to-model-context-protocol/` — MCP fundamentals, transports,
  sampling, roots, notifications
- `mcp-advanced-topics/` — advanced MCP patterns
- `introduction-to-agent-skills/` — skill frontmatter (`context: fork`,
  `allowed-tools`, `argument-hint`)

## Third-party material consulted

Five third-party repositories were fetched and read for this credential (see
`_external/github/MANIFEST.md` for the full manifest, licences and commit
SHAs). **Nothing from any of them is republished** — this is the same policy
that covers the captured Academy material, stated in the restructure plan §7a.

- [hamzafarooq/claude-certified-architect](https://github.com/hamzafarooq/claude-certified-architect) —
  MIT © 2025 Hamza Farooq. Supplied 20 of the 60 items in the private
  reference mock at `_external/derived/ccar-f-mock/`.
- Repo `utkarsh1agarwal/claude-architect-exam-guide`, hosted at
  https://github.com/utkarsh1agarwal/claude-architect-exam-guide —
  MIT © 2026 contributors. Supplied 23 of the 60 items in the private
  reference mock.
- [timothywarner-org/claude-architect](https://github.com/timothywarner-org/claude-architect) —
  MIT © 2026 Timothy Warner Organization. Read for reference; nothing from it
  is currently used in any local artefact.
- Repo `OlivierAlter/Claude-Certified-Architect-Foundations-Certification-Exam`,
  hosted at https://github.com/OlivierAlter/Claude-Certified-Architect-Foundations-Certification-Exam —
  **no licence, all rights reserved.** Supplied 17 of the 60 items in the
  private reference mock. Also the source of two files that were found
  reproducing it near-verbatim and were quarantined rather than published:
  see `_external/derived/olivier-cert-exam-skill/README.md` and
  `_external/derived/ccar-f-top20/README.md`.
- [paullarionov/claude-certified-architect](https://github.com/paullarionov/claude-certified-architect) —
  **no licence, all rights reserved.** Read for reference; nothing from it is
  currently used in any local artefact.

**What was taken and what was not.** All 60 items of the private reference
mock in `_external/derived/ccar-f-mock/` came from three of these five repos,
in the proportions above. That mock is not published and never will be — see
its own README. What these five repositories are permitted to inform, per the
restructure plan §8a, is *which discriminations are worth testing*: three
independent sources converging on the same 60 judgement points is real signal
about the blueprint. The practice bank this credential still needs will be
written from the published objectives, using that signal to decide what to
test, never copying an item, option or rationale from any of the five.

## Provenance
Not yet checked against the author's private source corpus — there is no
guide prose to check yet. This is a note for the author, not a reader: the
checking tool is not shipped in this repository (see `tools/README.md` for
why), and is run from the private workspace this guide is developed in,
against this repo as a checkout target. Both of the following must pass, at
8+ and 12+ word thresholds respectively, before any guide prose is committed:

```
python3 _workspace/tools/check-provenance.py publish/ccar-f-field-guide \
  --source _anthropic-training --source _exam-guides
python3 _workspace/tools/check-provenance.py publish/ccar-f-field-guide \
  --source _external/github --min-words 12
```

The second check matters more here than for any other credential in this
collection: the third-party repositories are as easy to absorb phrasing from
as the Academy courses are, and the Associate guide never had to guard against
that.
