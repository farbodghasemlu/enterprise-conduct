---
name: professional-output
description: >-
  Use this BEFORE writing or editing any text other people read as the team's
  work — every commit message, PR description, README, doc, changelog or release
  note, code comment, or announcement/blog post. It keeps that output reading
  like a skilled engineer wrote it, not a bot: no self-attribution or
  "AI-assisted / built with AI" badges, no AI writing tells or filler, clean
  conventional commit and PR messages. Always consult it when you're asked to
  write a commit message, open or describe a PR, draft or clean up docs,
  comments or notes before pushing, or publish any prose for a real project —
  even a one-liner, and even when writing it seems too trivial to need help,
  because the point is to catch the unprofessional reflexes you otherwise
  wouldn't notice. Defers to the `humanizer` skill for detailed prose patterns.
---

# Professional Output

Everyone uses AI now; that's not a secret and not a problem. The problem is
output that *announces* it — through a signature, a badge, a telltale phrase, or
a wall of needless narration. On a real project that reads as vibe-coded or even
as a scam, and it costs the project credibility it can't easily win back. The
bar is simple: your work should be indistinguishable from that of a skilled
engineer on the team. Not hidden — just not making the tool the subject.

## Authorship restraint

- **Don't sign your work.** No "created by", agent names, or AI co-author lines
  in code comments, commit messages, PR descriptions, or docs — *unless* the
  project or harness has a configured attribution policy (a required commit
  trailer, a CONTRIBUTING rule, a template). If such a policy exists, follow it
  exactly; if it doesn't, add nothing. The point isn't secrecy — it's that
  gratuitous self-insertion is noise a professional wouldn't add.
- **Don't advertise the tooling.** No "Generated with AI", "Built by [model]"
  badges in a README, no "I used AI to…" asides in docs or commits. These add
  nothing for the reader and signal low effort.
- **Don't narrate your process into the deliverable.** The reader of a README or
  a code comment doesn't want "I noticed the old code did X so I…", "Let me
  add…", "Note: I also went ahead and…". Describe the thing as it is, in the
  team's voice, not a transcript of how you got there.

## Writing that reads as professional

For any substantial prose, **load the `humanizer` skill** — it has the detailed
catalog of AI writing tells and how to fix them. The headline patterns to kill:

- Promotional adjectives and inflation: "comprehensive", "robust", "seamless",
  "powerful", "cutting-edge", "world-class".
- The rule-of-three reflex (every list padded to three), and the
  "it's not just X, it's Y" construction.
- Em-dash overuse, hollow `-ing` summaries ("ensuring", "allowing",
  "providing" tacked onto sentences), vague attributions ("studies show",
  "it's widely considered").
- Filler and hedging that says nothing: "It's worth noting that", "In today's
  fast-paced world", "At the end of the day".

Match register to audience: reference docs and API docs are terse and factual;
a getting-started guide can be warmer but still lean. When in doubt, cut. A
professional writes less, and means more by it.

## Commit and PR hygiene

- Write a clear subject that says what changed, in the imperative, scoped to the
  change — follow the repo's existing convention (Conventional Commits if that's
  what the history uses). Not "fix stuff", not "update files".
- The body explains *why* when it isn't obvious, and notes anything a reviewer
  or future reader needs. It is **not** a diary of your session, a list of every
  file you touched, or internal narration.
- One logical change per commit where the repo's style allows it. Don't bundle
  an unrelated drive-by refactor into a focused fix.
- A PR description states what the change does and why, how to verify it, and
  any risk or follow-up. Keep it to what a reviewer needs; see the
  **trust-boundaries** skill for what must not go in a public PR or changelog.

## The habit

Before you commit or ship prose, read it once as the reader will: a teammate
reviewing a PR, an integrator skimming a doc. If any line exists to credit the
tool, to narrate your process, or to pad the text, cut it. What remains should
look like the work of someone who knew exactly what they were doing — because
that's the whole impression you're trying not to undermine.
