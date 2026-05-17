# Changelog

All notable changes to the `virtual-team` plugin.

Format follows [Keep a Changelog](https://keepachangelog.com/). Published at
[github.com/Apoorve8055/virtual-team](https://github.com/Apoorve8055/virtual-team). On
every release, bump `version` in **both** `.claude-plugin/plugin.json` and the `virtual-team`
entry in `.claude-plugin/marketplace.json`, then commit and push.

## [Marketplace 1.6.0] - 2026-05-16

> Note: Entries prefixed `Marketplace x.y.z` refer to the marketplace metadata
> version (top-level `version` field in `.claude-plugin/marketplace.json`).
> Unprefixed entries below (`[3.0.0]`, `[2.3.1]`, ...) are plugin releases.

### Changed (breaking)

- **Marketplace renamed `team` → `virtual-team`.** The marketplace `name` in
  `.claude-plugin/marketplace.json` is now `virtual-team`, so the install spec
  becomes `/plugin install virtual-team@virtual-team`. Plugin behavior, version,
  and command prefixes are unchanged (still v3.0.0, still `/virtual-team:*`).
  Marketplace metadata version bumps to 1.6.0 to mark the namespace change.
  Historical entries below keep their original `virtual-team@team` install spec
  as a point-in-time record.

  **Migration for existing installs.** The marketplace key `team` no longer
  resolves — existing installs of `virtual-team@team` will not update. To pick
  up the new key:

  ```
  /plugin uninstall virtual-team@team
  /plugin marketplace remove team
  /plugin marketplace add Apoorve8055/virtual-team
  /plugin install virtual-team@virtual-team
  ```

## [3.0.0] - 2026-05-14

### Changed (breaking)

- **Command prefix renamed `/team:*` → `/virtual-team:*`.** The plugin `name` is now
  `virtual-team`, so every command is re-namespaced — `/team:backend` becomes
  `/virtual-team:backend`, the index command `/team` becomes `/virtual-team`, and so on
  for all 13. The `commands/` directory and router file were renamed to match
  (`commands/virtual-team/`, `commands/virtual-team.md`). No change to any persona's
  behavior, expertise, or output contract — only the invocation prefix. Install is now
  `/plugin install virtual-team@team`. Historical entries below keep their original
  `/team:*` names as a point-in-time record.

## [2.3.1] - 2026-05-14

### Changed

- **Repository renamed to `virtual-team`** and flattened so the repo root *is* the
  plugin — no `plugins/` subdirectory; `.claude-plugin/` holds both `plugin.json` and
  `marketplace.json`, with marketplace `source: "./"`. The `repository` / `homepage`
  metadata now points at the new URL. No changes to commands, personas, or behavior.

## [2.3.0] - 2026-05-14

### Changed

- **Genericized for public distribution.** Removed an organization-specific ecosystem
  reference; the legacy file is replaced by `reference/ecosystem-context.md` — a
  customizable, documented template that keeps the opt-in ecosystem-context mechanism
  useful for any multi-app organization. The shared `<stack-detection>` step 6 across
  all 11 personas, `reference/stack-detection.md` step 8, and `/team:all`'s detection
  flag were updated to generic wording.

### Added

- **Publishing metadata.** Repo-root `LICENSE` (MIT), a marketplace-level `README.md`,
  and `.gitignore`. `plugin.json` gains `repository`, `homepage`, `license`, `keywords`,
  and `author.url`. The marketplace manifest is renamed from `local` to `team` with
  `owner.url` and per-plugin `repository` / `license` — preparing the plugin for
  publication as a public Claude Code marketplace on GitHub.

## [2.2.0] - 2026-05-14

### Added

- **`/team:devops`** — Senior DevOps Engineer: CI/CD pipelines, infrastructure as code,
  deployment and rollback safety, container hygiene, observability, and secrets/config
  management.
- **`/team:security`** — Senior Security Engineer: an adversarial, cross-cutting lens —
  threat modeling, authn/authz, the OWASP Top 10, supply-chain risk, secrets lifecycle,
  and data exposure. Deliberately distinct from `/team:backend`'s security sub-coverage.
- **`/team:mobile`** — Senior Mobile Engineer: native and cross-platform apps, the app
  lifecycle, offline/sync state, performance and battery budgets, permissions, and
  store-release safety.
- **`/team:data`** — Senior Data Engineer: data pipelines, schema and data contracts,
  analytics correctness, and ML/AI + LLM integration safety. Deliberately distinct from
  `/team:database`'s store-at-rest focus.

### Changed

- **`/team:all` is now stack-aware.** It always dispatches the seven universal
  specialists (architect, backend, frontend, database, qa, devops, security) and
  conditionally dispatches `mobile` and `data` only when stack detection finds them
  relevant — 7 to 9 subagents instead of a fixed five. The "Per-Specialist Health"
  section of its Output Contract is now dynamic.
- **`reference/stack-detection.md`** gained domain-signal detection hints for CI/IaC,
  mobile, and data/ML stacks, so the four new personas share one source of truth.
- **Shared-block consistency pass.** Normalized the `<stack-detection>` block, the
  `<process name="REVIEW">` skeleton, and the Output Contract boilerplate in
  `architect.md`, `qa.md`, and `review.md` to match the canonical template — fixing
  accumulated wording drift (`review or design work`, `review or test-planning work`,
  `reading everything inline`, `the structure you detected`, a non-canonical step-3
  frame, and a non-canonical step-6 line). All ten specialists now carry byte-identical
  shared blocks except their sanctioned per-domain lines.

## [2.1.0] - 2026-05-14

### Added

- **`/team:senior-brother`** — Senior Brother: a generalist mentor and selective
  orchestrator, and the team's "default door." Bring it anything — it detects what you
  actually need, helps directly or pulls in the right `/team:` specialist (or dispatches
  a subagent), and always closes with the better solution, where you're lacking, and how
  to level up (the "Brother's Take"). Distinct shape from the six specialists: one
  adaptive process, mentorship-flavored output. Reuses the shared `<stack-detection>`
  block and `/team:all`'s subagent-dispatch pattern. Listed in `commands/team.md` and
  `README.md`.

## [2.0.0] - 2026-05-14

### Changed (breaking)

- **All personas are now project-adaptive.** Every command runs a stack-detection step
  first (project root, `CLAUDE.md`/`AGENTS.md`, manifest, lockfile, layout) instead of
  assuming a hardcoded stack. The same command now gives correct, stack-aware feedback
  in any repo.
- **All five specialists rewritten** to a single consistent template: `<role>`,
  `## Persona`, `## Expertise`, `<stack-detection>`, `## Red Flags — STOP`,
  `## Anti-Patterns You Call Out`, `<process name="REVIEW">`, `<process name="DEVELOP">`,
  and a fixed `## Output Contract`.
- **`allowed-tools` tightened** — broad `Bash(gh pr *)` / `Bash(gh issue *)` replaced
  with explicit read-only verbs (`gh pr diff`, `gh pr view`, `gh pr list`).
- **`plugin.json` and `marketplace.json`** descriptions de-coupled from the original
  host project; versions bumped (plugin → 2.0.0, marketplace → 1.1.0).
- **`README.md` fully rewritten** — the previous one referenced removed `/team:sr-*`
  command names, nonexistent `sr-*.md` files, and a wrong install path.

### Added

- **`/team:qa`** — Senior QA / Test Engineer: test strategy, coverage gaps, regression
  risk, and edge-case analysis.
- **`/team:all`** — Engineering Team orchestrator: detects the stack once, then
  dispatches all five specialists as parallel subagents and synthesizes one
  de-duplicated, globally-prioritized report with a ship verdict.
- **`reference/stack-detection.md`** — the full shared stack-detection protocol.
- **`reference/review-rubric.md`** — shared severity definitions and output format.
- **Opt-in ecosystem context** — loaded only inside the host organization's repos
  (migrated from the old hardcoded section in `architect.md`). Later genericized in 2.3.0.
- **`CHANGELOG.md`** — this file.

## [1.0.0]

- Initial release: five project-specific personas (`/team:review`,
  `/team:frontend`, `/team:backend`, `/team:database`, `/team:architect`) with
  hardcoded stack assumptions.
