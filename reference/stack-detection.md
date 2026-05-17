# Stack Detection Protocol

Shared reference for every `/virtual-team:*` persona. The team plugin is installed **globally**
and invoked across many different repos. A persona must **detect** the current
project's stack and conventions every time — it must never assume a stack.

Each command body carries a short inline `<stack-detection>` block. Read this file when
the project layout is unusual, ambiguous, or you hit a case the inline block doesn't
cover.

## Goal

Produce a one-line stack summary plus the project root path before doing any review or
development work. Example: `Stack: Next.js (App Router) · React · TypeScript ·
Drizzle/Postgres · pnpm — root: <project-root>`.

## Procedure

1. **Find the project root.** Walk up from cwd looking for the nearest directory that
   has a manifest (`package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`,
   `composer.json`, `*.csproj`, `Gemfile`, `pom.xml`, `build.gradle`) and/or a `.git`
   directory. That directory is the project root.

2. **Read the project's own instructions.** If the root has `CLAUDE.md`, `AGENTS.md`,
   `.cursorrules`, or `CONTRIBUTING.md`, read it — it states conventions, off-limits
   files, and commands. Project instructions override anything in this plugin.

3. **Read the nearest manifest.** Identify language, framework, and key dependencies.
   For monorepos, read both the root manifest and the workspace/package manifest
   closest to the files under review.

4. **Identify the package manager / build tool** from the lockfile:
   `pnpm-lock.yaml` → pnpm · `yarn.lock` → yarn · `package-lock.json` → npm ·
   `bun.lockb` → bun · `poetry.lock` / `uv.lock` → Poetry/uv · `go.sum` → go modules ·
   `Cargo.lock` → cargo. Use the detected manager — never assume pnpm.

5. **Scan the top-level layout.** Learn the project's *own* directory conventions
   (where source, tests, config, and shared code live). Match those conventions when
   suggesting file locations — do not impose a layout from another project. While
   scanning, note the domain signals the specialist personas rely on:
   - **CI / IaC / deploy** (`/virtual-team:devops`): `.github/workflows/`, `.gitlab-ci.yml`,
     `*.tf`, CloudFormation templates, `Dockerfile`, `docker-compose*`, `serverless.yml`.
   - **Mobile** (`/virtual-team:mobile`): `react-native` / `expo` in the manifest, `ios/` +
     `android/` directories, `pubspec.yaml` (Flutter).
   - **Data / ML** (`/virtual-team:data`): pipeline or orchestration tooling (Airflow, dbt,
     Spark, Kafka), warehouse clients, ML or LLM SDKs (`openai`, ...), eval frameworks.

6. **Handle the workspace-root case.** If cwd has **no** manifest of its own but
   contains **multiple child repos** (each with its own manifest/`.git`), there is no
   single project to detect. **STOP.** Either:
   - infer the target repo from `$ARGUMENTS` (a file path or PR clearly inside one
     child repo), or
   - ask the user which repo / path to work in.
   Do not pick one arbitrarily.

7. **Detect the test tooling** (QA persona always; others when relevant): test runner
   (`vitest`, `jest`, `pytest`, `go test`, `cargo test`, `rspec`) and e2e framework
   (`playwright`, `cypress`, `webdriver`) from the manifest and config files. Note the
   existing test directory layout and naming convention.

8. **Check for a documented ecosystem.** If `reference/ecosystem-context.md` has been
   customized with a multi-app ecosystem and the current project matches its signals
   (git remote, package scope, or workspace path), you *may* read it for cross-app
   context. If it is still the shipped example template, or the project doesn't match,
   do not load it.

## Output

State the detected stack in **one line** before proceeding. If detection fails (no
manifest found, ambiguous workspace root), say so and stop — do not guess a stack.

## Rules

- Project instructions (`CLAUDE.md` etc.) and detected facts always beat this plugin's
  examples.
- Framework names in persona files are **examples**, never assumptions.
- Re-detect every invocation — the user moves between repos.
