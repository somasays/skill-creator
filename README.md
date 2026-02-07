# Skill Creator

A Claude skill that creates other Claude skills. Drop this into your `.claude/skills/` directory and Claude learns how to build production-quality skills from scratch.

## What Are Skills?

Skills are reusable instruction sets that teach Claude how to perform specific tasks consistently. Think of them as programming Claude's behavior using precise natural language — where the LLM is the interpreter and the skill is the source code.

A skill is a folder with a `SKILL.md` file and optional supporting resources (scripts, references, assets). When triggered, Claude reads the SKILL.md and follows its instructions.

## What's In This Repo

| File | Purpose |
|------|---------|
| `SKILL.md` | The core skill — full workflow for creating skills from intent capture through testing and packaging |
| `description-examples.md` | Real-world examples of effective skill descriptions with analysis of what makes them work |
| `structure-patterns.md` | Four structural patterns (workflow, task-based, reference, capabilities) with templates |
| `skill-template.md` | Ready-to-use SKILL.md template with YAML frontmatter and section scaffolding |

## Installation

### Claude Code (CLI)

```bash
# Clone and copy to your global skills
git clone git@github.com:somasays/skill-creator.git
mkdir -p ~/.claude/skills/create-skills
cp skill-creator/SKILL.md ~/.claude/skills/create-skills/
cp skill-creator/description-examples.md ~/.claude/skills/create-skills/
cp skill-creator/structure-patterns.md ~/.claude/skills/create-skills/
cp skill-creator/skill-template.md ~/.claude/skills/create-skills/

# Or for a specific project
mkdir -p your-project/.claude/skills/create-skills
cp skill-creator/* your-project/.claude/skills/create-skills/
```

Then just tell Claude: "Create a skill for X" and it follows the full workflow.

### Claude Desktop App (Recommended for Skill Creation)

Claude Desktop is ideal for creating skills because the process is research-heavy and iterative — Desktop maintains longer conversations and has native Project Knowledge support.

**Setup:**

1. Open **Claude Desktop** and create a new Project (or open an existing one)
2. Click the **project name** at the top to open Project Settings
3. Under **"Project Knowledge"**, click **"Add content"**
4. Paste the contents of `SKILL.md` (and optionally the other files for reference)
5. In **Project Instructions**, add:
   ```
   When I ask you to create a skill, follow the skill creation workflow
   in your project knowledge. Use the description examples and structure
   patterns as reference when drafting.
   ```
6. Start a conversation: "Create a skill for [your use case]"

**Why Desktop for skill creation?** The process involves researching documentation, interviewing the user about requirements, drafting, testing, and iterating. Desktop's longer context persistence and file attachment features make this workflow smoother than CLI.

## Example Use Cases

Here are examples of skills you can create with this:

**"Create a skill for LangGraph architectural patterns"**
→ Claude researches LangGraph docs, interviews you about which patterns matter, and produces a skill with StateGraph architecture, ReAct agent loops, tool binding patterns, state persistence, and iteration control — all as copy-paste production code.

**"Create a skill for our company's API style guide"**
→ Claude captures your naming conventions, error handling patterns, authentication standards, and versioning rules into a reference-pattern skill that ensures consistent API design across your team.

**"Turn this workflow into a skill"** (after iterating on a task in conversation)
→ Claude extracts the tools used, sequence of steps, corrections made, and input/output formats from the conversation and packages them into a reusable skill.

**"Create a skill for Terraform infrastructure patterns"**
→ Claude produces a skill covering module structure, state management, provider configuration, common resource patterns, and testing approaches — tuned to your cloud provider and conventions.

**"Create a skill for writing technical blog posts in our brand voice"**
→ Claude captures your tone, structure preferences, vocabulary, and formatting standards into a guidelines-pattern skill that produces consistent content.

## How Skills Work (Three-Level Loading)

Skills use progressive disclosure to manage context efficiently:

| Level | What | When Loaded | Size |
|-------|------|-------------|------|
| **Metadata** | `name` + `description` in YAML frontmatter | Always in context | ~100 words |
| **SKILL.md body** | Full instructions | When skill triggers | <500 lines |
| **Resources** | Scripts, references, assets | On-demand | Unlimited |

The description is the trigger mechanism — Claude decides whether to activate a skill based on how well the description matches the user's request.

## Key Insight: Descriptions Are Everything

The most common failure mode is **undertriggering** — Claude being too conservative about activating a skill. The skill creator addresses this by teaching Claude to write "pushy" descriptions that explicitly list trigger phrases:

```yaml
# Bad — too vague, Claude won't activate
description: Creates Word documents.

# Good — explicit triggers, secondary scenarios, exclusions
description: >
  Use this skill whenever the user wants to create, read, edit, or
  manipulate Word documents (.docx files). Triggers include: any mention
  of "Word doc", "word document", ".docx", or requests to produce
  professional documents with formatting. Also use when extracting content
  from .docx files or converting content into Word format.
  Do NOT use for PDFs, spreadsheets, or Google Docs.
```

## Contributing

Found patterns that make skills better? PRs welcome.

## License

MIT
