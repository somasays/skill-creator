# Description Examples

Real-world examples of effective skill descriptions, analyzed for what makes them work.

## Example 1: DOCX Skill

```yaml
description: >
  Use this skill whenever the user wants to create, read, edit, or
  manipulate Word documents (.docx files). Triggers include: any mention
  of "Word doc", "word document", ".docx", or requests to produce
  professional documents with formatting like tables of contents,
  headings, page numbers, or letterheads. Also use when extracting or
  reorganizing content from .docx files, inserting or replacing images
  in documents, performing find-and-replace in Word files, working with
  tracked changes or comments, or converting content into a polished
  Word document. If the user asks for a "report", "memo", "letter",
  "template", or similar deliverable as a Word or .docx file, use this
  skill. Do NOT use for PDFs, spreadsheets, Google Docs, or general
  coding tasks unrelated to document generation.
```

**Why it works:**
- Opens with the core capability (create/read/edit/manipulate)
- Lists exact trigger phrases users actually say ("Word doc", ".docx")
- Includes secondary triggers (find-and-replace, tracked changes)
- Maps user vocabulary to skill scope ("report", "memo", "letter")
- Clear exclusions prevent false positives

---

## Example 2: Frontend Design Skill

```yaml
description: >
  Create distinctive, production-grade frontend interfaces with high
  design quality. Use this skill when the user asks to build web
  components, pages, artifacts, posters, or applications (examples
  include websites, landing pages, dashboards, React components,
  HTML/CSS layouts, or when styling/beautifying any web UI). Generates
  creative, polished code and UI design that avoids generic AI aesthetics.
```

**Why it works:**
- Leads with the value proposition ("distinctive, production-grade")
- Lists concrete output types (websites, landing pages, dashboards)
- Includes non-obvious triggers ("posters", "beautifying any web UI")
- States what makes this skill special (avoids generic AI aesthetics)

---

## Example 3: PDF Skill

```yaml
description: >
  Use this skill whenever the user wants to do anything with PDF files.
  This includes reading or extracting text/tables from PDFs, combining
  or merging multiple PDFs into one, splitting PDFs apart, rotating
  pages, adding watermarks, creating new PDFs, filling PDF forms,
  encrypting/decrypting PDFs, extracting images, and OCR on scanned
  PDFs to make them searchable. If the user mentions a .pdf file or
  asks to produce one, use this skill.
```

**Why it works:**
- Extremely broad trigger ("anything with PDF files")
- Exhaustive list of operations — leaves nothing to interpretation
- Catches the passive case ("mentions a .pdf file")
- Short, punchy final sentence as a catch-all

---

## Example 4: Skill Creator

```yaml
description: >
  Create new skills, improve existing skills, and measure skill
  performance. Use when users want to create a skill from scratch,
  update or optimize an existing skill, run evals to test a skill,
  benchmark skill performance with variance analysis, or optimize a
  skill's description for better triggering accuracy.
```

**Why it works:**
- Three clear capability areas (create, improve, measure)
- Lists specific sub-tasks under each area
- Includes technical terms for power users ("evals", "benchmark")

---

## Anti-Patterns

### Too vague
```yaml
description: Helps with documents.
```
**Problem:** Could match almost anything. No trigger phrases. Claude won't activate.

### Too narrow
```yaml
description: Converts .docx files to PDF format using LibreOffice.
```
**Problem:** Only matches one specific sub-task. Misses creating, editing, reading docs.

### Missing exclusions
```yaml
description: Use for any file creation or editing task.
```
**Problem:** Would trigger on spreadsheets, code files, images — way too broad. Needs explicit exclusions.

### Technical jargon only
```yaml
description: Orchestrates pandoc-based document pipeline with XML manipulation.
```
**Problem:** Users don't say "pandoc" or "XML manipulation." Descriptions must use user vocabulary, not implementation details.

---

## Formula for Writing Descriptions

```
[Core capability in 1 sentence] +
[Primary triggers with user vocabulary] +
[Secondary/non-obvious triggers] +
[Concrete examples in parentheses] +
[Exclusions to prevent false positives]
```

Target length: 50-150 words. Shorter descriptions miss triggers. Longer descriptions dilute the signal.
