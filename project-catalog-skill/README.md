# Project Catalog

A Claude Code / Cowork skill that generates a fresh, complete inventory of everything set up in a project's `.claude` folder, skills, agents, commands, hooks, MCP servers, and output styles, and writes it to `PROJECT-CATALOG.md` at the project root.

## What it does

- Crawls `.claude/skills`, `.claude/agents`, `.claude/commands`, `.claude/output-styles`, plus hooks and MCP servers defined in settings files and `.mcp.json`
- Every run rebuilds from scratch. No caching, no memory of the last run, so the output can't go stale
- Writes `PROJECT-CATALOG.md` to the project root (never inside `.claude`, so it's visible the moment someone opens the project)
- Also prints the full table in chat, so you get the answer immediately and a durable file to check later

## Install

1. Download or clone this repo
2. Copy the `project-catalog` folder into your project's `.claude/skills/` directory
3. That's it, no other setup

## Usage

This skill has `disable-model-invocation: true` set, so it never fires on its own. You have to ask for it directly:

- "Run project catalog"
- "Refresh the project catalog"
- "Catalog this project's skills, agents, and commands"

## Output

Creates or overwrites `PROJECT-CATALOG.md` in the project root. It's meant to be committed to git and kept current, not a one-off report, every run replaces it with the current state of the project.

## Details

Full step-by-step logic lives in [`project-catalog/SKILL.md`](./project-catalog/SKILL.md).

## Repo layout

```
your-repo/
  README.md
  project-catalog/
    SKILL.md
```
