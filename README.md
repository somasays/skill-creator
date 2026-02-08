# Skill Creator

A Claude skill that creates other Claude skills. Add it to your environment and Claude learns how to build production-quality skills from scratch.

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

This skill works best as a **user-level skill** — available across all your conversations and projects rather than scoped to a single project.

### Claude Desktop App

1. Go to **Settings → Capabilities → Add skills**
2. Upload the skill folder containing `SKILL.md` and the supporting files

### Claude Code (CLI)

```bash
git clone git@github.com:somasays/skill-creator.git ~/.claude/skills/skill-creator
```

Then use the `/skill-creator` slash command with a flag to specify where the generated skill should live:

```bash
# Create a user-level skill (available across all projects)
/skill-creator --user "Analytics with DuckDB"

# Create a project-level skill (scoped to current project)
/skill-creator --project "Analytics with DuckDB"
```

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

Apache License 2.0 — see [LICENSE](LICENSE) for details.