# battery-level

A Claude skill that checks the "battery level" of your current chat - how much of the context window is likely used - and tells you whether to keep going or move to a fresh chat.

Long chats eventually get compacted or start drifting: Claude forgets early instructions, re-asks settled questions, or loses track of files. This skill gives you a quick read before that happens, so you only start a new chat when you actually need to.

## What it looks like

🟢 **Battery: Good** (~30% used, estimate). Keep going.

🟠 **Battery: Getting low** (~60% used, estimate). Finish what you're working on, then move to a fresh chat with a handoff summary.

🔴 **Battery: Low** (compaction detected). Move now: run your handoff or summary skill if you have one, or ask me to write a handoff brief, then paste it into a new chat.

## What it checks

- **Compaction** - whether earlier messages have already been summarized
- **Size estimate** - turns, pastes, files, code blocks, and tool outputs against the model's window
- **Recall canary** - whether Claude can still restate your first request and the rules you set
- **Drift signals** - re-asked questions, contradicted decisions, lost file names, dropped rules
- **Heavy load** - lots of large files or tool outputs push the verdict down a level

## Install

1. Download the `battery-level` folder (the one containing `SKILL.md`) and zip it.
2. Upload the zip in the Skills section of Claude's settings.
3. In any chat, type `/battery-level`.

It runs only when you call it, never on its own.

## Honest limits

Claude can't see its real token count in the chat app, so the percentage is an estimate. The compaction, recall, and drift checks are usually the more reliable signals. The check is designed to be light, but on a very long chat it still uses a little context itself.

## License

MIT - see `LICENSE`.
