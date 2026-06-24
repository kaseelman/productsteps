# Inbox

This folder is the raw drop zone — a holding area for unprocessed content before it gets structured and moved into the right place.

## What to drop here

- Raw meeting transcripts or AI-generated summaries you haven't reviewed yet
- Exported documents (Notion exports, Google Docs, PDFs converted to markdown)
- Slack threads worth saving
- Links with a short note about why they're relevant
- Interview transcripts or research snippets before synthesis

## What NOT to drop here

- Processed, structured content — that goes directly into `context/` or `domains/<domain>/`
- Code, configs, or anything non-product-knowledge
- Large binary files or raw media exports

## Triage process

When you're ready to process inbox content:

1. **In Claude Code**: run `/inbox-triage` (skill coming soon) — Claude will read each inbox file, suggest where it belongs, and draft a structured summary you can commit.
2. **Manually**: read the file, create a structured note using the right template from `docs/templates/`, commit it to the correct folder, then delete or archive the raw inbox file.

Files in `inbox/` are treated as **lower-confidence signal** by Claude — they are not assumed to reflect current team thinking until processed and moved.
