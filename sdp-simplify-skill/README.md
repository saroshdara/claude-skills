# sdp-simplify

A Claude skill that rewrites a long or complicated response as a short, plain-English version that still covers every topic.

## What it does

- Simplifies the response just before it by default, or any text or link you give it
- Covers every topic, shrinking each one to a plain sentence and grouping related topics so the result stays short
- Uses everyday words, short sentences, and one idea per paragraph
- Uses no bullets, bold, headers, tables, or symbols, so it works well in voice mode
- Keeps the facts unchanged and keeps any warnings, anything unverified, or any hard rule
- Ends with a short offer to go deeper, one piece at a time

For a link, it opens the page and simplifies what the page says. It does no other research.

## Install

1. Download or clone this repo
2. Copy the `sdp-simplify` folder into your project's `.claude/skills/` folder
3. Or use the `sdp-simplify.skill` file anywhere your Claude app accepts skill files

## Usage

Ask for it in plain words:

- "Simplify that"
- "Shorten this"
- "Short and sweet, please"
- "Simplify this link" (then paste the link)

## Details

The full instructions live in [`sdp-simplify/SKILL.md`](./sdp-simplify/SKILL.md).

## Repo layout

```
sdp-simplify-skill/
  README.md
  LICENSE
  sdp-simplify.skill
  sdp-simplify/
    SKILL.md
```
