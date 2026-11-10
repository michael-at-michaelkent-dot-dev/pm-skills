# Worked example

A validation report for the example spec in `product-spec` (`014-saved-test-filters`) against a hypothetical agent-authored PR. Note the tone: correspondence only, no code-quality commentary; every row cites a location; the reinterpretation gets a concrete spec edit rather than a complaint.

---

# Spec Validation: `014-saved-test-filters` ↔ PR #482

**Verdict: Not ready**

AC-2.2 is missing (dropped-environment handling), AC-1.3 reinterprets the duplicate-name behavior, and OQ-1 was resolved in code without a decision. Telemetry for one required event is absent.

| | |
|---|---|
| Spec | `specs/014-saved-test-filters/spec.md` @ 2026-11-09 |
| Code | PR #482 `feat/saved-filters` @ `a3f9c21` |
| Constitution | `.specify/memory/constitution.md` v1.2 |
| Validated | 2026-11-16 by Claude for M. Kent |

## Acceptance criteria

| AC | Verdict | Implementation evidence | Test evidence | Note |
|---|---|---|---|---|
| AC-1.1 | Satisfied | `results/filters/save.ts:41–78` | `results/filters/__tests__/save.test.ts:12` | |
| AC-1.2 | Satisfied | `results/filters/SaveButton.tsx:19` (disabled + tooltip) | `…/SaveButton.test.tsx:30` | Tooltip copy matches spec |
| AC-1.3 | Reinterpreted | `save.ts:62` | `save.test.ts:48` | Spec: prompt overwrite-or-rename. Code: silently overwrites same-name filter |
| AC-2.1 | Satisfied | `results/ResultsPage.tsx:88–104` | `ResultsPage.test.tsx:55` | |
| AC-2.2 | Missing | — | — | No handling for environments the user lost access to; filter applies raw and query returns 403 |
| AC-2.3 | Satisfied | `ResultsPage.tsx:91` (URL params short-circuit) | `ResultsPage.test.tsx:72` | |
| AC-3.1 | Implemented, untested | `results/filters/manage.ts:22` | — | Rename works; no test |
| AC-3.2 | Satisfied | `manage.ts:40–57` | `manage.test.ts:31` | Confirm dialog present; default cleared |

Summary: 5 satisfied · 1 untested · 1 reinterpreted · 1 missing · 0 blocked, of 8 total.

## Unspecified work

| What | Where | Recommendation |
|---|---|---|
| Keyboard shortcut `S` opens save dialog | `SaveButton.tsx:44` | Keep → add AC-1.4 and an accessibility note, or remove; PM decision |
| Saved filters sorted by most recently used | `manage.ts:70` | Spec §6 Defaults is silent on list order. Keep → add to §6 Defaults |

## Forbidden changes & constitution

| Rule | Touched? | Where |
|---|---|---|
| Spec §3: results query semantics / API contract | No | — |
| Spec §3: permissions unchanged | **Yes, indirectly** | AC-2.2 gap means a saved filter can attempt a query against an environment the user can't access. Not a permission grant, but the spec's intent was that this never surfaces |
| Constitution §8: no direct DB access from UI layer | No | — |

## Operability (spec §8–§9)

| Item | Spec says | Code | OK? |
|---|---|---|---|
| Telemetry | `filter.saved.created`, `filter.saved.applied` (source), `filter.saved.deleted`, `results.filtered_view_shown` | First three present (`telemetry.ts:14–31`); `results.filtered_view_shown` absent | **No** — this is the event the PAD baseline depends on |
| Feature flag | `saved_filters`, default off | `flags.ts:9`, default off | Yes |
| Rollback | Flag off hides UI; no migration to reverse | New table `user_saved_filters` added via forward-only migration; flag-off leaves it inert | Yes |

## Open questions

| OQ | Status | Resolved in code as | Decision needed |
|---|---|---|---|
| OQ-1: apply default on dashboard deep links with no params? | Unanswered in spec | Yes — deep links with no params get the default (`ResultsPage.tsx:91` treats all param-less loads alike) | PM to confirm; if confirmed, fold into AC-2.1 |

## Recommended actions

1. **Eng:** implement AC-2.2 (drop inaccessible environments, show notice naming them, no error).
2. **Eng:** add `results.filtered_view_shown` event; without it the success metric can't be measured.
3. **PM:** decide AC-1.3 — keep spec (prompt) or adopt code (overwrite). If adopting: rewrite AC-1.3 to "…Then the existing filter is replaced and the user sees a confirmation that it was overwritten", changelog "AC-1.3 relaxed to overwrite-with-confirmation after review of PR #482".
4. **PM:** decide OQ-1; on "yes", amend AC-2.1 to include deep links and remove the open question.
5. **PM:** decide on the two unspecified items.
6. **Eng (non-blocking):** test for AC-3.1.

## Spec changes applied

None — awaiting PM decisions on items 3–5.
