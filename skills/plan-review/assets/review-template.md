# Plan Review: `<NNN>-<slug>` — <plan.md / design doc / PR #n>

**Verdict: Approve / Approve with changes / Rework**

One sentence on why. If not Approve, the smallest set of fixes to get there.

| | |
|---|---|
| Spec | `specs/<NNN>-<slug>/spec.md` @ <date/commit> |
| Plan | <path or link> @ <date/commit> |
| Constitution | v<x> |
| Reviewed | YYYY-MM-DD by <name / Claude>, product-side only |

## Trace: AC → plan → test

| AC | Plan step(s) | Planned test(s) | Status |
|---|---|---|---|
| AC-1.1 | P3, P4 | T2 | covered |
| AC-1.2 | P3 | — | covered, no test planned |
| AC-1.3 | P5 | T4 | **mismatch** — see F2 |
| AC-2.2 | — | — | **not covered** |

## Trace: plan step → AC

Steps with no AC are infrastructure (fine, say so) or unspecified scope (PM decision).

| Step | Serves | Classification |
|---|---|---|
| P1 | — | infrastructure (migration) |
| P7 | — | **unspecified scope** — see F4 |

## Findings

Grouped by check. Each: plan says / spec says / action.

### Coverage
- **F1** — AC-2.2 (dropped-environment handling) has no step. *Action: planner adds step + test.*

### Fidelity
- **F2** — P5 says "overwrite on duplicate name"; AC-1.3 says prompt overwrite-or-rename. *Action: planner aligns to spec, or PM decides to change AC-1.3.*

### Assumption leaks
- **F3** — P6 applies default view on all param-less loads; OQ-1 (deep links) is open. *Action: PM decides OQ-1 before P6 is tasked.*

### Scope
- **F4** — P7 adds keyboard shortcut; not in spec. *Action: PM keep (→ new AC) or drop.*

### Forbidden / constitution
- none

### PM-owned decisions made by planner
- **F5** — P8 sorts saved list by most-recently-used; spec §6 Defaults silent. *Action: PM confirm → add to §6.*

### Operability
- **F6** — Telemetry: `results.filtered_view_shown` not in plan; required by §8. *Action: planner adds.*

### Size
- Plan implies ~N tasks / ~N days against a spec that reads as ~N. *Comment or none.*

## Actions

**Planner / eng**
1. …

**PM**
1. Decide OQ-1 …
2. Decide F4, F5 …

**Spec changes applied in this review**
- none / list with changelog entries
