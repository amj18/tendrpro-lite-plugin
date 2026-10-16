# TendrPro Lite — Skills Pack

![TendrPro Lite logo](assets/logo.png)

A pack of 7 self-contained skills for buyer supplier-evaluation and
seller RFP-response workflows. Skills guide the LLM through
decomposing RFPs into atomic requirements, matching evidence from
supplier proposals and knowledge bases, scoring with weighted mandatory
gates, generating structured proposal documents, composing email
drafts, validating knowledge base quality, reviewing prior work, and
self-correcting from operator feedback. No external engine or scripts
required — the LLM executes every step inline using its native tools.

## Skills

| Skill | Purpose |
|-------|---------|
| `tp-kb-index` | Prepare and index a knowledge base for retrieval |
| `tp-evaluate-suppliers` | Buyer: decompose RFP, score suppliers, produce award docs |
| `tp-generate-rfp-response` | Seller: draft a structured proposal from KB evidence |
| `tp-email-draft` | Compose a professional email to accompany outputs |
| `tp-regression-gate` | Validate knowledge base retrieval quality |
| `tp-brain-recall` | Review prior work and apply corrections before a new run |
| `tp-feedback-review` | Capture operator feedback, revise outputs, record corrections |

## Install

### Claude Code (own marketplace)

```bash
claude plugin marketplace add amj18/tendrpro-lite-plugin
claude plugin install tendrpro-lite@tendrpro-lite-marketplace
```

Or submit to the Anthropic directory from
https://claude.ai/directory/manage (requires a paid Claude plan and a
public GitHub repo).

### ChatGPT / Codex

Zip the plugin folder and submit at the OpenAI plugin portal. See
https://developers.openai.com/plugins/deploy/submission for the current
submission flow.

## How it works

Each skill is a markdown workflow guide. When the operator's request
matches a skill's description, the LLM loads the skill and follows its
steps inline — reading documents, decomposing requirements, matching
evidence, computing scores, and writing outputs to disk. No Python
scripts, no database, no external API calls. The skills work on any
Claude or ChatGPT installation that supports the plugin format.

## What the plugin reads, writes and sends

- **Reads** only the files and folders the operator points it at
  (RFPs, supplier proposals, knowledge-base documents, prior outputs).
- **Writes** output files to the directory the operator names, or next
  to the inputs if none is given: evaluation reports, scoring tables,
  supplier letters, proposal drafts, email drafts, a `kb_index/`
  folder with `kb_index.md` and `kb_summaries/`, regression-gate
  reports, and feedback/correction notes.
- **Sends** nothing anywhere. There are no scripts, hooks, MCP servers,
  network calls or telemetry. Emails are drafted as text only and are
  never sent.

Scoring follows the RFP's own evaluation rules when it states them.
Missing evidence is marked Unknown and flagged for clarification; it
never disqualifies a supplier by itself. Prior corrections are only
re-applied when the requirement and evidence are unchanged.

## License

Free to use, including at work and inside companies. You may not sell, resell or bundle the skills into a paid product or service. For commercial licensing, contact the owner. See `LICENSE`.