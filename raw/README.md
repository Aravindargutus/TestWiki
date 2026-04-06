# Raw Sources

This directory holds immutable source documents. The LLM reads from here but **never modifies** these files.

## How to add sources

1. Save articles, papers, notes, transcripts, or any document as markdown (or other format) into this directory
2. Use `raw/assets/` for images, PDFs, and data files
3. Tell the LLM: "ingest `raw/filename.md`"

## Tips

- **Obsidian Web Clipper** converts web articles to markdown — great for quickly capturing sources
- Keep original filenames descriptive
- One source per file works best
- The LLM will create a summary page in `wiki/sources/` for each ingested file
