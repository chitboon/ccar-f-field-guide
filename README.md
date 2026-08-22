# CCAR-F Field Guide

An unofficial study guide for the **Claude Certified Architect – Foundations** (CCAR-F) exam.

**The guide is not written yet.** This repo currently holds original practice
material, the exam facts, and the tooling to keep whatever gets added
honest. See below for what exists and what's still to come.

One of four Claude certification field guides, each in its own repository and
built to the same standard: [ccao-f-field-guide](https://github.com/chitboon/ccao-f-field-guide) (Associate – Foundations, **complete** — start there if you want to see the finished shape of these guides), [ccar-p-field-guide](https://github.com/chitboon/ccar-p-field-guide) (Architect – Professional, not started), [ccdv-f-field-guide](https://github.com/chitboon/ccdv-f-field-guide) (Developer – Foundations, not started).

---

## No exam content. Ever.

This guide will contain **no questions, answers, or content from the live
exam**, and it never will. Candidates agree to keep exam content
confidential, so anything advertising itself as "real exam questions" or a
"dump" is either fabricated or a breach — and using it puts your credential
at risk.

Every practice item that ends up here will be written from the published
exam objectives. See [SOURCES.md](SOURCES.md) for exactly what informed
this guide and what, if anything, was taken from third-party material — that
question is not academic for this credential: several third-party study
guides were read while building it, and none of their content is reused.

## The exam

- 60 items, multiple-choice and multiple-response
- 720 on a 100–1000 scale to pass
- $125, 12 months' validity
- Proctored, online or at a test centre

Five domains and their weights, from the published exam guide:

| Domain | Weight |
|---|---|
| Agentic Architecture & Orchestration | 27% |
| Tool Design & MCP Integration | 18% |
| Claude Code Configuration & Workflows | 20% |
| Prompt Engineering & Structured Output | 20% |
| Context Management & Reliability | 15% |

## What's here

- **[practice/exam-day-cram.md](practice/exam-day-cram.md)** and
  **[practice/detailed-cheatsheet.md](practice/detailed-cheatsheet.md)** —
  roughly 4,400 words of genuinely original Architect material. Verified 0%
  matched, whole-file, against a private third-party mock, its key, and the
  official exam guide combined. This is the only original Architect content
  confirmed to survive that check, and the guide will be built outward from
  it.

## What's not here yet

- **The guide itself.** The main body of remaining work.
- **A practice bank.** Two candidate sources were checked and ruled out as
  starting points — a 60-item mock assembled entirely from three third-party
  repositories, and 159 items that looked like untagged originals but turned
  out to match the same third-party clones almost exactly. Neither is usable
  as a base. The practice bank for this credential has to be written from
  scratch against the published objectives — see "How to use this" below for
  how that actually gets done, incrementally, rather than all at once.

## How to use this

There's no full guide or drilled question bank yet, which makes this repo a
slightly different case from its sibling: the most useful thing you can do
with it right now is **generate your own practice items** rather than sit an
existing set.

- **Run it in Cowork.** Point Claude or Kimi at this cloned repo — `SOURCES.md`
  has the domain/objective framing, and `practice/` has the two original
  files above, which are worth reading first for the judgement calls they
  already cover.
- **The drill loop** — once you or a future version of this guide has written
  a practice set in the format used by
  [ccao-f-field-guide/practice/frameworks.md](https://github.com/chitboon/ccao-f-field-guide/blob/main/practice/frameworks.md),
  the same one-question-at-a-time paste-ready prompt from that repo's
  `HOW-TO-PRACTISE.md` applies unchanged.
- **Generating your own questions** is the section that matters most here.
  `tools/check-item-quality.py` (documented in `tools/README.md`) is copied
  into this repo for exactly this: write items from a named objective, then
  audit them before trusting them. The real numbers behind why that audit
  step isn't optional — a 94%-longest-answer draft, an 89% punctuation tell,
  a 16-of-18 answer run, all invisible without measuring — are in the
  Associate repo's `HOW-TO-PRACTISE.md`, from that guide's own drafts. Ask
  your tool something like:

  ```
  Using only the domain list in this repo's SOURCES.md, write 5 new
  multiple-choice items testing the objective "<name the objective>". Do not
  use, paraphrase, or reconstruct any real exam question or any item from a
  third-party guide — write these from the published objective description
  only. Vary the correct letter across items and avoid a punctuation tell in
  the correct answer.
  ```

  Then run `python3 tools/check-item-quality.py <the file>` and fix anything
  it flags.

  `check-provenance.py` — the tool that catches reproduced phrasing rather
  than cue defects — is not shipped here, because it needs a source corpus
  (the Academy captures and the third-party repositories) that readers won't
  have. For this credential specifically, that corpus mattered doubly: the
  third-party guides are as easy to absorb phrasing from as the courses are.

## Sources

See [SOURCES.md](SOURCES.md).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). The one hard rule: **no exam content, ever.**

## Licence

[CC BY 4.0](LICENSE) for the original writing. Quoted Academy material remains Anthropic's, and nothing third-party is reused here regardless of its own licence.

---

Compiled by **Chit Boon Lee** and **Claude**.
Unofficial. Not affiliated with, endorsed by, or sponsored by Anthropic.
