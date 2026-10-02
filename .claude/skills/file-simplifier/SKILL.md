---
name: file-simplifier
description: Convert a newly added non-markdown file (PDF, Word, PowerPoint, Excel, image, scan, HTML, ebook) into a compact, token-cheap .md twin, then move the original into "archive (to ignore)/originals/" so sessions only ever read the .md. Use whenever a new file is added, uploaded, attached or found without a .md twin, and whenever the user says convert, simplify, ingest, import, add this file or put this in the repo, even if they never mention markdown.
---

> **READ-ONLY FILE:** this file is read only, and any change to it requires the user's explicit approval beforehand.

# file-simplifier

Purpose: pay tokens once so every later session reads a small, clean `.md` instead of the original. After conversion, sessions use the `.md` only; the original goes to `archive (to ignore)/originals/`, which is for the user and never for a session (RULEBOOK A2).

## When
- A new file is added or attached, or the new-raw-file command in CLAUDE.md lists a file with no `.md` twin.
- Skip `.md`, `.txt`, source code and small json/csv/yaml: they are already cheap. Just index them.
- More than 5 files at once: ask before starting.

## Procedure
1. **Twin check.** If `<stem>.md` already exists (for example the user converted it elsewhere), do not convert again. If it lacks the frontmatter below, normalise it (step 3). Then go to step 5.
2. **Pick the route**, first that works, keeping the original out of your context whenever a tool can read it for you:
   - **a. Public URL:** WebFetch (built-in, nothing to set up).
   - **b. Local tool from "Trusted sources" Tier 1.** It runs inside the session VM, so the file never leaves it. Default for repo files. Run `markitdown --version` first and install only if missing.
   - **c. Built-in Read in chunks**, for scans, images and anything a tool garbled: PDFs with the `pages` parameter (up to 20 pages per read), images read directly. Write each chunk to the `.md` as you go. Most expensive route, so use it last.
   - **d. Third-party service from Tier 2**, only when it is reachable and the user approved sending this file (rules in Tier 2).
   - None works: say so and ask the user. Do not improvise.
3. **Rebuild** into the target format below.
4. **Verify before archiving**, without re-reading the original in full: section and page counts roughly match; every table and figure is accounted for; spot-check 5 numbers or names against the source pages. Mark gaps `[unclear: …]`. Never fill them.
5. **Index** it: add an entry to `navigation.md` using the `.md` frontmatter summary (repo-navigation skill).
6. **Archive** the original: move it into `archive (to ignore)/originals/` keeping its name (`git mv` if tracked, otherwise `mv`). On a name clash append `-YYYYMMDD`. Test only the exact target path; never list the folder.
7. **Report** in two lines: the new path, the size reduction, and any `[unclear]` items.

After step 6 the original is off limits. If the conversion turns out wrong, fix the `.md` from what it already contains, or tell the user so they can re-add the original.

## Target format
- **Path:** same folder, same stem (`report.pdf` becomes `report.md`). Output over about 600 lines: split into `report/00-index.md` and `report/01-<section>.md`, each 400 lines or fewer, each starting with a 2–3 line TL;DR.
- **Frontmatter:**
  ```yaml
  ---
  title: …
  summary: 1–2 sentences, in the words a user would ask with
  keywords: [main terms, synonyms]
  original: archive (to ignore)/originals/report.pdf   # name only, never open
  route: webfetch | local-tool | read-chunks | third-party | user-twin
  fidelity: high | medium | low (why)
  ---
  ```
- **Body:** `## TL;DR` with 3–5 bullets, then the content under real headings (H1–H3, no skipped levels).
- **Keep:** facts, figures, definitions, steps, decisions, exact numbers, names, dates, units, citations, tables (as markdown tables).
- **Drop:** headers and footers, page numbers, repeated legal boilerplate, TOC dot leaders, decorative images, cover fluff, empty cells.
- **Figures and charts:** one line, `[figure: what it shows + key values]`. Transcribe text inside an image only if it carries information.
- **Slides:** one `###` per slide, title plus bullets, speaker notes under `> notes:`.
- **Spreadsheets:** one `##` per sheet, header row plus rows as a table, formulas summarised in one line, values kept. Tables wider than about 12 columns: split them or list them row by row.
- **Scans and OCR:** mark low-confidence text `[?]`.
- **Fidelity:** compress wording, never meaning. Add no facts, opinions or commentary. Target 50% or less of the original's text size with no facts lost.

## Trusted sources
The user maintains this list. Use only what is listed here. Hosts marked "default" are already allowed by the cloud environment's Trusted network level, so nothing needs configuring. A 403 or 407 from a host means the environment blocks it: stop and report the host, never work around it.

### Tier 1: local tools (the file never leaves the VM)
Hosts, all default: `pypi.org`, `files.pythonhosted.org`, `archive.ubuntu.com`. Installs made mid-session do not carry over to other sessions.

| Tool | Install | Use for | Command |
| :- | :- | :- | :- |
| MarkItDown (Microsoft, MIT) | `pip install 'markitdown[all]'` | First choice: text-layer PDF, DOCX, PPTX, XLSX, HTML, CSV, JSON, XML, EPUB, ZIP | `markitdown "in.pdf" -o "in.md"` |
| pandoc via pypandoc-binary (MIT) | `pip install pypandoc-binary` | DOCX, EPUB, ODT, HTML when MarkItDown loses headings or lists | `python -c "import pypandoc; pypandoc.convert_file('in.docx','gfm',outputfile='in.md')"` |
| PyMuPDF4LLM (AGPL-3.0, VM use only) | `pip install pymupdf4llm` | Text PDFs with columns or tables that MarkItDown garbles. Advertises OCR for scanned pages (may need `apt-get install -y tesseract-ocr`) | `python -c "import pymupdf4llm,pathlib; pathlib.Path('in.md').write_text(pymupdf4llm.to_markdown('in.pdf'))"` |

Not covered: legacy `.doc` and `.ppt` (MarkItDown does not list them) and scans with no OCR. Use route c, or ask the user for a `.docx` or `.pptx` copy.

### Tier 2: third-party services (the file leaves the VM)
None is reachable by default; each needs the user's setup first.

| Service | Host | Use for | User setup |
| :- | :- | :- | :- |
| Mistral OCR (`mistral-ocr-latest`) | `api.mistral.ai` | Scans, images and complex tables to markdown per page. Takes PDF, PPTX, DOCX and images. Paid | API key stored as an environment API credential for this host |
| Cloudflare Workers AI toMarkdown | `api.cloudflare.com` | PDFs, HTML, images and more (its `/supported` endpoint lists formats). Most conversions free. `curl -X POST https://api.cloudflare.com/client/v4/accounts/$CF_ACCOUNT_ID/ai/tomarkdown -F "files=@in.pdf"` | Account ID plus API token stored as an API credential for this host (the proxy adds the Authorization header) |
| Jina Reader | `r.jina.ai` | Public URLs only (web pages, PDFs, Office files). Cannot open private repo files, so prefer WebFetch | Custom network access allowing `r.jina.ai` |

Tier 2 rules:
- Before the first send to a service in a session, ask "OK to send `<file>` to `<service>`?" Never send a file the user called private or confidential.
- Never print, log or write an API key into any file.
- Add nothing to this list yourself.

> **READ-ONLY FILE:** this file is read only, and any change to it requires the user's explicit approval beforehand.
