---
name: report
description: Gives a concise, scannable status update on the current conversation - what we're trying to accomplish, what's done, key decisions, current state, and open items - closing with a one-line bottom line. If the conversation belongs to a Claude Project, also pulls in relevant Project context as a separate section. Invoked explicitly via /report - this is a status check, not a handoff to a new conversation (that's relay) and not a recommendation for what to do next (that's engage).
disable-model-invocation: true
---

# Report

Report answers one question: **where are we, right now?** It is a status check, not a chronological retelling and not a plan. Someone should be able to read it in under thirty seconds and know exactly what's true today.

## Why the structure matters

A wall of prose forces the reader to reconstruct the state themselves. Fixed sections let them jump straight to the part they need - most of the time that's just the bottom line. Keep every section tight enough to scan; if a section is running long, that's a sign it needs editing, not more detail.

## What to do when invoked

### 1. Review the current conversation

Read back through the whole conversation and work out:

- What we are trying to accomplish
- What has already been completed
- Important decisions that have been made
- Important information, requirements, assumptions, or constraints established
- Where things currently stand
- Unresolved questions, blockers, or pending items
- The logical next step(s)

Write this up under a heading called **CURRENT CHAT**.

### 2. Check for Project context

First check whether this conversation is actually part of a Claude Project at all - look for project instructions, project knowledge, or a project name anywhere in what you can see. Don't assume it isn't just because nothing Project-related is already sitting in the visible chat.

If it is part of a Project, actively go look beyond this conversation before writing anything - check your available tools for anything that searches project knowledge or other conversations in the Project (names vary: project knowledge search, conversation search, and similar), and use them. This is a real research step, not a passive "if it happens to be in front of me" check - a one-line report that ignores a Project the user is clearly working in is not useful. Only treat Project context as unavailable after you've actually tried and come up empty, not because checking would take an extra tool call.

Once you've looked, work out:

- The broader purpose of the Project
- Work already completed elsewhere in the Project that's relevant here
- Decisions or requirements established in other conversations that bear on this one
- How this chat fits into the larger Project
- Dependencies or relationships with other work in the Project
- Outstanding work or issues elsewhere that affect this conversation
- Anything else from the Project that this chat alone would miss

Write this up separately under a heading called **PROJECT CONTEXT**. Keep it distinct from CURRENT CHAT - don't blend Project findings into the first section.

Only skip this section if you've confirmed the conversation genuinely isn't part of a Project, or you actively tried to look beyond it and had no way to reach anything more. Don't guess at what a Project might contain, and don't skip the section just to save a step.

### 3. Close with the bottom line

End every report with a heading called **BOTTOM LINE**: one or two sentences stating exactly where things stand right now. No hedging, no recap - just the current truth.

## Principles

- When earlier and later discussion conflict, the newer decision wins.
- Keep completed work and proposed-but-not-done work clearly separate. A good idea that was floated is not the same as a decision that was made, and neither is the same as work that's actually finished.
- Brainstorming is not a decision. Don't report an idea as settled just because it was discussed at length.
- Don't invent context that isn't actually in the conversation or Project. If something is unclear or missing, say so rather than filling the gap.
- Skip the chronological walkthrough - nobody needs "first we discussed X, then Y, then Z." Report the current state, not the history that produced it.
- Bias toward what matters right now over everything that was ever mentioned. A detail that got resolved or abandoned doesn't need airtime.

## Format

```
CURRENT CHAT
[what we're doing, what's done, key decisions, current state, open items, next steps]

PROJECT CONTEXT
[only if applicable - broader purpose, relevant work elsewhere, cross-conversation decisions, how this fits in]

BOTTOM LINE
[one or two sentences - where things stand, right now]
```
