# Team — Virtual Senior Engineering Team

[![version](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fraw.githubusercontent.com%2FApoorve8055%2Fvirtual-team%2Fmain%2F.claude-plugin%2Fplugin.json&query=%24.version&label=version&color=blue)](./CHANGELOG.md)
[![license: MIT](https://img.shields.io/badge/license-MIT-green)](./LICENSE)
[![Claude Code plugin](https://img.shields.io/badge/Claude%20Code-plugin-d97757)](https://code.claude.com/docs/en/plugins)

A Claude Code plugin that turns Claude into a virtual senior engineering team. Each
member is a slash command with a sharp expert persona, a repeatable workflow, and a
structured output contract — for **code review** and **development guidance**.

**Project-adaptive:** the personas detect the current project's stack and conventions
at invocation time (manifest, project instructions, layout). They work in any repo and
are not tied to any single project.

## Install

```
/plugin marketplace add Apoorve8055/virtual-team
/plugin install virtual-team@virtual-team
/reload-plugins
```

The 13 `/virtual-team` commands then appear in the slash-command menu — run `/virtual-team` to list them.

## Commands

| Command | Role | Focus |
|---------|------|-------|
| `/virtual-team` | — | Lists the team (this index) |
| `/virtual-team:senior-brother` | Senior Brother — generalist mentor | Help wherever you're stuck — the better solution, where you're lacking, how to level up |
| `/virtual-team:review` | Senior Full Stack Engineer | End-to-end review, cross-cutting concerns, integration between layers |
| `/virtual-team:frontend` | Senior Frontend Engineer | Component architecture, accessibility, UX states, client/server boundaries |
| `/virtual-team:backend` | Senior Backend Engineer | API design, input validation, authorization, error handling, security |
| `/virtual-team:database` | Senior Database Engineer | Schema design, indexing, query optimization, migration safety |
| `/virtual-team:architect` | Senior Software Architect | Module boundaries, dependency direction, coupling, tradeoff analysis |
| `/virtual-team:qa` | Senior QA / Test Engineer | Test strategy, coverage gaps, regression risk, edge-case analysis |
| `/virtual-team:devops` | Senior DevOps Engineer | CI/CD pipelines, IaC, deployment safety, observability, secrets/config hygiene |
| `/virtual-team:security` | Senior Security Engineer | Threat modeling, authn/authz, OWASP, supply-chain risk, secrets, data exposure |
| `/virtual-team:mobile` | Senior Mobile Engineer | Native and cross-platform apps, lifecycle, offline/sync, performance, store release |
| `/virtual-team:data` | Senior Data Engineer | Data pipelines, schema/data contracts, analytics correctness, ML/AI integration |
| `/virtual-team:all` | Engineering Team (orchestrator) | Dispatches all specialists in parallel and synthesizes one prioritized report |

## How it works

**Stack detection.** Every persona's first step is to detect the project: it finds the
project root, reads the root `CLAUDE.md` / `AGENTS.md`, reads the nearest manifest and
lockfile, and scans the layout. Project instructions and detected facts always override
this plugin's examples. The full protocol is in `reference/stack-detection.md`.

**Two modes.** Each specialist works in **REVIEW** mode (a PR, files, or a changeset)
or **DEVELOP** mode (guide new work). State which you want when invoking — e.g.
`/virtual-team:backend review PR 25` vs `/virtual-team:database design a schema for recurring subscriptions`.

**Structured output.** REVIEW mode produces a fixed contract — Critical / Warning /
Suggestion findings (each with `path:line` and rationale) plus a one-paragraph role
Health line. Severity definitions live in `reference/review-rubric.md`.

**Parallel orchestration.** `/virtual-team:all` detects the stack once, acquires the changeset
once, then dispatches the team as parallel subagents — the seven universal specialists
always, plus `mobile` and/or `data` when the detected stack calls for them (7-9 in
total) — and synthesizes their findings into one de-duplicated, globally-prioritized
report with a ship verdict. It is review-only — for building, invoke a single specialist.

**Generalist mentor.** `/virtual-team:senior-brother` is the default door — bring it anything.
It figures out what you actually need, then helps at the right depth: directly, by
adopting a `/virtual-team:` specialist, by dispatching a focused subagent, or by pointing you
at `/virtual-team:all`. Its output is the "Brother's Take" — what you actually need, the better
solution, where you're lacking, and how to level up. The last two are non-negotiable:
they're what make it a mentor rather than just a helper.

## Usage

```
# Not sure who to ask — start with your senior brother
/virtual-team:senior-brother how should I handle errors in this API?
/virtual-team:senior-brother am I doing this right?
/virtual-team:senior-brother 25

# Single-specialist review
/virtual-team:review 25
/virtual-team:frontend review the components under src/components/checkout/
/virtual-team:backend review the api routes touched in this branch
/virtual-team:database review the migration in this PR
/virtual-team:architect review the system design of the notification flow
/virtual-team:qa review PR 25

# Development guidance
/virtual-team:frontend build a date-range calendar component
/virtual-team:backend design the API for crew availability checking
/virtual-team:database design the schema for recurring subscriptions
/virtual-team:architect how should we structure the notification system?
/virtual-team:qa write a test plan for the subscription cancellation feature

# Full multi-specialist review
/virtual-team:all 25
```

## Project adaptivity

The plugin is installed globally and works in any repo. Because personas detect the
stack per invocation, the same `/virtual-team:backend` gives Next.js-aware feedback in one repo
and Django-aware feedback in another.

When the current directory is a **workspace root** containing multiple child repos
(with no manifest of its own), a persona will not guess — it infers the target repo
from your arguments (a file path or PR clearly inside one repo) or asks which repo to
work in.

## Layout

The repository **is** the plugin — it also self-hosts as its own single-plugin marketplace.

```
virtual-team/
├── .claude-plugin/
│   ├── plugin.json                # plugin manifest
│   └── marketplace.json           # self-hosting marketplace manifest (source: "./")
├── commands/
│   ├── virtual-team.md            # /virtual-team — team index
│   └── virtual-team/
│       ├── senior-brother.md      # /virtual-team:senior-brother
│       ├── review.md              # /virtual-team:review
│       ├── frontend.md            # /virtual-team:frontend
│       ├── backend.md             # /virtual-team:backend
│       ├── database.md            # /virtual-team:database
│       ├── architect.md           # /virtual-team:architect
│       ├── qa.md                  # /virtual-team:qa
│       ├── devops.md              # /virtual-team:devops
│       ├── security.md            # /virtual-team:security
│       ├── mobile.md              # /virtual-team:mobile
│       ├── data.md                # /virtual-team:data
│       └── all.md                 # /virtual-team:all
├── reference/
│   ├── stack-detection.md         # Shared stack-detection protocol
│   ├── review-rubric.md           # Shared severity rubric + output format
│   └── ecosystem-context.md       # Opt-in ecosystem context (customizable template)
├── CHANGELOG.md
├── README.md                      # this file
└── LICENSE
```

The `reference/*.md` docs are read on demand — command bodies carry short inline copies
of the shared blocks and point to the reference files by relative path, so base context
stays small.

## Maintenance

Published as a public marketplace at
[github.com/Apoorve8055/virtual-team](https://github.com/Apoorve8055/virtual-team) — the
repository is both the plugin and its own single-plugin marketplace.

- When you change a shared inline block (`<stack-detection>`, the `<process>`
  skeletons, or the Output Contract), update it in **all** command files together. The
  blocks are kept short so this stays cheap.
- On any **plugin release**, bump `version` in **both** `.claude-plugin/plugin.json`
  **and** the `virtual-team` entry inside `.claude-plugin/marketplace.json`, add a
  `CHANGELOG.md` entry, then commit and push.
- On any **marketplace-only change** (renames, owner/category changes, or anything
  that only touches `.claude-plugin/marketplace.json` outside the `plugins[]` block),
  bump the **top-level** `version` field in `.claude-plugin/marketplace.json` instead.
  That version is independent from the plugin version — track entries under it in
  `CHANGELOG.md` with a `[Marketplace x.y.z]` prefix to keep the two namespaces
  distinct.
- Users pull updates with `/plugin marketplace update` followed by `/plugin update`.

## Setup

To verify the plugin is active:

1. Restart Claude Code (`exit`, then `claude`).
2. The 13 `/virtual-team` commands appear in the slash-command menu.
3. Invoke any specialist — it should run stack detection, then act in its persona.

## Contributing

Issues and pull requests are welcome at
[github.com/Apoorve8055/virtual-team](https://github.com/Apoorve8055/virtual-team).
When changing a shared inline block, keep it byte-identical across every command file —
see [Maintenance](#maintenance) for the discipline and the release process.

## Author

**Apoorve Verma**

- Website — [apoorveverma.com](https://apoorveverma.com)
- GitHub — [@Apoorve8055](https://github.com/Apoorve8055)
- Repository — [Apoorve8055/virtual-team](https://github.com/Apoorve8055/virtual-team)

## License

[MIT](./LICENSE) © 2026 Apoorve Verma
