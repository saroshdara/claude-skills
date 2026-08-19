---
name: relay
description: Produces a self-contained handoff brief that lets a brand-new Claude conversation pick up the work with zero prior context - mission, background, what's done, key decisions and why, current state, requirements/constraints, artifacts, open questions, and recommended next work, ending with an instruction to the receiving Claude. Invoked explicitly via /relay. This is different from report (a status check for the current reader) and engage (a recommendation for what to do next here) - relay's output must stand completely on its own with no assumption that the reader can see this conversation or any Project.
disable-model-invocation: true
---

# Relay

Relay produces a handoff brief for a Claude that has never seen this conversation. The test for every line: if this got pasted into a brand-new chat with nothing else, would the receiving Claude have what it needs to continue intelligently, without the user re-explaining the history?

## Why this is different from a summary

A summary optimizes for brevity and assumes the reader has context. Relay assumes the *opposite* - zero context - so it optimizes for continuity instead. That means it's fine, even correct, for relay to be longer than a report if that's what full self-sufficiency requires. Cutting a detail that would be painful to reconstruct later defeats the point, even if it makes the output shorter.

## What to do when invoked

Review the entire current conversation carefully. Then check whether it's part of a Claude Project - look for project instructions, project knowledge, or a project name anywhere you can see, don't assume it isn't just because nothing Project-related is already in the visible chat. If it is, actively check your available tools for anything that searches project knowledge or other conversations in the Project and use them before writing the handoff - a receiving conversation that misses decisions made elsewhere in the Project isn't getting a real handoff. Fold in what actually matters from there.

Build the handoff using these sections, in order:

**MISSION / OBJECTIVE**
What we're ultimately trying to accomplish.

**BACKGROUND**
Only the background needed to understand the work - not the full history, just what's load-bearing.

**WORK COMPLETED**
What has actually been finished. Not proposed, not in progress - done.

**KEY DECISIONS**
Decisions that have already been made, with the reasoning behind them when that reasoning matters to not re-litigating them later.

**CURRENT STATE**
Exactly where things stand right now.

**REQUIREMENTS & CONSTRAINTS**
Technical requirements, stated preferences, naming conventions, architectural choices, limitations, or other rules the next conversation needs to keep following.

**IMPORTANT ARTIFACTS**
Files, documents, code, prompts, designs, or other outputs that were mentioned or created. Name them and note what they're for when known - but never imply an artifact is available to the receiving conversation if it isn't actually attached or accessible there.

**OPEN QUESTIONS / BLOCKERS**
Anything genuinely unresolved.

**NEXT EXPECTED WORK**
What the receiving conversation should probably tackle next.

**HANDOFF INSTRUCTION**
Close with a short note addressed directly to the receiving Claude - what it should understand before it starts working.

## Principles

- Optimize for continuity, not for brevity - trim clutter, not substance.
- Preserve anything that would be genuinely painful for the user to reconstruct from memory.
- Cut conversational noise: abandoned ideas, repetition, dead-end exploration that didn't lead anywhere.
- Keep facts, decisions, proposals, and unresolved ideas clearly distinct from one another - don't let a "we could try X" read like a decision.
- When a later decision supersedes an earlier one, only carry forward the later one (but note the reversal if the reasoning matters).
- Never describe proposed or discussed work as if it were completed.
- The output must be fully self-contained - assume the receiving conversation has no access to this conversation, this Project, or anything else.

## Format

Use the section headers above, in that order, exactly as written. The whole point is that the user can paste this into a new chat and say "continue from here" and get an intelligent continuation, so write every section as if the reader is meeting this work for the first time - because they are.
