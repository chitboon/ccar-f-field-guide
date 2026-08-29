# CCAR-F Concept Drill — Domain 4: Prompt Engineering & Structured Output

12 items, one correct answer each. Untimed. Answer all 12 first, then
grade against the key in one pass — item by item, reading each rationale,
including the ones you got right. This is a fast recall check for this domain,
not an exam simulator: the domain drills and mocks carry the scenario register.

---

**1.** `[task 4.3 · schema guarantees]` After an extraction pipeline switches to tool use with a strict JSON schema, malformed-JSON errors drop to zero — but extracted line items still sometimes fail to sum to the stated invoice total. Why didn't the schema catch this?

A. The schema was too small to hold the line-item array, so some extracted values were silently dropped.
B. tool_choice was left on 'auto', so the model sometimes answered in prose instead of calling the tool.
C. The model's temperature was set high enough to corrupt the numeric fields during generation.
D. A schema enforces syntactic shape, not semantic consistency; cross-field checks like sums need separate validation.

---

**2.** `[task 4.1 · explicit criteria]` A PR-review agent instructed to 'be conservative and only report high-confidence findings' still floods reviews with nitpicks, and its false-positive rate has not moved. Which prompt change actually reduces false positives?

A. Strengthen the wording to 'be very conservative' and repeat it at the start and end of the prompt.
B. Define explicit inclusion criteria (report bugs, security issues) and exclusion criteria (skip style, local patterns).
C. Require a self-rated confidence score on every single finding, and filter out anything that is rated below 8 out of 10.
D. Switch the review work to a larger model that judges which findings actually matter to authors.

---

**3.** `[task 4.5 · batch vs synchronous]` Which workload is the right candidate for the Message Batches API?

A. A blocking pre-merge check that must finish before a developer is allowed to merge.
B. An interactive pair-programming session where a developer watches and reacts to each reply.
C. A nightly technical-debt report over 2,000 files that is reviewed the next morning.
D. Real-time triage of incoming on-call pages as they arrive in the incident queue.

---

**4.** `[task 4.3 · strictness spectrum]` Three techniques can push a model toward JSON output: prompt-only formatting instructions, prefilling the assistant turn with an opening brace, and tool use with a forced schema. Which statement ranks them correctly?

A. Tool use with a forced tool_choice is strictest; prefilling the assistant turn with '{' constrains the continuation into JSON; prompt-only instructions are weakest.
B. Prompt-only instructions are strictest because natural language is the model's native interface; prefill and tool use are equal-strength fallbacks for older models.
C. Prefill is strictest because the model can only continue the given prefix; once a schema exists, tool use adds no further constraint worth the extra tokens.
D. All three are equivalent in strictness; they differ only in token cost, latency, and how the request is authenticated.

---

**5.** `[task 4.1 · false-positive management]` One automated-review category ('naming suggestions') has a 70% dismissal rate, and developers have started ignoring the same tool's accurate security findings as a result. What is the best interim move?

A. Raise the severity label on the security findings so they stand out clearly from the noise.
B. Add more few-shot examples to the security category to lift its accuracy even further above the rest.
C. Reduce the review cadence so that developers see fewer total findings in any given week.
D. Disable the naming category while its prompt is reworked, protecting trust in the rest.

---

**6.** `[task 4.4 · retry limits]` An invoice extraction fails validation because the source document never states a contract end date. Will a retry loop that appends the validation error to the prompt fix this?

A. Yes — enough retries with the error attached will eventually produce a value that satisfies the validator.
B. Yes — but only with a larger model that can infer the missing date reliably from context.
C. No — retries fix format and structural errors, not information absent from the source; make the field nullable.
D. Yes — provided the schema is tightened so each retry error becomes more specific.

---

**7.** `[task 4.2 · few-shot prompting]` Detailed written instructions alone keep producing inconsistent output formats in a citation-extraction task, especially on documents with unusual layouts. What most reliably stabilizes the output?

A. Lengthen the instructions until every layout variation is described explicitly in precise prose.
B. Add two to four few-shot examples covering the varied layouts, each showing the desired output.
C. Lower the temperature to zero so the output stops varying between runs of the same input.
D. Wrap the call in a validation-retry loop that reprompts on any format error.

---

**8.** `[task 4.3 · nullable schema fields]` An extraction schema marks every field required, and the model has started inventing plausible values for fields the source documents do not contain. What is the correct fix?

A. Make the fields nullable so the model can return null instead of fabricating.
B. Add an explicit line to the prompt instructing the model that it must never fabricate values.
C. Widen every field's type to plain string so that any guess remains syntactically valid.
D. Retry the failed documents in a loop until the validator finally accepts the extracted values.

---

**9.** `[task 4.6 · independent review]` Why does a fresh Claude instance reliably catch issues in code that the generating session keeps approving, even when the generating session is told to review its own work critically?

A. The fresh instance starts with a larger available context window for the review pass.
B. The fresh instance runs with extended thinking enabled by default during reviews.
C. The generating session's temperature drops once it has finished producing the code.
D. The generating session retains the reasoning that produced the code and stays anchored to it; a fresh instance reviews without that context.

---

**10.** `[task 4.3 · enum extensibility]` A document-type enum offers five fixed categories, and unusual documents keep getting force-fit into the nearest one — corrupting downstream routing statistics. What schema change fixes this?

A. Remove the enum constraint so the model can return any category string it likes for the document.
B. Add an 'other' value with a detail field, plus 'unclear' for genuinely ambiguous documents.
C. Add more required fields so each document is forced to describe itself fully at extraction.
D. Split the five categories into twelve narrower ones to reduce the force-fit mismatch further.

---

**11.** `[task 4.5 · batch failure handling]` A batch of 500 document extractions finishes with 18 failed requests mixed among the successes. How do you identify and reprocess exactly those 18?

A. Match results to requests by custom_id and resubmit only the failed documents with fixes.
B. Resubmit the entire batch and keep whichever copy of each result happens to finish first.
C. Sort results by position, since failed requests tend to cluster near the end of a large batch.
D. Rerun the batch with a higher max_tokens so the failed requests succeed on the second pass.

---

**12.** `[task 4.4 · feedback loop design]` Developers dismiss 40% of an automated reviewer's findings, and the team wants to learn which code constructs trigger the false alarms so the prompt can be fixed systematically. What should each structured finding record?

A. A longer prose explanation of why the finding might matter to the pull request author.
B. A severity guess, so that dismissed findings can be downgraded in bulk at the triage meeting.
C. A detected_pattern field naming the triggering construct, so dismissals group by pattern.
D. A timestamp, so stale findings can be expired out of the review queue automatically each week.

---
