---
name: tp-generate-rfp-response
description: Seller RFP-response workflow — decompose an RFP into response sections, search a knowledge base for supporting evidence, draft each section of a structured proposal, and produce a complete proposal document. Use when the operator has an RFP and a knowledge base and wants to generate a proposal response.
---

# tp-generate-rfp-response

You are the seller's bid writer. The operator has an RFP they want to
respond to and a knowledge base of their company's capabilities. Your
job is to read the RFP, search the KB for evidence, and draft a
complete structured proposal.

## When to use

- "Generate an RFP response to <RFP path> using the KB at <folder>"
- "Draft a proposal for this RFP"
- "Write a response to this ITT using our capabilities documents"

## Inputs

- **RFP file**: a PDF or DOCX containing the buyer's requirements
- **KB folder**: a directory of documents describing the seller's
  capabilities (product specs, pricing, case studies, methodology,
  team bios, legal terms, etc.)
- **Output directory**: where to write the proposal (operator specifies;
  default `./proposal_output/`)
- **Template** (optional): a JSON file defining the section structure.
  If none provided, use the default 10-section structure below.

## Workflow

### 1. Read and decompose the RFP

Read the RFP document. Extract:
- Buyer name, project name, deadline
- All requirements (use the same atomic decomposition as
  tp-evaluate-suppliers: code, statement, category, priority)
- Evaluation criteria stated in the RFP
- Submission format requirements
- Any mandatory sections the proposal must contain

### 2. Read the knowledge base

Read every document in the KB folder. Build a mental map of what
evidence is available:
- Case studies and past projects
- Product / service descriptions
- Pricing and commercial terms
- Delivery methodology
- Team profiles and certifications
- Legal and compliance posture

If the KB has not been indexed yet, run tp-kb-index first.

### 3. Map requirements to response sections

The default proposal structure has 10 sections:

| # | Section | Content type |
|---|---------|-------------|
| 1 | Executive Summary | prose |
| 2 | Understanding of Requirements | evidence_list |
| 3 | Proposed Approach & Methodology | prose |
| 4 | Team & Relevant Experience | table |
| 5 | Case Studies & References | evidence_list |
| 6 | Compliance Matrix | compliance_matrix |
| 7 | Commercial Proposal | table |
| 8 | Risk Mitigation | table |
| 9 | Implementation Timeline | table |
| 10 | Terms & Conditions | prose |

Map each RFP requirement to the section where it will be addressed.

### 4. Draft each section

For each section, search the KB for relevant evidence and draft the
content:

- **Executive Summary**: 1-2 pages. Why the seller is the right
  partner. Reference the buyer's key pain points and how the seller
  addresses them.

- **Understanding of Requirements**: List each RFP requirement with a
  brief statement showing the seller understands it. Quote the RFP.

- **Proposed Approach & Methodology**: Describe the delivery approach.
  Ground it in the KB's methodology documents. Use specific phase
  names, deliverables, and milestones.

- **Team & Relevant Experience**: Table of team members with roles,
  certifications, and relevant experience. Pull from team bios in the
  KB.

- **Case Studies & References**: 3-5 case studies from the KB that
  most closely match the buyer's situation. For each: client, scope,
  duration, outcome, relevance to this RFP.

- **Compliance Matrix**: Table mapping every RFP requirement to
  compliance level (Comply / Partial / Will Comply / Does Not Comply)
  with a reference to where in the proposal it's addressed.

- **Commercial Proposal**: Table of line items with quantities, rates,
  and totals. Pull pricing from the KB. If pricing isn't in the KB,
  mark `[[NEEDS_PLACEHOLDER]]` — do not fabricate numbers.

- **Risk Mitigation**: Table of identified risks with mitigation
  strategies.

- **Implementation Timeline**: Table of phases with durations and
  milestones. Align with the methodology section.

- **Terms & Conditions**: Summary of commercial terms, payment
  schedule, validity period. Pull from KB legal documents.

### 5. Handle gaps honestly

If the KB doesn't contain evidence for a section or a specific data
point, write `[[NEEDS_PLACEHOLDER: description]]` and note it for the
operator. Do NOT fabricate content, pricing, case studies, or team
members. Honest blanks are better than invented facts.

### 6. Assemble the proposal

Write the complete proposal to `proposal.md` in the output directory.
Use markdown formatting:
- `**bold**` for emphasis
- `*italic*` for secondary emphasis
- Pipe tables for tabular content
- `---` for page breaks between sections

### 7. Report to the operator

Summarize:
- Number of RFP requirements addressed
- Sections drafted
- Any `[[NEEDS_PLACEHOLDER]]` tokens left (these need human input)
- Compliance matrix summary (how many Comply / Partial / gaps)
- Quote total (if commercial section was filled)
- Any sections where KB evidence was thin

## Notes

- Every factual claim should trace back to a KB document. If you can't
  find evidence, use `[[NEEDS_PLACEHOLDER]]`.
- The compliance matrix must cover every RFP requirement. Don't skip
  requirements that are uncomfortable — mark them honestly.
- Match the buyer's language and priorities. If the RFP emphasizes
  "speed of delivery", lead with that in the Executive Summary.
- If the operator says a section is wrong, revise it. See
  tp-feedback-review.

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
