# Enterprise Conduct skills

A set of skills that give an AI coding agent the work ethic and common sense to
operate on real, production, enterprise codebases — instead of treating every
project like a student assignment.

They're standard [Agent Skills](https://developers.openai.com/codex/skills)
(`SKILL.md` format), so the same files work in **Claude Code**, **Codex**, and
other standard-compliant agents.

## Quick install

**Claude Code** — each skill is its own plugin, so install any subset (or all
four for the whole thing):

```bash
claude plugin marketplace add farbodghasemlu/enterprise-conduct
claude plugin install trust-boundaries@enterprise-conduct
claude plugin install professional-output@enterprise-conduct
claude plugin install calibrated-judgment@enterprise-conduct
claude plugin install enterprise-conduct@enterprise-conduct   # umbrella
```

**Codex** — copy the skills you want into `~/.agents/skills/`:

```bash
git clone https://github.com/farbodghasemlu/enterprise-conduct
cp -r enterprise-conduct/plugins/trust-boundaries/skills/trust-boundaries ~/.agents/skills/
```

Manual install, Claude.ai `.skill` upload, and other agents: see
**[INSTALL.md](INSTALL.md)**.

The core idea is two instincts the agent is missing:

1. **Audience before output** — know *who reads this* (internal vs external)
   before writing, and disclose accordingly.
2. **Stakes before action and effort** — gauge how reversible and far-reaching a
   thing is, and spend caution and effort to match — not based on how a command
   happens to be phrased.

## The skills

| Skill | What it fixes |
| --- | --- |
| **`trust-boundaries`** | Internal info leaking into public SDKs, docs, READMEs, changelogs, and error messages; roadmap/decision narration in public writing; secrets in the repo. |
| **`professional-output`** | Self-attribution in commits, advertising that AI wrote the code, AI writing tells and filler, sloppy commit/PR messages. |
| **`calibrated-judgment`** | Gold-plating trivial code while shipping breaking changes; asking to read a log but migrating prod without asking; dependency hacks and stray artifacts. |
| **`enterprise-conduct`** | The umbrella: the two instincts, a pre-flight checklist, and pointers to the three above. |

## Two ways to use it

The repo is a Claude Code plugin marketplace of four separate plugins (and the
skill folders install manually in any agent), so:

- **Cherry-pick.** Each of the three pillar skills is self-contained — install
  just `trust-boundaries` (or either other) on its own and it works.
- **The whole thing.** Install all four. `enterprise-conduct` supplies the
  unifying frame and the pre-flight checks that tie the pillars together.

## Layout

```
.claude-plugin/marketplace.json     # lists the 4 plugins (Claude Code install)
plugins/<skill>/
  .claude-plugin/plugin.json
  skills/<skill>/SKILL.md           # the skill itself (+ evals/)
.agents/skills/<skill> -> ...        # symlinks so Codex finds them in this repo
dist/<skill>.skill                   # zips for Claude.ai / API upload
```

## Always-on companion (recommended)

Skills are *pull-based*: the agent consults one only when it decides it needs
help. For the riskiest conduct cases — writing a commit message, cleaning up a
file before a push, running a production command — the agent is confident it can
just do the task, so it may never open the skill at all. Description tuning
raises this but can't fully fix it.

To guarantee the agent looks, drop this block into your project's `CLAUDE.md`
(Claude Code) or `AGENTS.md` (Codex and most other agents) — or a pre-tool hook.
The skills still carry all the actual guidance; this just forces the lookup:

```markdown
## Conduct on this codebase
This is a real/production project. Before you act, consult the matching skill —
these are easy to skip because the task looks routine:
- Writing ANY commit message, PR, README, docs, changelog, release notes, or
  code comments → `professional-output`
- Writing or editing anything public-facing, generating release notes or a
  changelog from git history, or writing error responses → `trust-boundaries`
- Running destructive or production commands (migration, drop/alter table,
  bulk delete, deploy, force-push), or changing a public API, schema, or
  response shape → `calibrated-judgment`
Never add AI self-attribution or "built with AI" badges. Never put internal
infra, roadmap, internal decisions, or secrets in public artifacts.
```

## Testing

Each skill ships an `evals/` folder with runnable behavioral cases (prompt +
pass/fail criteria). With Claude Code v2.1.269+ you can run them from a skill
directory:

```bash
claude plugin eval . --allow-tools Write,Edit,Bash
```

These are real, billed model runs. For stable numbers prefer 3 runs per case;
triggering and conduct are stochastic, so a single run per case is noisy.

## Related skills it leans on

`professional-output` defers to **`humanizer`** for detailed prose patterns, and
`calibrated-judgment` defers to **`right-sized-code`** for how much to build.
Those are separate skills; install them alongside for the full effect.

## License

MIT — see [LICENSE](LICENSE).
