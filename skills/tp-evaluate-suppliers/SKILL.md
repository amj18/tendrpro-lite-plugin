---
name: tp-evaluate-suppliers
description: Buyer supplier-evaluation workflow — decompose an RFP into atomic requirements, read supplier proposals, match evidence against each requirement, assign compliance, compute weighted scores with a mandatory gate, and produce an award recommendation, shortlist, and supplier letters. Use when the operator has an RFP and one or more supplier proposals and wants them scored and ranked.
---

# tp-evaluate-suppliers

You are the buyer's assistant. The operator has an RFP (or ITT) and a
set of supplier proposals. Your job is to decompose the RFP into
requirements, evaluate each supplier against those requirements, score
them, and produce the award documents.

## When to use

- "Evaluate the suppliers in <folder> against the RFP at <path>"
- "Score these supplier proposals against the RFP"
- "Produce an award recommendation and shortlist"
- "Which supplier should we award to?"

## Inputs

- **RFP file**: a PDF or DOCX containing the buyer's requirements
- **Supplier proposals**: a folder of PDFs/DOCXs, one per supplier
- **Output directory**: where to write results (operator specifies;
  default `./evaluation_output/`)

## Workflow

### 1. Read and decompose the RFP

Read the RFP document. Extract every requirement as an atomic item:

- **Code**: R001, R002, R003, ...
- **Statement**: one sentence describing the requirement
- **Category**: Technical, Commercial, Methodology, Team, Compliance,
  Timeline, Legal, Other
- **Priority**: Mandatory, Preferred, Optional
- **Evidence needed**: what a supplier must show to comply

Be thorough. A typical RFP yields 20-60 requirements. Don't merge
distinct requirements into one entry.

### 2. Read each supplier proposal

For each supplier file, read the full document. Note:
- Supplier name (from filename or document header)
- Overall structure and tone
- Key claims and evidence provided
- Pricing details
- Any missing sections

### 3. Match evidence per requirement

For each supplier × each requirement:
- Search the supplier's proposal for evidence addressing the requirement
- Assign a compliance level:
  - **Comply** — explicit evidence that the requirement is met
  - **Partial** — some evidence but gaps or caveats
  - **Non-compliant** — the proposal contains explicit evidence that the
    requirement is NOT met (e.g. the supplier states it does not offer
    the capability, or quotes terms that contradict the requirement)
  - **Unknown** — no evidence either way: the proposal is silent, or the
    relevant document could not be read
- Absence of evidence is always **Unknown**, never Non-compliant. Only
  affirmative evidence of non-compliance may be marked Non-compliant.
- Quote the specific passage that supports the assignment (with page
  or section reference if available)

### 4. Score each supplier

**Use the RFP's own evaluation rules first.** If the RFP states
evaluation criteria, weightings, scoring scales, pass/fail gates or a
price/quality split, apply those exactly and record that you did.
Use the default scheme below only when the RFP defines none, and say
in the outputs that a default scheme was used.

Default scheme (only when the RFP gives no scoring rules):

- **Mandatory requirements**: weight × 2
  - Any Mandatory requirement marked Non-compliant → supplier is
    **disqualified** (regardless of overall score)
  - Any Mandatory requirement marked Unknown → supplier is **flagged
    for clarification**, not disqualified. List the open items so the
    operator can raise clarification questions before award.
- **Preferred requirements**: weight × 1
- **Optional requirements**: weight × 0.5

Scoring per requirement:
- Comply = 1.0
- Partial = 0.5
- Non-compliant = 0
- Unknown = 0

Compute:
- Raw score = sum of (weight × compliance_value) for all requirements
- Max possible = sum of all weights
- **Weighted score %** = (raw / max) × 100
- **Disqualified flag** = true if any Mandatory Non-compliant
- **Clarification flag** = true if any Mandatory Unknown (and not disqualified)

### 5. Rank and shortlist

Rank suppliers by weighted score, with disqualified suppliers at the
bottom regardless of score. Do not recommend award to a supplier with
an open clarification flag; recommend clarification first. Produce a shortlist of the top 3-5
non-disqualified suppliers.

### 6. Generate outputs

Write the following to the output directory:

- **`award_recommendation.md`** — the award memo:
  - Recommended supplier (highest non-disqualified score)
  - Summary of why they won
  - Full ranking table
  - Key strengths and weaknesses of the top 3
  - Risk notes

- **`shortlist.md`** — ranked shortlist with scores and
  qualification status

- **`supplier_letter_<name>.md`** — one per supplier:
  - APPROVED (shortlisted) or REJECTED (not shortlisted) or PENDING
  - Summary of their evaluation
  - Key compliance gaps (for rejected suppliers)

- **`scorecard_<name>.md`** — one per supplier:
  - Full requirement-by-requirement breakdown
  - Compliance level + evidence quote per requirement
  - Overall score

### 7. Report to the operator

Summarize:
- Number of requirements decomposed
- Number of suppliers evaluated
- Any suppliers disqualified and why
- The top 3 ranked suppliers with scores
- The recommended award supplier
- Any requirements where evidence was thin across all suppliers

## Notes

- Ground every compliance assignment in a specific quote from the
  proposal. If you can't find evidence, mark Unknown (not Non-compliant) —
  don't infer compliance from tone or marketing language.
- Be honest about gaps. A supplier with a high score but a Mandatory
  gap is disqualified. Report this clearly.
- If the operator disagrees with a compliance assignment, revise it
  and recompute the scores. See tp-feedback-review.

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
