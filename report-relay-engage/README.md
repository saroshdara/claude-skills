# report-relay-engage

Three Claude Skills for managing conversation state - checking status, handing off work, and deciding what's next. Built for [Claude Code](https://code.claude.com/docs/en/skills) and Cowork; each also works in a claude.ai Project chat.

All three are explicit-invocation only (`disable-model-invocation: true`) - Claude never triggers them on its own, you call them by name.

## The three skills

| Skill | Invocation | Answers | Output |
|---|---|---|---|
| **report** | `/report` | Where are we, right now? | A scannable status update: current chat, Project context if relevant, and a bottom line. |
| **relay** | `/relay` | What does a brand-new conversation need to know to take over? | A self-contained handoff brief you can paste into a fresh chat and say "continue from here." |
| **engage** | `/engage` | Given where we are, what should we do next? | One decisive recommended next move, with reasoning, plus a short sequence after that. |

They share a conversation as their source of context but are built not to overlap: report looks backward at status, relay packages everything for a new reader, engage looks forward and picks the next action.

## Install

Each skill is a folder containing a single `SKILL.md`. Drop the folders into your skills directory:

```
~/.claude/skills/report/SKILL.md
~/.claude/skills/relay/SKILL.md
~/.claude/skills/engage/SKILL.md
```

(Use a project's `.claude/skills/` instead for a project-scoped install.) Claude Code and Cowork pick them up from there - no packaging or upload step needed for a local install. Restart or start a new session for a newly added skill to be picked up.

## report

Reviews the current conversation and reports:

- **CURRENT CHAT** - what we're trying to accomplish, what's done, key decisions, current state, open items, and next steps
- **PROJECT CONTEXT** (only if the conversation is part of a Claude Project) - the Project's broader purpose, relevant work done elsewhere in it, and how this chat fits in
- **BOTTOM LINE** - one or two sentences on where things stand right now

Newer decisions override older ones, brainstorming isn't treated as a decision, and nothing is invented that isn't actually in the conversation or Project.

## relay

Produces a handoff brief assuming zero shared context with the reader - written so it can be pasted into a brand-new conversation:

MISSION/OBJECTIVE, BACKGROUND, WORK COMPLETED, KEY DECISIONS, CURRENT STATE, REQUIREMENTS & CONSTRAINTS, IMPORTANT ARTIFACTS, OPEN QUESTIONS/BLOCKERS, NEXT EXPECTED WORK, and a HANDOFF INSTRUCTION addressed to the receiving Claude.

Optimized for continuity over brevity - it strips conversational noise and dead ends but keeps anything that would be painful to reconstruct later.

## engage

Recommends the smartest next move rather than summarizing what happened:

MISSION, an optional WHERE WE ARE, a single RECOMMENDED NEXT MOVE with reasoning, AFTER THAT (a short sequence), optional DECISIONS NEEDED, and READY TO ENGAGE - naming the specific action Claude can take immediately with a go-ahead.

Decisive by design: when several paths are viable it picks one and names the trade-off, rather than listing options.

## Notes

- All three check for Claude Project context (project instructions, project knowledge, other conversations) when it's genuinely accessible, and actively look for search tools before concluding there isn't any - they don't guess at what a Project might contain if they can't reach it.
- `disable-model-invocation: true` is a [Claude Code frontmatter extension](https://code.claude.com/docs/en/skills#control-who-invokes-a-skill), not part of the base Agent Skills spec - it's fully supported by local install and by Cowork's skill upload, so no changes are needed there.
