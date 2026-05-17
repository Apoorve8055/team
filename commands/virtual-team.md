---
name: virtual-team
description: Virtual senior engineering team — list available specialists and the team orchestrator
---

# Engineering Team

A virtual senior engineering team. Each member is a persona for **code review** and
**development guidance**. Invoke with `/virtual-team:<role>`:

| Command | Role | Focus |
|---------|------|-------|
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

**Usage:**

- `/virtual-team:senior-brother` — not sure who to ask? Start here. A generalist mentor who
  figures out what you actually need, helps directly or pulls in the right specialist,
  and always tells you where you're lacking and how to level up.
- `/virtual-team:<role>` — get a focused review or development guidance from one specialist.
- `/virtual-team:all` — get a comprehensive multi-specialist review of a PR or changeset.

Each specialist works in two modes: **REVIEW** existing code (a PR, files, or a
changeset) or **DEVELOP** — guide new work. State what you need when invoking, e.g.
`/virtual-team:backend review PR 25` or `/virtual-team:database design a schema for recurring subscriptions`.
`/virtual-team:all` is review-only — for building, invoke a single specialist.

**All personas auto-detect the current project's stack and conventions** — they read
the project's manifest, instructions, and layout at invocation time, and are not tied
to any single project.
