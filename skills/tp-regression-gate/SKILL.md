---
name: tp-regression-gate
description: Validate that a knowledge base has good retrieval coverage. Define test queries, search the KB for each, check that the right documents are returned, verify guardrail queries don't return false positives, and report coverage. Use when the operator wants to verify their KB is working correctly before relying on it for evaluation or proposal generation.
---

# tp-regression-gate

Validate that a knowledge base returns the right evidence for known
queries. This is a quality check — run it after indexing a KB and
before relying on it for supplier evaluation or proposal generation.

## When to use

- "Validate the knowledge base"
- "Check if the KB retrieval is working"
- "Run a quality check on the KB"
- After re-indexing the KB
- Before a critical evaluation or proposal run

## Workflow

### 1. Define test queries

Create 5-10 test queries that represent real questions the KB should
answer. For each query, note:
- **Query text**: the natural-language question
- **Expected documents**: which KB documents SHOULD be returned
- **Expected strength**: strong (the document directly answers the
  query) or weak (the document is tangentially related)

Include at least:
- 3-4 **positive queries** — questions the KB should answer well
  (e.g., "What is the pricing for SAP S/4HANA implementation?")
- 1-2 **guardrail queries** — questions that should NOT return strong
  matches from the wrong category (e.g., a pricing query should not
  return a marketing brochure)
- 2 **paraphrase queries** — ask for something a specific document
  covers, but in different words from the document (no shared key
  phrases). These test whether evidence is found by meaning, not by
  matching exact text.

### 2. Run each query against the KB

For each query, search the KB index (or read the KB documents if no
index exists). Record:
- Which documents were returned
- Relevance of each result (strong / weak / irrelevant)
- Whether the expected documents were found

### 3. Check coverage

Compute:
- **Positive coverage** = (positive queries that returned the
  expected document with strong relevance) / (total positive queries)
- **Target**: ≥ 90% positive coverage

### 4. Check guardrails

For each guardrail query:
- Did it return a strong match from the wrong category? If yes, that's
  a **guardrail failure** — the KB is returning false positives.

### 5. Check paraphrase queries

For each paraphrase query:
- PASS if the expected document is returned with strong relevance.
- FAIL if it is missed or returned only weakly.

Note: these skills have no separate search engine; Claude reads the
index and documents directly. This gate measures coverage and
labelling quality of the KB as Claude reads it. It does not certify
a vector or semantic retrieval system.

### 6. Report

Write a report to the output directory:

```
Query: <text>
Expected: <document>
Returned: <documents with relevance>
Status: PASS / FAIL

...

Summary:
  Positive coverage: X/Y (Z%)
  Guardrail failures: N
  Paraphrase: X/2 PASS
  Overall: PASS / FAIL
```

**PASS** requires:
- Positive coverage ≥ 90%
- Zero guardrail failures
- Both paraphrase queries pass

If any check fails, tell the operator what failed and suggest
possible causes (KB needs re-indexing, chunks too large/small,
category mislabeling, etc.)

## Notes

- This is a quality gate. If it fails, fix the KB before proceeding
  with evaluation or proposal generation.
- The test queries should reflect real usage. If the KB is for SAP
  proposals, query about SAP topics — not generic questions.
- Guardrail queries are the most important check. A KB that returns
  pricing documents for a methodology query will produce bad proposals.

## File access and persistence

- Before starting, confirm every input path exists and can be read.
  If a file or folder is missing, unreadable, or not attached, stop and
  tell the operator which one, and ask them to attach or re-share it.
  Never proceed on guessed or remembered content.
- If a document only partly parses (e.g. scanned PDF, password
  protected), list it as unreadable. Treat requirements that depend on
  it as Unknown, not as met or failed.
- Outputs may not persist between sessions (in Claude chat, files live
  only in the current conversation). Tell the operator where each
  output was written and that they should download or save anything
  they need later. A later session must not assume earlier outputs
  still exist; it should ask for them to be re-attached.
