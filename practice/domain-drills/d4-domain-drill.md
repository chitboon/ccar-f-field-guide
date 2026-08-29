# CCAR-F Domain Drill — Domain 4: Prompt Engineering & Structured Output

10 items, one correct answer each. Untimed. Answer all 10 first, then
grade against the key in one pass — item by item, reading each rationale,
including the ones you got right. This is a scenario-based drill for this
domain, not a recall check: every stem carries a concrete situation you
must reason about.

---

**1.** `[task 4.1 · explicit criteria]` A project-management agent reviews quarterly budget reports and flags 'significant' variances for the finance team. After 3 quarters, the agent flags 47 variances but the finance team finds that 31 of them are under $5,000 and well within normal fluctuation. The word 'significant' is doing no real work in the prompt. Which revision best fixes this?

A. Replace 'significant' with 'variances the project manager should know about.'
B. Ignore all variances below $1 million.
C. Add more examples of generic budget reports without numeric bounds.
D. Define thresholds: flag variances ≥10% or ≥$50,000 from baseline.

---

**2.** `[task 4.1 · explicit classification]` A customer-experience agent processes 200 complaints per week and must flag 'high-risk' ones for immediate escalation. After reviewing a month of flags, the team finds that 40% of flagged complaints are routine billing questions that the agent's vague definition of 'high-risk' caught by mistake. Which instruction best turns this into an explicitly scorable classification task?

A. Ask the model to 'use good judgment about risk.'
B. Request the model to list every complaint verbatim, preserving all original wording without any classification.
C. Define risk tiers and required evidence fields.
D. Tell the model to flag only complaints mentioning legal terms.

---

**3.** `[task 4.2 · few-shot examples]` A support team is building a prompt that extracts refund reasons from customer chat transcripts. They have already written five detailed rules, but results still misclassify ambiguous cases like 'it wasn't what I expected' (could be product-defect or buyer-remorse). What is the most effective next step?

A. Increase the rule count to fifteen and ban all ambiguity.
B. Use only the last customer sentence.
C. Remove all rules and rely only on a generic task description.
D. Add three diverse input/output examples showing how ambiguous cases should be classified.

---

**4.** `[task 4.3 · schema design for parsing]` A prompt requests JSON outputs describing server incidents. The downstream parser needs to categorize severity programmatically. Which schema choice best supports reliable downstream parsing?

A. Wrap every JSON object inside triple backticks labeled json.
B. Use an enum for severity with values Low, Medium, High, Critical.
C. Allow free-text 'notes' without any length or type constraint.
D. Return the response as a Markdown table for human readability.

---

**5.** `[task 4.3 · schema design requirement]` A recruiting platform's API returns structured candidate-evaluation records to downstream systems that parse them programmatically. The current schema uses free-text fields and markdown formatting, causing 3 different parsers to break on unexpected characters. Which requirement is most important when redesigning this schema?

A. Include decorative markdown headers for readability, such as bold section titles and explanatory paragraphs.
B. Distinguish required and nullable fields; use enums for closed categories.
C. Let the model omit any field it considers irrelevant.
D. Store the entire evaluation in one unconstrained string field.

---

**6.** `[task 4.4 · retry on structural failure]` A pipeline extracts invoice line items from scanned PDF text. The model returns malformed JSON because a footnote interrupted the table. The source PDF clearly contains the line items on page 2. What is the best immediate action?

A. Retry the same prompt with a higher temperature and no other changes to the instructions.
B. Flag as missing information and stop.
C. Retry with instructions to ignore footnotes and emit only the requested schema.
D. Delete the footnote pages from the source PDF permanently so they cannot interfere again.

---

**7.** `[task 4.4 · targeted retry]` A medical-research assistant uses an LLM to extract adverse-event dates from clinical notes. Some notes contain clear dates the model misses because they appear only in parenthetical remarks or scanned headers. Which action forms the best first step in the handling strategy?

A. Retry with a prompt that instructs the model to look for dates in headers, footers, and parenthetical asides.
B. Retry every 'unknown' response up to ten times with the same prompt.
C. Hide all 'unknown' responses from the final dataset so they do not lower metrics.
D. Add a validation step that routes 'unknown' outputs and low-confidence extractions to a human reviewer.

---

**8.** `[task 4.5 · batch vs sync]` A security team must scan three million archived log files for suspicious token patterns across multiple cloud buckets. The scan is not time-critical but must complete within 48 hours. Another team needs a pre-merge gate that checks every pull request for leaked secrets and returns a clear pass-or-fail verdict within seconds. Which API pairing best fits both workloads?

A. Batch API for the log scan and synchronous API for the pre-merge gate.
B. Synchronous API for both the log scan and the pre-merge gate, processing every file one request at a time.
C. Batch API for both the log scan and the pre-merge gate.
D. Synchronous API for the log scan and batch API for the pre-merge gate.

---

**9.** `[task 4.6 · self-review bias]` A generative coding assistant drafts unit tests for a new authentication module. The same model instance that wrote the code also reviews it and consistently approves the tests. Bugs still reach production. Which change is most likely to catch the remaining defects?

A. Review again with a 'be critical' prefix.
B. Route the tests to a separate model instance for independent review.
C. Remove all automated review and ship whatever the generator produces.
D. Increase the generator temperature so it writes different tests next time.

---

**10.** `[task 4.6 · independent review]` A financial content platform uses an LLM to draft compliance summaries and then has the same model score each summary against a detailed checklist. The checklist scores are consistently high, yet external auditors repeatedly find material omissions. The team adds more checklist items but the self-review scores stay high. What is the most effective remediation?

A. Switch to an independent model instance for review.
B. Increase the temperature of the self-review prompt to make it more 'creative.'
C. Shorten the checklist so the model is more likely to pass every item.
D. Train the generator model on past auditor findings and continue self-review.

---
