---
name: tp-email-draft
description: Compose a professional email to accompany a proposal or evaluation document. Drafts the email body with subject, greeting, body, sign-off, and notes the attachment. Use when the operator needs an email to send with a generated proposal, award notification, or supplier letter.
---

# tp-email-draft

Compose a professional email to accompany a proposal, evaluation
result, or supplier letter. The email is a draft — the operator
reviews and sends it manually.

## When to use

- "Draft an email to accompany the proposal"
- "Write a cover email for this RFP response"
- "Compose a supplier notification email"
- "Draft an award letter email"

## Inputs

- **Attachment**: the document being sent (proposal, award memo,
  supplier letter, evaluation report)
- **Recipient**: buyer, supplier, or internal stakeholder
- **Context**: what was generated and why
- **Tone**: formal (default), warm, firm

## Workflow

### 1. Identify the email type

- **Proposal submission** — sending a proposal to a buyer
- **Award notification** — notifying the winning supplier
- **Rejection notification** — notifying a non-shortlisted supplier
- **Shortlist notification** — informing a supplier they're shortlisted
- **Internal summary** — sending evaluation results to a colleague

### 2. Draft the subject line

- Proposal submission: `Proposal Submission: <Project Name> — <Seller Name>`
- Award: `Contract Award: <Project Name>`
- Rejection: `Update on <Project Name> — <Buyer Name>`
- Shortlist: `Shortlist Notification: <Project Name>`
- Internal: `Evaluation Results: <Project Name>`

### 3. Draft the body

**Structure:**
1. **Greeting**: professional, using recipient's name if known
2. **Context**: 1-2 sentences on what this email is about and why
3. **Key points**: 3-5 bullet points or short paragraph covering:
   - What is attached
   - Key highlights (proposal strengths, evaluation outcome, next steps)
   - Deadline or action required (if any)
4. **Closing**: offer to answer questions, next steps
5. **Sign-off**: professional closing + sender name

**Tone guidelines:**
- Proposal submission: confident, professional, brief
- Award: warm, congratulatory, clear on next steps
- Rejection: respectful, brief, honest but not detailed about why
- Shortlist: encouraging, clear on what happens next
- Internal: concise, factual, highlight key findings

### 4. Note the attachment

End the email with:
```
Attachment: <filename>
```

### 5. Present the draft

Show the complete email to the operator. Ask if they want any changes
before they send it.

## Notes

- This skill produces a draft only. The operator reviews, copies it
  into their email client, and sends manually.
- Never auto-send. Never include sensitive information the operator
  hasn't explicitly approved.
- Keep proposal submission emails to under 200 words. The proposal
  speaks for itself.
- For rejection emails, be respectful but brief. Don't provide a
  detailed critique unless the operator asks for it.
- If the operator says the email is wrong, revise it. See
  tp-feedback-review.