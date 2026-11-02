# Installing

These are standard **Agent Skills** (`SKILL.md` format), so they work in any
agent that supports the standard. Pick your agent below. Install one skill
(cherry-pick) or all four (the whole thing) — each triggers independently.

> If you fork this, swap `farbodghasemlu/enterprise-conduct` for your own repo
> path and update `owner` in `.claude-plugin/marketplace.json`.

## Claude Code — one command (recommended)

This repo is a plugin marketplace with four cherry-pickable plugins.

```bash
# register the marketplace once
claude plugin marketplace add farbodghasemlu/enterprise-conduct

# install exactly what you want
claude plugin install trust-boundaries@enterprise-conduct
claude plugin install professional-output@enterprise-conduct
claude plugin install calibrated-judgment@enterprise-conduct
claude plugin install enterprise-conduct@enterprise-conduct   # the umbrella

claude plugin list        # verify
```

Updates reach users when a plugin's `version` in its `plugin.json` changes.

## Claude Code — manual (no marketplace)

Copy a skill folder into your skills directory:

```bash
# user-level (all projects)
cp -r plugins/trust-boundaries/skills/trust-boundaries ~/.claude/skills/

# or project-level
cp -r plugins/trust-boundaries/skills/trust-boundaries .claude/skills/
```

## Codex CLI / IDE / app

Codex discovers skills in `~/.agents/skills/` (user) and `.agents/skills/`
(project). Copy the ones you want:

```bash
# user-level (all projects)
mkdir -p ~/.agents/skills
cp -r plugins/trust-boundaries/skills/trust-boundaries ~/.agents/skills/

# or let Codex use them while working inside THIS repo — the .agents/skills/
# symlinks already point at the skills, so no copy is needed here.
```

Invoke explicitly with `/skills` or `$` in the CLI (`@` in ChatGPT); Codex also
picks a skill implicitly when a task matches its `description`. Disable one
without deleting it via a `[[skills.config]]` entry in `~/.codex/config.toml`.

## Claude.ai / Claude API

Upload the packaged `.skill` files from `dist/` in the Skills/Capabilities
settings (or via the API Skills endpoint):

```
dist/trust-boundaries.skill
dist/professional-output.skill
dist/calibrated-judgment.skill
dist/enterprise-conduct.skill
```

Rebuild them after edits with the skill-creator's `package_skill` script.

## Any other standard agent

Point it at a skill folder (`plugins/<skill>/skills/<skill>/`) or copy that
folder into whatever directory the agent scans for `SKILL.md` skills.

## Make it work without being asked

Skills are pull-based — see the `CLAUDE.md` / `AGENTS.md` companion block in the
README to force the agent to consult them on the riskiest actions.
