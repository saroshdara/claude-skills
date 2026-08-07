---
name: deep-research
description: >
  A single-tier deep research skill. Runs real multi-source web research
  (6-8 varied searches, the same depth as a full deep-research pass) but
  delivers it as free-flowing, plain-language prose directly in chat — never
  a rigid report, never a downloadable file. CRITICAL: only trigger on
  explicit phrases — "deep research on X", "go deep on X", "research this in
  depth", or "use the deep research skill". Do NOT trigger on casual
  questions, background knowledge, or anything a direct answer already
  covers. If in doubt, do not trigger — just answer normally.
---

# Deep Research

One depth setting, real search volume, no tier menu, no rigid template, and
the answer stays in the chat.

This skill exists because a generic "go do deep research" instinct tends to
either turn into a background research agent (opaque, slow, token-heavy) or
a formal report (headers, citations on every line, a downloadable file).
Neither is the goal here. This skill is the middle path: genuinely thorough
searching, explained like a person talking, not a document being filed.

---

## Trigger Rule

Fire immediately on explicit trigger phrases — no tier question, no menu,
this skill only has one setting. Never fire on a casual question, a "what is
X," or anything answerable directly from general knowledge. When genuinely
unsure whether the trigger applies, don't use the skill — just answer the
question normally.

---

## Search Mechanics

Use regular `web_search` only. Never an autonomous/background research tool
— that kind of tool runs on its own, decides its own query count, and comes
back with a finished report; it's the opposite of what this skill is for.
Every query here should be chosen deliberately, one at a time, in the open.

- **6-8 queries**, each targeting a genuinely different angle — don't
  rephrase the same question twice. If the topic has 2-3 distinct parts,
  split the query budget across them rather than spending it all on one
  slice.
- **Fetch policy (`web_fetch`) is opt-in only.** Work off search snippets by
  default. Fetch a full page only when one specific claim is central to the
  answer, surprising, or contested across sources, and the snippet alone
  doesn't settle it. Max 1 fetch per response. Never fetch just to "read
  more broadly" — that defeats the point of staying light per-source while
  still being thorough in query count.

---

## Output Length — No Fixed Cap, But Two Checks

Don't write to a target word count. Let the answer be as long as the
question's actual shape requires, then use these two checks instead of a
number:

- **Soft checkpoint around ~1,200 words in:** pause and ask — is continuing
  to write still resolving a real, distinct part of the question, or just
  adding more detail to something already answered? Genuine uncovered
  ground → keep going. Padding → wrap up.
- **Hard tripwire at ~4,000 words:** stop regardless of what's left. If
  something is still genuinely uncovered at that point, name it in one line
  rather than continuing to expand.
- **Before searching**, if the question clearly spans 4+ distinct
  dimensions, say so in one sentence and ask whether to stay within this
  skill's scope or do something broader, rather than discovering the
  overrun mid-answer.

---

## Structure and Format

Free-flowing prose. No fixed template, no forced section headers, no
numbered buckets like "Key Findings / Recommendations / Caveats." Organize
around the natural logic of the topic itself — one idea leading into the
next, the way you'd actually explain it out loud to someone.

- A header earns its place only if the topic genuinely has 2-3 distinct
  chunks that benefit from a visual break. Never add one as a default.
- Bullets are for genuinely parallel items only (a list of options, a set of
  named sources) — never as a substitute for explaining something in a
  sentence.

---

## Language Level

Plain, everyday language throughout, from the first sentence to the last.
This is not "explain it technically, then translate" — there's no separate
translation pass. If a technical term is genuinely the right word, use it,
and unpack it briefly right where it appears, in the same sentence or the
next one, not in a separate glossary later.

No persona-switching into a field-specific voice (no "as a researcher,"
no adopting a specialist tone based on topic). The read should feel like a
knowledgeable friend explaining something over coffee — a learning session,
not a report being delivered.

---

## Citation Style

Keep the body completely clean — no inline citation markers, no "according
to X" scattered through every paragraph breaking up the flow. At the very
end, add a short line:

> Sources: [Name 1], [Name 2], [Name 3]

Two to three names, not a bibliography.

---

## Delivery Surface

Always deliver the answer inline in the chat, as normal conversational text.
Never create a markdown file, document, or any other artifact for this
output — regardless of length — unless explicitly asked for a downloadable
version after seeing the answer.

---

## Optional Add-Ons

None. This is a pure research-and-explain tool. Don't bolt on a
philosophical layer, video or podcast recommendations, or any other extra
pass. If one of those is wanted, it should be asked for separately.

---

## Quality Check (Lightweight)

Before finishing, a quick internal check — not a formal checklist, not
something to show your work on:

- At least 3 distinct sources actually incorporated into the answer
- At least 1 genuine nuance or common misconception flagged somewhere in
  the explanation
- One pass, one answer — don't auto-expand into extra searches once this
  bar is met just because more could theoretically be found

---

## Follow-Up Behavior

Close every answer with a specific, named offer to go deeper — never a
generic "want to know more?" Point at the actual angle that's worth
extending, e.g., "Want a closer look at how this plays out for [specific
sub-topic]?"

- If the answer is yes, run 2-3 more targeted queries on that specific
  angle, same skill, same plain-language voice, same guardrails. Fold the
  new findings into what's already there — don't restart the whole answer.
- If that "yes" turns out to open up something genuinely broad (multiple
  new dimensions, not just one thread), flag that and ask whether to keep
  going here or treat it as its own separate research question.

---

## Design Notes

Single-tier by design — no Quick/Standard/Deep menu, just one depth,
always. That's a deliberate choice: a tier menu adds a decision point before
every use, and this skill is meant to be simple enough that "deep research
on X" just works, every time, at one predictable depth.
