---
name: create-skills
description: Use this skill whenever the user wants to create a new Claude skill, write a SKILL.md file, turn a workflow into a reusable skill, package instructions for Claude to follow, or build a custom automation that Claude can trigger. Also use when the user says "make this a skill", "create a skill for X", "I want Claude to always do X", or asks how skills work. Covers skill anatomy, writing patterns, description optimization, progressive disclosure, testing, and packaging. Even if the user just mentions wanting to standardize a workflow or create reusable instructions, this skill applies.
---

# Creating Claude Skills

Skills are reusable instruction sets that teach Claude how to perform specific tasks consistently and well. A skill is a folder with a `SKILL.md` file and optional supporting resources. When triggered, Claude reads the SKILL.md and follows its instructions.

Think of a skill as programming Claude's behavior using precise natural language — where the LLM is the interpreter and the skill is the source code.

## When to Create a Skill

Create a skill when:
- A task needs **consistent, repeatable execution** across many conversations
- The task involves **specific tools, sequences, or output formats** that Claude wouldn't know by default
- You've iterated on a workflow manually and want to **capture what works**
- The instructions are too detailed to repeat every time but too important to leave to chance

Don't create a skill when:
- A simple prompt gets the job done
- The task is a one-off
- The behavior is already built into Claude

## Workflow

```
1. Capture Intent → 2. Research & Interview → 3. Draft SKILL.md →
4. Test with real prompts → 5. Refine → 6. Package & deliver
```

---

## Step 1: Capture Intent

Start by understanding what the user wants. Answer these four questions:

1. **What should the skill enable Claude to do?**
   Get specific: "Create pixel-perfect Word documents with corporate letterhead" is better than "make docs."

2. **When should it trigger?**
   What phrases, file types, or contexts should activate this skill? Be specific — undertriggering is the most common failure mode.

3. **What's the expected output?**
   A file? A code snippet? A structured response? Define the deliverable clearly.

4. **Are the outputs objectively verifiable?**
   File transforms, data extraction, and code generation can be tested. Writing style and creative work usually can't. This determines whether to set up test cases.

If the current conversation already contains a workflow the user wants to capture (e.g., "turn this into a skill"), extract answers from the conversation history — the tools used, the sequence of steps, corrections made, input/output formats observed. Fill gaps with targeted questions.

---

## Step 2: Skill Anatomy

Every skill follows this structure:

```
skill-name/
├── SKILL.md              ← Required. The instruction file.
│   ├── YAML frontmatter   (name + description — always in context)
│   └── Markdown body      (instructions — loaded when skill triggers)
│
└── Optional resources/
    ├── scripts/           Executable code for deterministic tasks
    ├── references/        Docs loaded into context as needed
    └── assets/            Templates, icons, fonts used in output
```

**Do not include**: README.md, INSTALLATION_GUIDE.md, CHANGELOG.md, or any documentation for humans. Skills are read by AI agents, not people.

### Progressive Disclosure (Three-Level Loading)

Skills load information in layers to manage context efficiently:

| Level | What | When loaded | Size guidance |
|-------|------|-------------|---------------|
| **1. Metadata** | `name` + `description` in YAML frontmatter | Always in context | ~100 words |
| **2. SKILL.md body** | The full markdown instructions | When skill triggers | <500 lines ideal |
| **3. Bundled resources** | Scripts, references, assets | On-demand via explicit read | Unlimited |

The description is the most important part — it's the **trigger mechanism**. The body is the playbook. Resources are the deep reference material.

---

## Step 3: Writing the YAML Frontmatter

### The `name` Field

A short identifier. Lowercase, hyphenated. Examples: `docx`, `pdf`, `frontend-design`, `deploy-aws`.

### The `description` Field (Critical)

The description is the **primary trigger mechanism**. Claude decides whether to use a skill almost entirely based on the description matching the user's request. A bad description means the skill never fires.

**Common failure: undertriggering.** Claude tends to be conservative about activating skills. Combat this by making descriptions slightly "pushy" — explicitly list scenarios where the skill applies, even ones that seem obvious.

**Template:**

```yaml
description: >
  [What the skill does — 1 sentence].
  Use this skill when [primary triggers].
  Also use when [secondary triggers that might be missed].
  Covers [key capabilities].
  Do NOT use for [explicit exclusions to prevent false triggers].
```

**Good example:**

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
- Lists the primary trigger phrases explicitly
- Includes secondary triggers that could be missed
- Has exclusions to prevent false positives
- Mentions the user's likely vocabulary, not just technical terms

**Bad example:**

```yaml
description: Creates Word documents.
```

**Why it fails:** Too vague, no trigger phrases, no exclusions. Claude won't know when to activate it.

**Description writing checklist:**
- [ ] States what the skill does in the first sentence
- [ ] Lists explicit trigger phrases the user might say
- [ ] Includes secondary/non-obvious trigger scenarios
- [ ] Mentions relevant file types or extensions
- [ ] Has exclusions to prevent false triggers
- [ ] Is slightly "pushy" to counteract undertriggering

---

## Step 4: Writing the SKILL.md Body

### Structure Patterns

Choose the structure that fits the skill's purpose. Most skills combine patterns:

| Pattern | Best for | Example structure |
|---------|----------|-------------------|
| **Workflow-based** | Sequential processes | Overview → Decision Tree → Step 1 → Step 2... |
| **Task-based** | Tool collections with distinct operations | Overview → Quick Start → Task A → Task B... |
| **Reference/Guidelines** | Standards, specs, brand guidelines | Overview → Guidelines → Specifications → Usage |
| **Capabilities-based** | Integrated systems with related features | Overview → Core Capabilities → Feature 1 → Feature 2... |

### Writing Principles

**Use the imperative form.** Write instructions as commands, not descriptions.

```markdown
# Good
Read the input file. Extract the header row. Validate each column type.

# Bad
The skill should read the input file and then it would extract the header row...
```

**Explain the WHY, not just the WHAT.** Today's LLMs are smart. When they understand the reasoning behind an instruction, they generalize better and handle edge cases that the instruction didn't explicitly cover.

```markdown
# Good
Use pandoc for text extraction because it preserves tracked changes
and handles nested formatting that raw XML parsing misses.

# Bad
ALWAYS use pandoc for text extraction. NEVER use raw XML parsing.
```

**Avoid heavy-handed MUSTs and NEVERs.** If you find yourself writing ALWAYS or NEVER in all caps, that's a yellow flag. Reframe by explaining the reasoning so the model understands why it matters. Heavy-handed constraints make skills brittle and narrow.

**Include few-shot examples.** Show the model what good input→output looks like. Examples are one of the most powerful tools for shaping behavior.

```markdown
## Commit message format

**Example 1:**
Input: Added user authentication with JWT tokens
Output: feat(auth): implement JWT-based authentication

**Example 2:**
Input: Fixed crash when uploading files over 10MB
Output: fix(upload): handle large file uploads without crash
```

**Define output formats explicitly** when the output structure matters:

```markdown
## Report structure
Use this exact template:

# [Title]
## Executive summary
[2-3 sentence overview]
## Key findings
[Bulleted list of findings with evidence]
## Recommendations
[Numbered actionable recommendations]
```

### Organizing Large Skills

If the SKILL.md is approaching 500 lines, split content into reference files:

```
my-skill/
├── SKILL.md                  (core workflow + pointers to references)
└── references/
    ├── aws-deployment.md      (read when deploying to AWS)
    ├── gcp-deployment.md      (read when deploying to GCP)
    └── troubleshooting.md     (read when errors occur)
```

In the SKILL.md, tell Claude when to read each reference:

```markdown
## Deployment
Read the appropriate reference file based on the target platform:
- AWS: `references/aws-deployment.md`
- GCP: `references/gcp-deployment.md`

If you encounter errors during deployment, consult `references/troubleshooting.md`.
```

For reference files over 300 lines, include a table of contents at the top.

### Scripts

Use scripts for deterministic, repetitive operations. Scripts execute without loading into context (saving tokens), but Claude can read them when it needs to understand or modify the logic.

Good candidates for scripts:
- File format conversions
- Validation and linting
- Template scaffolding
- Data extraction or transformation

```python
#!/usr/bin/env python3
"""
Extract form fields from a PDF.
Usage: python scripts/extract_fields.py input.pdf
"""
```

### Assets

Use assets for static files that appear in outputs: templates, fonts, icons, images, CSS files. Assets are copied or referenced, not interpreted.

---

## Step 5: Testing

After drafting the skill, create 2-3 realistic test prompts — the kind of thing a real user would actually say, not carefully constructed test inputs.

**Good test prompts:**
- "Can you make me a Word doc summarizing this data?" (casual, vague)
- "Create a professional report with a table of contents and page numbers" (specific)
- "Fix the formatting in this .docx file" (edit task)

**What to verify:**
1. Does the skill **trigger correctly** from the test prompt?
2. Does Claude **follow the instructions** in the SKILL.md?
3. Is the **output quality** acceptable?
4. Does it handle **edge cases** (missing inputs, ambiguous requests)?

If the user wants formal evals, create an `evals/evals.json`:

```json
{
  "skill_name": "my-skill",
  "evals": [
    {
      "id": 1,
      "prompt": "The user's actual request",
      "expected_output": "What success looks like",
      "files": [],
      "assertions": [
        "Output includes a table of contents",
        "All headings use Heading 1 style",
        "Page numbers appear in the footer"
      ]
    }
  ]
}
```

### The Feedback Loop

Every time the user provides an example or input, immediately run it rather than waiting for a full specification. Show the output and let the user react. Seeing what Claude actually does is the fastest way to refine requirements.

---

## Step 6: Common Pitfalls

| Pitfall | Fix |
|---------|-----|
| Skill never triggers | Make the description more explicit and "pushy" — list more trigger phrases |
| Skill triggers on wrong requests | Add exclusions to the description ("Do NOT use for...") |
| Claude ignores instructions | Add examples showing the desired behavior — few-shot > rules |
| Output format is inconsistent | Provide an explicit template with exact structure |
| SKILL.md is too long | Move detailed content to `references/` files with clear pointers |
| Skill is too rigid | Replace MUST/NEVER rules with explanations of WHY |
| Skill only works for test examples | Generalize from specific feedback — explain principles, not just fixes |
| Claude wastes time on unproductive steps | Read the execution transcript, remove instructions causing wasted effort |

---

## Step 7: Packaging & Delivery

Once the skill is finalized:

1. **Verify the structure** — SKILL.md exists with valid YAML frontmatter
2. **Check all file references** — scripts, references, and assets are present and paths are correct
3. **Copy to the output directory** so the user can access it
4. **Present the files** to the user with a brief summary of what the skill does and how to install it

If a `package_skill.py` script is available, use it to create a `.skill` archive:

```bash
scripts/package_skill.py <path/to/skill-folder>
```

---

## Quick Reference: SKILL.md Template

For a ready-to-use template, read `templates/skill-template.md` in this skill's directory.

---

## Meta-Advice for Skill Authors

These principles come from extensive iteration on production skills:

1. **The description is everything.** Spend more time on the description than you think you need. A great skill with a bad description is invisible.

2. **Explain reasoning, not just rules.** "Use pandoc because it handles tracked changes" beats "ALWAYS use pandoc." The model generalizes better when it understands why.

3. **Write a draft, then look at it fresh.** Your first draft will be either too detailed (overfitting to examples) or too vague (leaving room for interpretation where you don't want it). Revise with fresh eyes.

4. **Generalize from specific feedback.** When a user says "this output is wrong," don't just fix that specific case. Understand what principle was violated and encode the principle.

5. **Keep it lean.** Remove instructions that aren't pulling their weight. If something in the skill makes the model waste time on unproductive work, cut it.

6. **Skills are for millions of uses.** You iterate on a few examples because it's practical, but the skill must work across many different prompts. Avoid overfitting to your test cases.

7. **Don't hold back.** Claude is capable of extraordinary work. Skills that set high standards and explain why they matter produce dramatically better results than skills that play it safe.
