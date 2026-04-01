# AGENTS.md

This repository contains **skills** — markdown-based instruction packs that guide AI agents in performing specialized tasks. There is **no source code, build system, or test suite**.

## Repository Structure

```
xueyufish-scheme-writer/   Skill for writing architecture/design documents
xueyufish-scheme-checker/  Skill for reviewing/checking design documents
xueyufish-translation/     Skill for bidirectional CN↔EN markdown translation
```

Each skill directory contains:
- `SKILL.md` — entry point with metadata, when-to-use, and how-to-use instructions
- `templates/` — markdown templates that documents must follow
- `examples/` — example usage guides for different document types
- `README.md` — brief description

## Build / Lint / Test Commands

**None.** This repo is pure markdown. There are no build, lint, or test commands.

To validate a skill manually:
1. Read the `SKILL.md` to understand its purpose
2. Verify that referenced template/example files exist
3. Ensure all internal relative links resolve correctly

## Markdown Conventions

### File Naming
- All files use **kebab-case** with `.md` extension
- Template files: `*-template.md`
- Example files: descriptive kebab names (e.g., `arch-designs.md`, `faq-answers.md`)
- Split documents: `arch-design-<章节名称>.md`, `arch-design-func-<子章节名称>.md`

### Frontmatter (for translation skill)
Markdown files processed by the translation skill use YAML frontmatter:
```yaml
---
type: fleeting
lang: CN          # CN or EN — determines translation direction
status: wait
tags: [tag1, tag2]
translated: true  # added after translation; prevents re-translation
---
```

### Heading Structure
- Use numbered top-level headings for document sections: `# 1. 需求定义`, `# 2. 需求分析`, etc.
- Use `##` for sub-sections, `###` for sub-sub-sections, `####` for deeper levels
- Do not skip heading levels

### Tables
- Use standard GFM table syntax with aligned pipes
- Include a header row and separator row (`| --- | --- |`)
- Template tables use bracketed placeholders like `[field_name]`

### Links
- Use relative paths for internal references (e.g., `../templates/arch-design-template.md`)
- Use anchor links for cross-references within a document (e.g., `[2.2功能需求分析](#2.2-功能需求分析)`)
- External references must include full URLs

### Content Rules
- **NA for inapplicable sections** — never delete or skip template sections; mark as `NA`
- **Uncertain items must be flagged** — explicitly mark anything requiring confirmation
- **Keep documents under 2000 characters** — split into multiple files if longer
- **Professional tone** — maintain architect-level expertise in all generated content
- **Terminology table required** — explain all domain-specific terms with Chinese, English full name, abbreviation, and definition

## Skill-Specific Guidelines

### xueyufish-scheme-writer
- When asked to write design documents, load this skill
- Identify document type from the request, then load the matching example from `examples/`
- Strictly follow the template in `templates/arch-design-template.md`
- Supported types: architecture design, development design, requirements design, data design, FAQ, project status reports, risk reports

### xueyufish-scheme-checker
- When asked to review/check design documents, load this skill
- Verify documents match the template structure exactly — no deviations allowed
- Cross-reference against project requirements, architecture docs, and industry best practices
- Output must be opinionated and unambiguous; avoid vague language

### xueyufish-translation
- **Only activate when user explicitly runs `/translate`** — never auto-load
- Supports CN→EN and EN→CN bidirectional translation
- Original content is never modified; translated content is appended under `# AI Translation`
- Uses `lang` frontmatter attribute to determine direction; skips files with `translated: true`

## Git Workflow

- Each skill subdirectory is its own git submodule/working tree
- The root directory is also a git repo
- Commit messages should be concise and descriptive
- Never commit without explicit user request
