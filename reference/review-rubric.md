# Review Severity Rubric

Shared reference for every `/virtual-team:*` persona's Output Contract. Each command body
carries the short severity tiers inline; read this file when a finding is hard to
classify.

## Severity tiers

### Critical — must fix before merge
A defect that will cause incorrect behavior, data loss, a security hole, or an outage.
The change should not ship until this is resolved.

Examples: unvalidated user input reaching a query · missing authorization check on a
mutating route · a migration that drops a column with live data · a broken data
contract between API and UI · an unhandled rejection on a critical path · a secret
committed to the repo.

### Warning — should fix, not blocking
A real problem that degrades correctness, performance, accessibility, or
maintainability, but does not by itself justify blocking the merge. Fix now or file a
follow-up.

Examples: an N+1 query on a warm path · a missing index on a frequently filtered
column · a `use client` boundary wider than necessary · a missing `aria-label` on an
interactive control · an error swallowed without logging · duplicated logic that
should be extracted.

### Suggestion — optional improvement
A judgment-call improvement: naming, structure, a cleaner pattern, a small DX win. The
author can take it or leave it.

Examples: extract a helper · prefer an existing utility · tighten a type · add a
clarifying comment on non-obvious logic.

## Classification guidance

- **When unsure between Critical and Warning**, ask: *can this corrupt data, breach
  security, or break a user-facing path?* If yes → Critical.
- **When unsure between Warning and Suggestion**, ask: *is this objectively a defect,
  or a preference?* Defect → Warning. Preference → Suggestion.
- **Severity is about impact, not effort.** A one-line fix can be Critical; a large
  refactor can be a Suggestion.
- **Don't inflate.** Style nitpicks are Suggestions at most. Reserve Critical for
  things that genuinely must not ship.
- **Cite evidence.** Every finding names a `path:line` (or `path` for whole-file
  concerns) and states *why it matters*, not just *what* it is.

## Output format

```
### <Role> Review: <scope>

**Critical** (must fix before merge)
- `path:line` — issue — why it matters

**Warning** (should fix)
- `path:line` — issue — recommendation

**Suggestion** (optional)
- `path:line` — improvement

**<Role> Health**
One-paragraph assessment of the area through this role's lens.
```

If a tier has no findings, write `- None.` under it — don't omit the tier.
