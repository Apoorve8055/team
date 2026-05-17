# Ecosystem Context

Opt-in cross-app context for the `/virtual-team:*` personas. A persona loads this file only
when stack detection indicates the current project belongs to a multi-app ecosystem
**documented below** — and ignores it otherwise.

> **This is a customizable template, not ground truth.** Out of the box the example
> below is inert — no persona will match it against a real repo. To make it useful,
> replace the example with your own organization's multi-app context. Stacks drift, so
> even once customized: always verify against the actual repo — detected facts beat
> this map.

## How it works

The shared `<stack-detection>` step in every persona ends with: *"If the project is
part of a documented multi-app ecosystem, you may read `reference/ecosystem-context.md`
for cross-app context."* A persona consults this file when it detects — by git remote,
package scope, or workspace path — that it is inside an ecosystem you have documented
here. The `/virtual-team:architect` and `/virtual-team:all` personas benefit most: it lets them reason
about service boundaries and shared infrastructure that a single repo doesn't reveal.

If you maintain a single standalone project, you can leave this file as-is (the example
never matches a real repo) or delete it.

## Customizing it

Replace everything below the marker with your own ecosystem. Useful sections:

- **Match signals** — how a persona recognizes a repo as part of your ecosystem (a git
  remote substring, a package scope like `@yourorg/*`, a known workspace path).
- **Apps / services** — a table of the apps, their domains, purpose, and known stack.
- **Shared infrastructure** — auth, databases, deployment, design system: what is
  shared and what is isolated.
- **Architectural principles** — the rules that hold ecosystem-wide (service
  boundaries, shared identity, config conventions).
- **How to use it in a review** — the cross-app concerns a persona should flag.

<!-- ============================================================== -->
<!-- EXAMPLE — replace everything below with your own org's context  -->
<!-- ============================================================== -->

## Match signals (example)

This repo is part of the example ecosystem if the git remote contains `acme`, a package
uses the `@acme/*` scope, or the path is under a known `acme-workspace/` directory.

## Apps (example)

| App | Domain | Purpose | Stack (as last known) |
|-----|--------|---------|------------------------|
| **web** | app.example.com | Main customer app | Next.js, Postgres |
| **admin** | admin.example.com | Internal admin console | Next.js, Postgres |
| **api** | api.example.com | Shared GraphQL backend | Node, GraphQL |

## Shared across apps (example)

- **Auth:** a single shared identity provider with session sharing across subdomains —
  apps don't build custom auth.
- **Database:** each app owns an **isolated** database; apps never reference another
  app's tables.
- **Deployment:** one shared hosting platform for all apps.
- **Design system:** a shared component library and brand tokens.

## Architectural principles (example)

- **Service boundaries:** each app owns its data; cross-app communication goes through
  defined APIs, never shared tables.
- **Shared identity:** session sharing means auth state crosses subdomains — account
  for that when reasoning about a single app.
- **Config conventions:** prefer database or config files over environment variables
  for application config; environment variables are for secrets and connection strings.

## How to use this in a review (example)

When reviewing a repo in this ecosystem, check whether a change:
- crosses a service boundary (reaches into another app's data or assumes its schema),
- weakens the shared auth boundary,
- introduces config that should live in the database instead of env vars.

Flag those as boundary concerns — but always confirm against the repo's own `CLAUDE.md`
and code first.
