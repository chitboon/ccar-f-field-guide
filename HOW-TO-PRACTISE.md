# How to Practise

A guide to getting the most from this repo's drills and mocks. Covers the
drill loop, the concrete-artifact authoring rule, calibration techniques,
and hands-on reps.

## The drill loop

1. **Sit the drill.** Untimed, one item at a time or all at once — your
   choice. Do not peek at the key.
2. **Grade in one pass.** Compare your answers to the key. For every miss,
   read the rationale — including the items you got right.
3. **Classify each miss.** Three categories:
   - **Didn't know it** → re-read the corresponding `guide/dN-*.md` page
   - **Misread the item** → slow down; scenario stems reward careful reading
   - **Picked the plausible-but-soft option** → review `guide/trap-patterns.md`
4. **Re-sit if needed.** If your score is below the target (90% for concept
   drills, 80% for domain drills, 720-equivalent for mocks), re-read the
   guide material and re-sit after a break.

## The concrete-artifact rule

Every scenario stem in domain drills and mocks carries at least one concrete,
specific artifact — a number ("47 pull requests"), an actual tool-response
payload, a literal file or line count, a named tool. This is an authoring
principle, not a content limitation: the artifacts make the scenario real
enough to test judgement, not just recognition.

When writing your own practice items (or explaining concepts to a colleague),
follow the same rule: name the tool, cite the number, quote the error
message. Abstract scenarios test whether you can identify the category;
concrete scenarios test whether you can apply the mechanism.

## Recognition vs. mechanism

A correct multiple-choice answer is not proof of mechanism understanding.
The exam's four-option format rewards recognition — picking the answer that
"looks right" — as much as derivation. To calibrate:

- **After each item, ask yourself:** "Could I explain *why* this answer is
  correct to someone who picked the wrong one?" If the answer is "it just
  seemed right," that's recognition, not mechanism.
- **Self-report signal:** When reviewing rationales, note whether the correct
  answer felt obvious (recognition) or whether you had to reason through the
  why (mechanism). Items where you picked the right answer for the wrong
  reason are soft spots the exam can expose with different framing.
- **The gap matters:** "Concept drill feels easy but mock feels hard" is real
  signal. It means you recognize mechanisms but cannot yet *select* them
  under scenario pressure — which is what the exam actually grades.

## Hands-on reps

Some objectives benefit from actual tool use, not just written drills. These
are not optional extras — they are the kind of "obvious in hindsight once you
know the mechanism, easy to miss otherwise" reps that close the gap between
recognition and application.

### Glob vs Grep

The most commonly confused tool pair. Glob matches *file paths/names* against
a pattern (`**/*.ts`). Grep searches *file contents* for a pattern. A
rename/signature-change task needs Grep on the function name to enumerate
call sites, then Read on each match — Glob alone cannot answer "who calls
this."

**Rep:** In any repo, run `grep -r "functionName" --include="*.ts" .` and
see the list of call sites. Then run `glob **/*.ts` and see that it returns
file paths, not content matches. The felt difference transfers directly to
exam items.

### Plan mode vs direct execution

Plan mode is for large architectural decisions with competing approaches
before execution. Direct execution is for small, well-scoped changes with
clear fixes.

**Rep:** Try both in a real Claude Code session. Use plan mode for a
multi-file refactor where you need to compare approaches. Use direct
execution for a one-line bug fix. The selection criteria (scope, risk,
number of valid approaches) become intuitive after one rep each.

### CLI flags for CI

`-p` / `--print` runs Claude Code headlessly and emits output to stdout.
`--bare` skips skill/command/settings auto-discovery for faster scripted
calls. `--system-prompt-file` injects required prompt content directly.

**Rep:** Run `claude -p "explain what this repo does"` in a terminal and
compare the output to an interactive session. Then try `claude -p --bare --system-prompt-file prompt.txt "review this PR"` to see the fast startup
path for CI.

### Structured output enforcement

The strictness spectrum: prompt-only formatting instructions (weakest) →
prefilled assistant turn with opening brace (constrains continuation) →
tool use with JSON schema + forced `tool_choice` (strictest, schema-enforced
by construction).

**Rep:** Try all three in a Claude Code or API session. Ask for JSON output
with only a prompt instruction, then with a prefilled `{`, then with a tool
definition that has a JSON schema. The difference in reliability is the
exam's tested distinction.

## Score interpretation

| Tier | Target | What it means |
|---|---|---|
| Concept drill | 90%+ | You can state mechanisms cold |
| Domain drill | 80%+ | You can select mechanisms under scenario pressure |
| Mock | 720+ equivalent | You can sustain judgement across 60 items at exam density |

A concept-drill score above 90% with a mock score below 720 means the gap
is in scenario application, not knowledge. Focus on domain drills and
targeted drills, not more concept drills.

## Writing your own items

If you want to author additional practice items (for yourself or a study
group), follow these rules:

1. **Single-answer 4-option only.** The real exam has no multi-select.
2. **Concrete-artifact rule.** Every stem carries at least one specific
   number, tool name, error payload, or file path.
3. **Closely-spaced distractors.** The correct answer should not be obviously
   longer, more detailed, or in a different register than the distractors.
   All four options should be plausible to someone who doesn't know the
   answer.
4. **Vary answer positions.** Don't always make A or B the correct answer.
   Balance across A, B, C, D.
5. **Don't leak length cues.** The correct answer should not consistently be
   the longest or shortest option. Vary it.
