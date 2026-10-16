---
name: tp-kb-index
description: Prepare and index a knowledge base folder for RAG retrieval. Extract content from documents (PDF, DOCX, TXT, PPTX, XLSX, CSV), chunk into searchable sections, and create a structured index. Use when the operator asks to index a KB, build a search index, or prepare documents for evidence retrieval.
---

# tp-kb-index

Prepare a knowledge base folder so that its contents can be searched
for evidence during supplier evaluation or RFP response generation.

## When to use

- "Index the knowledge base at <folder>"
- "Prepare the KB for searching"
- "Build a search index from these documents"
- Before running tp-evaluate-suppliers or tp-generate-rfp-response

## Workflow

### 1. Scan the KB folder

Read every file in the target folder. Supported types: PDF, DOCX, TXT,
PPTX, XLSX, CSV. For each file, extract the text content and note:
- File name and path
- Document type
- Approximate length (pages / word count)
- Category if obvious from the filename or content (product specs,
  pricing, technical notes, case studies, legal, methodology, etc.)

### 2. Summarize each document

For each document, write a 3-5 sentence summary covering:
- What the document is (product overview, pricing sheet, case study, etc.)
- Key topics covered
- Notable data points (prices, dates, metrics, product names)

### 3. Chunk into searchable sections

Break each document into logical chunks of ~500-1000 words each. For
each chunk, record:
- Source file name
- Chunk number
- Section heading or topic label
- First line of content (for quick identification)

### 4. Build the index

Write a single index file (`kb_index.md` or `kb_index.json` in the
output directory) that maps topics to source chunks. Structure:

```
Topic → [source file, chunk number, summary]
```

Group entries by category (product specs, pricing, case studies, etc.)
so retrieval can filter by category.

### 5. Report

Tell the operator:
- Number of documents indexed
- Total chunks created
- Categories identified
- Any documents that could not be parsed (and why)

## Output

- `kb_index.md` (or `.json`) — the structured index
- `kb_summaries/` — one summary file per source document

The operator specifies the output directory. If none is given, use
`./kb_index/` relative to the KB folder.

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
