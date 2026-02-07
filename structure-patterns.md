# Skill Structure Patterns

Detailed examples of the four main structural patterns for organizing a skill's SKILL.md body.

## Pattern 1: Workflow-Based

Best for: Sequential processes where order matters.

```markdown
# Document Processing Skill

## Overview
Converts uploaded documents into clean, formatted outputs.

## Workflow Decision Tree

Is the input a .doc or .docx?
├── .doc → Convert to .docx first (see Converting Legacy Files)
└── .docx → Proceed to analysis

What does the user want?
├── Read/extract content → See Step 1
├── Create new document → See Step 2
└── Edit existing document → See Step 3

## Step 1: Reading Content
[Instructions for reading...]

## Step 2: Creating Documents
[Instructions for creating...]

## Step 3: Editing Documents
[Instructions for editing...]

## Validation
[How to verify the output is correct]
```

**When to use:** The task has a clear beginning, middle, and end. Steps depend on previous steps. Examples: document pipelines, deployment workflows, data migrations.

---

## Pattern 2: Task-Based

Best for: Skills that offer distinct, independent operations.

```markdown
# PDF Toolkit

## Overview
Complete toolkit for PDF manipulation.

## Quick Start
For simple tasks, use these one-liners:
- Extract text: `python scripts/extract.py input.pdf`
- Merge files: `python scripts/merge.py file1.pdf file2.pdf`

## Extracting Text
[Detailed instructions...]

## Merging PDFs
[Detailed instructions...]

## Splitting PDFs
[Detailed instructions...]

## Filling Forms
[Detailed instructions...]

## Adding Watermarks
[Detailed instructions...]
```

**When to use:** The skill is a collection of related but independent capabilities. Users will typically want one operation per invocation. Examples: file format tools, API wrappers, utility collections.

---

## Pattern 3: Reference/Guidelines

Best for: Standards, specifications, or style guides the model should follow.

```markdown
# Brand Voice Skill

## Overview
Apply the company's brand voice to all written content.

## Voice Principles
- Confident but not arrogant
- Technical but accessible
- Warm but professional

## Tone by Context

| Context | Tone | Example |
|---------|------|---------|
| Blog post | Conversational, curious | "We've been thinking about..." |
| Documentation | Clear, direct | "To configure X, set Y to Z." |
| Error message | Empathetic, helpful | "Something went wrong. Here's how to fix it." |

## Vocabulary

**Preferred terms:**
- "people" not "users"
- "help" not "assist"
- "build" not "construct"

**Avoid:**
- Jargon without explanation
- Passive voice in instructions
- Exclamation marks in formal content
```

**When to use:** The skill encodes a standard that should be applied consistently. There's no sequential workflow — it's a set of rules and examples to follow. Examples: brand guidelines, coding standards, review checklists.

---

## Pattern 4: Capabilities-Based

Best for: Integrated systems with interrelated features.

```markdown
# Data Analysis Skill

## Overview
Turn raw data into actionable insights with automated analysis.

## Core Capabilities

### 1. Data Profiling
Automatically assess data quality, types, distributions, and anomalies.
[Details...]

### 2. Statistical Analysis
Run appropriate statistical tests based on data characteristics.
[Details...]

### 3. Visualization
Generate publication-quality charts matched to the data type.
[Details...]

### 4. Report Generation
Combine profiling, analysis, and visuals into a coherent narrative.
[Details...]

## How Capabilities Interact
Profiling informs which statistical tests to run. Statistical results
determine which visualizations are most informative. All three feed
into the final report.
```

**When to use:** The skill's features build on each other or work together. The user may invoke any combination. Examples: analytics platforms, content management, project management.

---

## Mixing Patterns

Most production skills combine patterns. Common combinations:

- **Task-based + Workflow for complex operations:** Top-level is task-based (separate sections per operation), but individual operations follow a workflow pattern internally.

- **Capabilities + Reference:** Core capabilities section describes what the skill can do, followed by a reference section with standards and guidelines to follow.

- **Workflow + Task-based fallback:** A primary workflow for the happy path, with task-based sections for edge cases and special operations.

---

## Domain Organization

When a skill supports multiple variants (e.g., cloud providers, languages, frameworks), organize with a routing section in SKILL.md and separate reference files:

```
cloud-deploy/
├── SKILL.md
│   └── Contains: decision tree for selecting the right platform
│       and instructions to read the appropriate reference file
└── references/
    ├── aws.md      (AWS-specific instructions)
    ├── gcp.md      (GCP-specific instructions)
    └── azure.md    (Azure-specific instructions)
```

This way Claude only loads the relevant reference file, keeping context lean.
