---
name: trust-boundaries
description: >-
  Keeps internal information out of anything the public, customers, or
  integrators see, and secrets out of the repo. Use it whenever a project has
  public and private parts (a private backend plus a public SDK, a monorepo with
  internal and published packages) and whenever you write or edit anything
  external-facing: a README, SDK/client code and comments, API docs, examples,
  or user-visible error messages. Consult it even when the task looks like plain
  coding or writing — generating release notes or a changelog from a git log,
  writing or fixing error responses that could leak stack traces or internal
  paths, and cleaning up code, comments, or TODOs before pushing to a shared or
  public repo. It tells you how to tell a public artifact from a private one and
  what must never cross: infrastructure, internal structure, roadmap, decisions,
  secrets. Reach for it before committing to a public repo or publishing docs,
  even if nobody said the repo was public.
---

# Trust Boundaries

A codebase is not one audience. Some of it is read by teammates, ops, and your
own future self; some is read by integrators, customers, and anyone on the
internet. The single most damaging habit on real projects is treating these as
the same pile — letting what's true and useful internally leak into what's
public, because both were just "files in the repo."

Before you write or edit anything, answer one question: **who reads this, and
are they entitled to it?** Everything below is that question made concrete.

## Step 1 — Classify the artifact

Decide whether what you're about to touch is **external** (anyone outside the
team may see it) or **internal**. Check for the project's own declaration first;
it always wins over your inference.

**Honor an explicit declaration when one exists:**
- A note in `CLAUDE.md`, `CONTRIBUTING.md`, or repo docs saying which
  paths/packages are public vs internal.
- `package.json` `"private": true` (internal) or a public `"publishConfig"` /
  absent `private` with a real version and license (external).
- Publish config and registry scope: an internal scope (e.g. a private registry
  or `@company-internal/*`) vs a public registry target.
- Monorepo workspace layout that separates `packages/public/*` from
  `packages/internal/*`, `apps/` from `libs/`, etc.
- An explicit marker file or header (e.g. `PUBLIC`, `INTERNAL`, a license
  header, an SPDX tag).

**Infer from signals when nothing is declared:**
- *Leans external:* published to a public registry; named `sdk`, `client`,
  `public`, `docs`, `examples`, `api`; a permissive `LICENSE` at the package
  root; a README addressed to "you"/"developers"/"integrators"; a `CHANGELOG.md`
  or `/docs` at package root; a public Git remote.
- *Leans internal:* `"private": true`; infra-as-code (Terraform, Kubernetes/
  Helm, Pulumi), CI/CD and deploy config, secrets management; names containing
  `internal`, `backend`, `infra`, `ops`, `admin`, `core`; ADRs, RFCs, runbooks,
  postmortems, design docs.

When signals conflict or you genuinely can't tell, treat it as external (the
safer default) and say so briefly, or ask. It is cheap to keep an internal
detail out of a public file by mistake; it is expensive to leak one.

## Step 2 — What must not cross into external artifacts

Once something is external, it describes **what exists and how to use it** — and
nothing about the world behind it. Keep all of the following on the internal
side of the line:

- **Infrastructure and topology.** Internal hostnames, service and queue names,
  internal endpoints, cluster/region layout, capacity numbers, rate-limit
  thresholds not meant to be public, internal auth mechanisms.
- **Internal structure.** How the private backend is organized, module/file
  paths, database schema, build internals — anything that reveals the private
  side's shape.
- **Roadmap and non-existence.** Why a feature doesn't exist, what's planned,
  deferred, or shelved, and why. A public API doc lists the `buy` endpoint and
  how to call it. It does **not** add "a `sell` endpoint was shelved for now
  because…". No real company narrates to integrators what it decided not to
  build or why. Document what is, not what isn't.
- **Internal decisions and process.** Why the team chose an approach, trade-offs
  debated, mistakes made and corrected along the way, sprint/ticket history.
  Integrators need the behavior, never the deliberation.
- **Internal identifiers.** Ticket/issue IDs (JIRA-1234), employee names,
  customer or client names, competitor names, internal project codenames.
- **Dev-process residue in code.** `TODO`/`FIXME`/`HACK` comments that carry
  internal context, commented-out internal URLs, debug notes, "temporary until
  we migrate X" asides — all shipped in public SDK/client code.
- **Copied private code.** Don't lift private implementation into a public repo
  (IP leak), and watch the license direction so you don't pull an incompatible
  license into either side.

### Changelogs and release notes

A public changelog is for the consumer and lists only what they can **observe**:
behavioral changes, new/changed/removed API surface, bug fixes that affect them,
and breaking changes with migration notes. Keep out internal refactors,
dependency bumps they can't see, "fixed a mistake from last sprint," ticket
references, and anything that reads as a diary of your work. If an entry doesn't
change what an integrator experiences, it doesn't belong there.

### User-visible errors

Errors returned to end users or integrators must not leak internals: no stack
traces, internal file paths, framework/version banners, SQL, or raw exception
dumps in production responses. Give a useful, bounded message; log the detail
internally.

## Step 3 — Secrets never enter the repo

Independent of public/private, secrets don't get committed, and they don't get
echoed into output:

- No credentials, API keys, tokens, passwords, private keys, or connection
  strings in code, config, tests, comments, or commit messages. Use the
  project's secret mechanism (env vars, a secrets manager, `.env` kept out of
  git via `.gitignore`).
- In public examples and docs, use obvious placeholders (`YOUR_API_KEY`,
  `sk_test_xxx`), never a real or real-looking value.
- Logging is disclosure too: don't log secrets, full request/response bodies, or
  PII. Log identifiers and outcomes, not payloads.
- If you find a secret already committed, stop and flag it — it likely needs
  rotation, not just deletion, because git history keeps it.

## The habit

You don't need to memorize the list. Keep the one question live: *who reads
this, and are they entitled to it?* When the answer is "someone outside the
team," write only what helps them use the thing, and let the internal world stay
internal.
