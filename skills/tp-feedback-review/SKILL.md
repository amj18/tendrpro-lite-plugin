---
name: tp-feedback-review
description: Walk through operator feedback on an evaluation or proposal, capture what was wrong and what the correction is, revise the output, and record the correction for future runs. Use when the operator says the output is wrong, disagrees with a compliance assignment, or wants to correct a score, section, or finding.
---

# tp-feedback-review

When the operator says the output is wrong, walk through each issue
systematically, capture the correction, revise the output, and record
it so the same mistake isn't repeated next time.

## When to use

- "The evaluation is wrong — Supplier 3 should be Non-compliant on
  multi-entity"
- "The proposal quote total is incorrect"
- "I disagree with the compliance matrix"
- "Section 3 is missing the agile approach detail"
- "Supplier 8 should be disqualified"
- Any time the operator indicates the output has an error

## Workflow

### 1. Identify what's wrong

Ask the operator (or parse their statement) to identify:
- **Which output** is wrong (evaluation, proposal, email, etc.)
- **Which specific item** is wrong (a compliance assignment, a score,
  a section, a quote total, a shortlist ranking, etc.)
- **What it currently says** (the incorrect value/assignment)
- **What it should say** (the correction)

If the operator gave multiple issues, list them all before proceeding.

### 2. Walk through each issue

For each issue, one at a time:

1. **Show the current value**: "Currently, Supplier 3 is marked as
   Comply on R005 (Multi-entity, multi-currency experience)."

2. **Confirm the correction**: "You said this should be
   Non-compliant. Is that correct? What's the reason?"

3. **Capture the reason**: Record why the correction is needed (e.g.,
   "Supplier 3 only has single-entity experience per their proposal,
   section 4.2"). The reason is important — it explains the correction
   and helps apply it in future runs.

4. **Apply the correction**: Revise the output:
   - For a compliance assignment: change the level, recompute the
     supplier's score, check if disqualification status changes,
     update the shortlist and award recommendation if needed.
   - For a proposal section: rewrite the section with the correction.
   - For a quote total: update the commercial section and recompute.
   - For a shortlist ranking: re-rank and update the award memo.

### 3. Check downstream impacts

After applying a correction, check if it affects other parts of the
output:
- Does a score change affect the shortlist?
- Does a disqualification change the award recommendation?
- Does a compliance change affect the compliance matrix?
- Does a quote change affect the commercial summary?

Update all affected sections.

### 4. Rewrite the output

Regenerate the affected output files with the corrections applied.
Write them to the same output directory, overwriting the previous
versions.

### 5. Record the correction

Write the correction to `corrections.md` in the output directory so
tp-brain-recall can find and apply it in future runs:

```
## Correction — <date>

Output: <evaluation/proposal name>
Item: <what was corrected>
Was: <old value>
Now: <new value>
Reason: <why>
```

### 6. Report to the operator

Summarize:
- What was corrected
- What downstream impacts were updated
- The revised result (new score, new ranking, new shortlist, etc.)
- Confirmation that the correction is recorded for future runs

## Notes

- Always apply downstream impacts. A single compliance change can
  flip the award recommendation. Don't just patch one cell —
  recompute everything affected.
- The reason for the correction is as important as the correction
  itself. Without the reason, the same mistake can't be prevented
  next time.
- If the operator gives multiple corrections, process them one at a
  time and show the cumulative impact at the end.
- Never argue with the operator's correction. They are the source of
  truth. Apply it, note the reason, and move on.
- If a correction contradicts a prior correction, ask the operator
  which one takes precedence.

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
