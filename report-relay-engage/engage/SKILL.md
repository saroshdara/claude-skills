---
name: engage
description: Recommends the single smartest next action given everything known in the conversation (and Project, if applicable), rather than summarizing what already happened. Names one clear recommended next move with reasoning, sequences a few moves after that, flags any decisions that genuinely need the user's input, and offers to start immediately. Invoked explicitly via /engage. This is different from report (a status check) and relay (a handoff for a new conversation) - engage is forward-looking and prescriptive, not a recap, and should never just restate the conversation.
disable-model-invocation: true
---

# Engage

Engage answers one question: **given everything we know, what's the smartest next move?** It is not a summary with a to-do list bolted on - it is a decisive recommendation. The output should feel like a sharp colleague telling you what to do next, not a status report.

## Why decisiveness matters here

A generic task dump makes the user do the prioritization work that engage exists to do for them. The value of this skill is in picking one thing and defending it - not in listing every possible next step. If several paths are genuinely viable, pick the best one and name the trade-off briefly; don't present options as if they're all equally good.

## What to do when invoked

Review the conversation carefully. Then check whether it's part of a Claude Project - look for project instructions, project knowledge, or a project name anywhere you can see, don't assume it isn't just because nothing Project-related is already in the visible chat. If it is, actively check your available tools for anything that searches project knowledge or other conversations in the Project and use them before recommending anything - a next move that ignores relevant Project context could easily be the wrong one. Factor in what actually matters from there.

Work out:

- What objective is being pursued
- Where things currently stand
- What remains unfinished
- Dependencies that constrain what can happen next
- Decisions that still need to be made
- Whether something needs validating, testing, researching, designing, building, documenting, or discussing before proceeding

Then recommend next actions using this structure:

**MISSION**
One short statement of what we're trying to accomplish.

**WHERE WE ARE**
A very short status line - only include this if it's needed to make the recommendation make sense. Skip it if the recommendation is self-explanatory.

**RECOMMENDED NEXT MOVE**
The single best next action, with a brief explanation of why it should come before anything else.

**AFTER THAT**
The next few actions in sensible order - only as far ahead as is actually useful right now, not a full roadmap.

**DECISIONS NEEDED**
Decisions or questions that genuinely require the user's input before proceeding. Omit this section entirely if there aren't any - don't manufacture one.

**READY TO ENGAGE**
Close by naming the specific action Claude can take immediately if given the go-ahead.

## Principles

- Be decisive - pick a recommendation, don't hedge across several.
- Prioritize; don't dump a generic task list.
- Don't just summarize the conversation - that's what report is for.
- Never recommend work that's already been completed.
- Respect real dependencies and sequencing - don't suggest something that can't actually happen yet.
- Call out when something genuinely needs research or validation before moving forward, rather than skipping straight to building.
- Separate what must happen next from what could happen later.
- When multiple paths exist, recommend one and briefly explain the trade-off rather than listing them all as equal options.
- Only ask the user something if the answer would actually change what happens next - don't ask questions for the sake of thoroughness.
- Favor practical progress over more planning - if the honest answer is "just start building," say that.

## Format

```
MISSION
[one short statement]

WHERE WE ARE
[only if needed for the recommendation to make sense]

RECOMMENDED NEXT MOVE
[the one best action, and why it comes first]

AFTER THAT
[next few actions, sensibly ordered]

DECISIONS NEEDED
[only if real decisions are blocking progress]

READY TO ENGAGE
[the specific action Claude can take right now with a go-ahead]
```
