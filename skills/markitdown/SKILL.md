---
name: markitdown
description: Convert rich/binary documents (PDF, Word .docx, PowerPoint .pptx, Excel .xlsx, images, audio, HTML, CSV/JSON/XML, EPub, ZIP) into clean Markdown via the MarkItDown MCP server, so the agent can read their content. Use whenever the user asks to read, open, extract, summarize, or answer questions about the contents of a file that is NOT plain text/code — especially PDFs, Office documents, images, or audio.
---

# MarkItDown — read any document as Markdown (via MarkItDown MCP)

Use the **MarkItDown MCP** server (`markitdown-mcp`, by Microsoft) to turn documents the plain
`Read` tool can't parse into Markdown the agent can consume.

## When to use
- User: "read / open / what's in this PDF / .docx / .pptx / .xlsx", "extract the text from this image",
  "summarize this document", "what does this file say".
- Any time the target is a **rich or binary format**: PDF, Word, PowerPoint, Excel, images
  (OCR + metadata), audio (transcription + metadata), HTML, CSV/JSON/XML, EPub, ZIP.

**Not for plain text/code/markdown** — read those directly with the normal `Read` tool. Reach for
MarkItDown only when the format needs decoding.

## Setup (once)
```bash
pip install markitdown-mcp==0.0.1a4     # pinned version for reproducibility
claude mcp add markitdown markitdown-mcp
```
Or MCP config JSON (direct command — simplest for local files):
```json
{
  "mcpServers": {
    "markitdown": {
      "command": "markitdown-mcp"
    }
  }
}
```
Docker alternative (note: `file:` URIs require mounting the file into the container):
```json
{
  "mcpServers": {
    "markitdown": {
      "command": "docker",
      "args": ["run", "--rm", "-i", "markitdown-mcp:latest"]
    }
  }
}
```

## Usage
Single tool: **`convert_to_markdown(uri)`**. Accepts URI schemes: `file:`, `http:`, `https:`, `data:`.

- Local file → `file:///abs/path/to/report.pdf`
- Remote → `https://example.com/deck.pptx`

Workflow: build the `file://` URI from the absolute path → call `convert_to_markdown` → work with
the returned Markdown (summarize, extract, answer). For large docs, convert once and reuse the result
instead of re-converting.

## Rules
- Plain-text/code/`.md` → use `Read`, not MarkItDown.
- Always pass an **absolute** path in the `file://` URI.
- If Docker mode fails on a local file, it's usually because the file isn't mounted — prefer the
  pip/direct-command setup for local document reading.
