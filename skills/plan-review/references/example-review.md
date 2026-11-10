# Worked example

A product-side review of an agent-generated plan for the `014-saved-test-filters` example spec. Had this review happened, three of the findings in the sibling `spec-validate` example report (missing AC-2.2, reinterpreted AC-1.3, OQ-1 answered in code) would have been caught before a line was written. That's the point of the skill.

---

# Plan Review: `014-saved-test-filters` — `specs/014-saved-test-filters/plan.md`

**Verdict: Approve with changes**

Coverage gap on AC-2.2, one fidelity mismatch on AC-1.3, one open question treated as decided, and one unspecified addition. Build may start on P1–P4; P5–P8 wait on the actions below.

| | |
|---|---|
| Spec | `specs/014-saved-test-filters/spec.md` @ 2026-11-09 |
| Plan | `specs/014-saved-test-filters/plan.md` + `tasks.md` @ 2026-11-11 (agent-generated) |
| Constitution | v1.2 |
| Reviewed | 2026-11-11 by Claude for M. Kent, product-side only |

## Trace: AC → plan → test

| AC | Plan step(s) | Planned test(s) | Status |
|---|---|---|---|
| AC-1.1 | P2 (store), P3 (save action) | T1 | covered |
| AC-1.2 | P3 | T2 | covered |
| AC-1.3 | P3 | T3 | **mismatch** — F2 |
| AC-2.1 | P6 | T5 | covered |
| AC-2.2 | — | — | **not covered** — F1 |
| AC-2.3 | P6 | T6 | covered |
| AC-3.1 | P5 | — | covered, no test planned — F7 |
| AC-3.2 | P5 | T4 | covered |

## Trace: plan step → AC

| Step | Serves | Classification |
|---|---|---|
| P1 migration `user_saved_filters` | §7 contract | infrastructure |
| P4 flag `saved_filters` | §9 | operability — matches spec |
| P7 keyboard shortcut `S` | — | **unspecified scope** — F4 |
| P8 list sort MRU | AC-3.x (indirect) | **PM decision** — F5 |

## Findings

### Coverage
- **F1** — AC-2.2 (default filter references an environment the user lost access to → apply remaining, show notice naming it, no error) has no step and no test. This is also the AC that protects the §3 "permissions unchanged" intent. *Action: planner adds a step after P6 and a test; treat as blocking for P6.*

### Fidelity
- **F2** — P3 says duplicate name → overwrite. AC-1.3 says prompt overwrite-or-rename, nothing saved until chosen. *Action: planner aligns P3/T3 to AC-1.3 — or, if PM prefers overwrite-with-confirmation, PM edits AC-1.3 first.*

### Assumption leaks
- **F3** — P6 applies the default view on every param-less load, including dashboard deep links. OQ-1 asks exactly this and is open. *Action: PM decides OQ-1; P6 not tasked until then.*

### Scope
- **F4** — P7 adds a keyboard shortcut. Not in any story. *Action: PM keep → new AC-1.4 with an accessibility note, or drop P7.*

### Forbidden / constitution
- None. P1–P8 don't touch the results query or permissions. (F1's absence is the indirect risk; see above.)

### PM-owned decisions made by planner
- **F5** — P8 sorts saved filters by most-recently-used. §6 Defaults is silent on order. Reasonable call; needs to be in the spec so the next agent doesn't pick alphabetical. *Action: PM confirm → add "list ordered by most recently used" to §6 Defaults, changelog entry.*

### Operability
- **F6** — Plan's telemetry step lists `filter.saved.created/applied/deleted` but not `results.filtered_view_shown`, which §8 requires and which the PAD's primary metric depends on. *Action: planner adds.*
- **F7** — AC-3.1 (rename) has no planned test. *Action: planner adds T7.*

### Size
- 8 steps / 12 tasks for a spec with 8 ACs — proportionate.

## Actions

**Planner / eng**
1. Add step + test for AC-2.2 (blocks P6).
2. Align P3/T3 to AC-1.3 as currently written, pending PM decision.
3. Add `results.filtered_view_shown` to the telemetry step.
4. Add a test for AC-3.1.

**PM**
1. Decide OQ-1 (deep links) — unblocks P6.
2. Decide F4 (keyboard shortcut) and F5 (MRU sort).
3. If changing AC-1.3 to overwrite-with-confirmation, edit the spec before P3 is tasked.

**Spec changes applied in this review**
- None yet; awaiting PM decisions 1–3.
