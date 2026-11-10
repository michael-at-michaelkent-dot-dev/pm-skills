# Worked example — weekly brief

Covers the example PAD/spec from the sibling skills plus two other in-flight items, so the table has more than one row. Note: "Needs you today" first and short; every number traceable; no adjectives.

---

# Product Progress — Results & Insights — 2026-11-17 (weekly)

Window: 2026-11-10 → 2026-11-17. Sources: specs/, pads/, GitHub, Jira. Not available: none.

## Needs you today

1. **014-saved-test-filters** — decide AC-1.3: keep spec (prompt on duplicate name) or adopt code (overwrite with confirmation). PR #482 is blocked on this. Source: validation report 11-16. Waiting since 11-16.
2. **014-saved-test-filters** — answer OQ-1 (default view on dashboard deep links). Code already assumes "yes". Waiting since 11-09.
3. **PAD-2026-013** (Scheduled report exports) — Stage 1 review was 11-13; no Decision Log entry recorded. Spec can't start without it.

## Shipped since last brief

| Spec | What | PR | Validation |
|---|---|---|---|
| `012-results-telemetry` | `results.filtered_view_shown` event and dashboard | #477 | Ready (11-14) |

## In-flight specs

| Spec | Status | AC done / total | Untested | Open Qs | PRs open / merged | Last movement | Flags |
|---|---|---|---|---|---|---|---|
| `014-saved-test-filters` | Approved | 5 / 8 | 1 | 1 | 1 / 0 | 11-16 | blocked (PM decisions) · drift (AC-1.3) |
| `013-env-health-summary` | Approved | 3 / 6 | 0 | 0 | 2 / 1 | 11-15 | — |
| `015-export-scheduler` | Draft | — | — | 2 | 0 / 0 | 11-04 | stale (13 d, Draft) |

## PAD stage board

| PAD | Stage | Last outcome | Next milestone | Status |
|---|---|---|---|---|
| PAD-2026-011 (filters) | Solution framing | Proceed (11-07) | Build complete 12-05 | on time |
| PAD-2026-010 (env health) | Solution framing | Proceed (10-30) | Beta 11-24 | on time |
| PAD-2026-013 (exports) | Problem framing | — (review held 11-13, no entry) | Spec approved 11-20 | slipping risk — 3 days to milestone, no outcome recorded |
| PAD-2026-008 (alert routing) | Shipped | Proceed (09-12) | Recap 11-10 | stale — recap 7 days overdue |

## Risks & drift

- **014 / AC-1.3 drift.** PR #482 overwrites same-name filters silently; spec says prompt. Either outcome is fine; what isn't fine is leaving the spec saying one thing and the code doing another. → Decide, then `spec-validate` re-run.
- **PAD-2026-013 has no recorded review outcome.** If the review happened and the outcome wasn't logged, the 11-20 spec milestone will be missed by default. → `product-pad` to record the Decision Log entry.
- **PAD-2026-008 recap overdue.** The alert-routing success metric (acknowledgement time) has 8 weeks of data. → `product-pad` Stage 3.

## Agent activity

7 agent PRs opened, 4 merged, 1 awaiting review > 2 days (#484, 3 d, on `013-env-health-summary`).

## Next actions (offered, not taken)

- Decide AC-1.3 and OQ-1; re-run `spec-validate` on #482
- Record Decision Log for PAD-2026-013
- Start PAD-2026-008 Stage 3 recap
- Nudge review on #484
