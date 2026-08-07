# Deep Research

A Claude skill for doing real, multi-source web research without either of
the two usual extremes: an opaque background research agent that burns a lot
of time and tokens, or a rigid formal report full of headers and dense
citations.

## What it does differently

- **Single tier, no menu.** No Quick/Standard/Deep choice to make every time
  — one consistent depth, always.
- **Real search volume.** 6-8 varied web searches per question, not a
  shallow skim.
- **Snippet-first.** Works off search result snippets by default; only
  opens a full page when one specific claim genuinely needs verifying.
- **Plain language throughout.** No jargon-then-translate. Technical terms
  get unpacked right where they show up, not shoved into a glossary at the
  end.
- **Free-flowing prose.** No forced template, no "Key Findings /
  Recommendations / Caveats" skeleton. The explanation follows the topic's
  own logic.
- **Stays in the chat.** Delivered as normal conversation text, never
  auto-converted into a downloadable file.
- **Length follows the question.** No arbitrary word target — a soft
  checkpoint around 1,200 words and a hard stop around 4,000 keep it from
  running away, without cutting a genuinely broad question short.

## Trigger phrases

The skill activates only on explicit phrases, by design, so it never fires
on a casual question it shouldn't:

- "deep research on X"
- "go deep on X"
- "research this in depth"
- "use the deep research skill"

## Install

1. Download `SKILL.md` from the `deep-research/` folder in this repo.
2. In Claude, add it as a custom skill (or use the packaged `.skill` file
   from this repo's Releases, if available, for a one-click install).

## Files

```
deep-research/
└── SKILL.md
```
