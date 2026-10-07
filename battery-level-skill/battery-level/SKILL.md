---
name: battery-level
description: Quick health check of the current chat's context window ("battery level") that tells the user whether to keep going or move to a fresh chat. Explicit invocation only - run when the user types /battery-level or asks to check the battery level of this chat. Never auto-trigger.
---

# Battery Level

A fast, low-cost check of how much usable context is left in the current chat, so the user knows whether to continue here or hand off to a new chat.

## Rules for running it

- Keep it cheap. One quiet pass over the conversation you can already see. No tools, no web searches, no file reads, no thinking out loud.
- Never claim a precise token count. Claude cannot see its real usage in chat, so every number is an estimate and must be labeled that way.
- Output is 2 lines maximum, in the chat. No headings, no tables, no explanation of the method unless the user asks.

## What to check

1. **Compaction** - Does the start of the conversation look like a summary of earlier messages instead of the user's actual words? If yes, the verdict is red.
2. **Size estimate** - Weigh the number of turns, long pastes, uploaded files, long code blocks, and large tool outputs. Estimate the share of the model's context window used. If the window size is not known, assume 200K tokens.
3. **Recall canary** - Can you accurately restate the user's first request and any rules or constraints they set earlier in the chat? If not, or if you are unsure, the verdict is red.
4. **Drift signals** - Look at recent turns for: re-asking settled questions, contradicting earlier decisions, losing track of file names or versions, or dropping a rule the user set. One signal means orange at minimum. Two or more means red.
5. **Heavy load** - If the chat holds many large files or long tool outputs, move the verdict down one level, because these fill the window faster than conversation does.

## Verdict

The worst signal wins.

- 🟢 Under ~50% used and no drift signals
- 🟠 ~50-75% used, or one drift signal
- 🔴 Over ~75% used, compaction detected, a failed recall canary, or two or more drift signals

## Output format

Use exactly one of these, filling in the estimate or the main reason:

🟢 **Battery: Good** (~[X]% used, estimate). Keep going.

🟠 **Battery: Getting low** (~[X]% used, estimate). Finish what you're working on, then move to a fresh chat with a handoff summary.

🔴 **Battery: Low** ([main reason, e.g. "compaction detected" or "~80% used, estimate"]). Move now: run your handoff or summary skill if you have one, or ask me to write a handoff brief, then paste it into a new chat.
