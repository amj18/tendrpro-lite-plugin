---
name: tp-brain-recall
description: Review prior work and context before starting a new task. Read previous evaluation or proposal outputs, identify lessons learned and corrections from earlier runs, and consolidate the relevant context. Use at the start of any evaluation or proposal run to avoid repeating mistakes and to apply prior corrections.
---

# tp-brain-recall

Before starting a new evaluation or proposal run, review what was done
previously and surface anything relevant: prior outputs, corrections
the operator gave, lessons learned. This prevents repeating mistakes
and ensures prior feedback is applied.

## When to use

- Automatically at the start of tp-evaluate-suppliers or
  tp-generate-rfp-response
- "Recall prior work on this RFP"
- "What did we learn from the last evaluation?"
- "Check for corrections from previous runs"

## Workflow

### 1. Identify the target

Determine what the upcoming run is about:
- Which RFP?
- Which buyer / project?
- Which KB?
- Which suppliers (if evaluation)?

### 2. Look for prior outputs

Search the output directory (and any subdirectories) for:
- Previous evaluation results for the same RFP or buyer
- Previous proposals for the same RFP or project
- Scorecards, award documents, shortlists
- Proposal drafts and their revisions
- Any `corrections.md` or `feedback.md` files

### 3. Read and consolidate

For each prior output found:
- **What was done**: brief summary of the prior run
- **What the outcome was**: scores, recommendations, key findings
- **What was corrected**: any feedback or corrections the operator
  gave (look for revision notes, comments in the document, or
  dedicated correction files)
- **What to apply this time**: specific things to do differently based
  on prior corrections

### 4. Present the context

Summarize for the operator (and for yourself to apply during the
upcoming run):

```
Prior work found:
  - <output name> (<date>)
    Outcome: <brief>
    Corrections: <list>
    Apply this time: <specific actions>

Relevant corrections to apply:
  1. <correction 1>
  2. <correction 2>
```

### 5. Apply during the upcoming run

When the evaluation or proposal generation starts, apply the
corrections from step 4. For example:
Before applying any prior correction, check it still holds:
- Is the requirement wording the same in the current RFP?
- Is the supplier's evidence (proposal version, pricing, documents)
  unchanged since the correction was made?
- For figures such as pricing or quote totals, is the source data
  unchanged?

If all three hold, apply the correction and cite it in the output.
If anything changed, or you can't tell, do NOT apply it automatically.
List it as "prior correction — needs confirmation" and ask the
operator. For example: a prior run marked Supplier X as Comply, the
operator corrected it to Non-compliant, and the supplier has since
submitted a revised proposal. Re-evaluate against the new proposal and
show the prior correction for reference.

## Notes

- This skill is about avoiding repeated mistakes, not about replaying
  old outputs. The goal is to carry forward corrections, not to copy
  prior work.
- If no prior work exists for this target, report "No prior work
  found" and proceed with a clean slate.
- Operator corrections take priority over your own judgement, but
  only for the same requirement and the same evidence they were made
  against. Never carry a correction across changed inputs silently.
- If a prior output was revised multiple times, the latest revision
  is the source of truth.

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
