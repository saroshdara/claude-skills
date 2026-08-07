---
name: project-catalog
description: >-
  Crawls a project's .claude folder (skills, agents, commands, hooks, MCP
  servers, output styles) and generates a fresh PROJECT-CATALOG.md inventory
  file at the project root, also displaying the full table in chat. Every run
  rebuilds from scratch, no caching, no memory of prior runs. Only run this
  when the user explicitly asks for it, e.g. "run project catalog," "refresh
  the project catalog," "regenerate PROJECT-CATALOG.md," or "catalog this
  project's skills/agents/commands." Do not auto-trigger on generic questions
  like "what skills do I have" without an explicit request to run or refresh
  the catalog.
disable-model-invocation: true
---

## What this does

Produces a complete, current inventory of everything Claude-related that's set up in a project: skills, agents, commands, hooks, MCP servers, and output styles. The result is written to `PROJECT-CATALOG.md` at the project root and also shown in chat.

Every run starts from zero: no cached state, no memory of a previous run, no freshness check. It just reads what's actually in the project's `.claude` folder right now, every single time. That's a deliberate choice: it makes the output impossible to distrust. It can never drift out of sync with reality because it's never trusted to begin with, it's always rebuilt from the source files themselves.

## Step 1: Confirm a project is connected

Check whether a project folder is currently connected to this session. If none is connected, stop here and tell the user: "No project folder is connected yet. Connect one first, then run this again." Do not create or write any file in this case, there's nothing to scan.

## Step 2: Look for `.claude`

Check whether the connected project's root contains a `.claude` folder.

If it does not exist, stop here and tell the user: "No `.claude` folder found in this project, so there are no components to catalog." Do not write a file.

If it exists, continue, even if it turns out to be empty once you crawl it. An empty `.claude` folder (a project mid-setup, or one that only has a CLAUDE.md so far) is a valid state, not an error, so don't bail out on it. Produce the catalog with the empty categories marked as such.

## Step 3: Crawl every component in one batched pass

Don't read files one at a time with separate tool calls, that gets slow fast once a project has more than a handful of items. Instead, run one shell scan per category and pull everything back in a single pass, something like:

```bash
cd "<project-root>"

echo "## SKILLS"
for f in .claude/skills/*/SKILL.md; do [ -f "$f" ] && echo "--- $f ---" && cat "$f"; done

echo "## AGENTS"
for f in .claude/agents/*.md; do [ -f "$f" ] && echo "--- $f ---" && cat "$f"; done

echo "## COMMANDS"
for f in .claude/commands/*.md .claude/commands/**/*.md; do [ -f "$f" ] && echo "--- $f ---" && cat "$f"; done

echo "## OUTPUT STYLES"
for f in .claude/output-styles/*.md; do [ -f "$f" ] && echo "--- $f ---" && cat "$f"; done

echo "## SETTINGS (hooks + mcp servers may live here)"
cat .claude/settings.json 2>/dev/null
cat .claude/settings.local.json 2>/dev/null

echo "## MCP CONFIG"
cat .mcp.json 2>/dev/null
```

Adjust glob patterns as needed for how the project is actually laid out; some categories will legitimately come back empty, that's fine. From the output, extract for each item:

- **Skills / Agents / Commands / Output styles**: the `name` and `description` from YAML frontmatter at the top of each file. If a command file has no frontmatter, use the filename (without extension) as the name and the first heading or first line of the file as the description.
- **Hooks**: read the `hooks` key inside the settings files. Entries are keyed by event (e.g. `PreToolUse`, `PostToolUse`). Use the event name as the item's name, and the matcher/command as its description.
- **MCP servers**: read the `mcpServers` key, wherever it's defined (`.mcp.json` or inside a settings file). Use the server's key as the name, and its command/args or URL as the description.

## Step 4: Write a short trigger line for each item

Raw descriptions are often written for a different purpose than a quick-glance table, some are long, some are vague, some are just a filename. For each item, write one short phrase, roughly a dozen words or fewer, capturing when someone would actually reach for it. Ground this in the real description, don't invent behavior that isn't documented. If a description is already short and clear, reusing it close to verbatim is fine and often better than paraphrasing for its own sake.

## Step 5: Assemble the catalog content

Structure it as one markdown table per category, in this order: Skills, Agents, Commands, Hooks, MCP Servers, Output Styles. Columns: Name, Trigger, Description.

If a category has no items, still include its heading, with a single line underneath reading "*None found.*" A stable, predictable structure (every category always present) makes it immediately obvious when a project is missing something, versus looking like the scan itself failed.

Open the file with a short note that it's auto-generated, so nobody mistakes it for a hand-maintained doc and starts editing it directly, since those edits would just get overwritten next run:

```markdown
# Project Catalog

_Auto-generated by the `project-catalog` skill. Rebuilt from scratch on every run — edits made directly to this file will be overwritten._

## Skills
| Name | Trigger | Description |
|---|---|---|
| ... | ... | ... |

## Agents
| Name | Trigger | Description |
|---|---|---|
| ... | ... | ... |

## Commands
...

## Hooks
...

## MCP Servers
...

## Output Styles
...
```

## Step 6: Write the file

Save the content to `PROJECT-CATALOG.md` at the **project root**, never inside `.claude`, that placement is deliberate so the catalog is immediately visible to anyone opening the project, not tucked away in a folder people treat as internal config. If the file already exists, overwrite it completely. That's the expected outcome on every run, not a special case to detect or guard against.

## Step 7: Report back, in this order

1. Confirm success and state the file's location first, e.g. "Created the catalog at `PROJECT-CATALOG.md` in the project root."
2. Then print the full table content in the chat as well.

Both parts matter: the file gives the user something durable they can open anytime without re-running anything, the chat output gives them the answer immediately without having to go open a separate file.

## Why it works this way

There's no cached state and no staleness-detection logic, on purpose. A batched scan of a typical project's `.claude` folder is cheap enough that rebuilding from scratch costs about the same as checking whether a rebuild is even needed, so the extra complexity of tracking what changed since last time isn't worth carrying. It also means this skill has zero setup and no first-run special case: it behaves exactly the same way every single time it's invoked, on any project, regardless of whether it's been run before.
