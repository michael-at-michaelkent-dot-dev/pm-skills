# Worked example (abridged)

A finished spec for a small feature, to calibrate granularity. Note what is *not* here: no component names, no endpoints, no library choices. Everything the agent needs to decide is either pinned down or marked as an open question.

---

# Feature Spec: Saved Test Filters

| | |
|---|---|
| Spec ID | `014-saved-test-filters` |
| Status | Approved |
| Owner (PM) | M. Kent |
| Source PAD | PAD-2026-11 "Operators re-apply the same filters every session" (approved 2026-11-03) |
| Constitution | `.specify/memory/constitution.md` v1.2 |
| Last updated | 2026-11-09 |

## 1. Problem & Users

**Problem.** Operators reviewing test results re-apply the same combination of environment, time range, and status filters at the start of every session. Support call reviews show a median of 6 clicks and ~40 seconds before the first useful result is visible, and operators report abandoning the results page when they lose their filters after navigation.

**Primary persona.** Operations engineer (→ constitution glossary) checking the health of a named environment several times a day, usually from a dashboard link.

**Secondary personas.** Account admin who sets up standard views for a team. None others.

## 2. Success Metrics

| Metric | Baseline | Target | How measured |
|---|---|---|---|
| Time from results page load to first filtered view | 40 s median (session replay sample, Oct 2026) | < 5 s median for users with ≥1 saved filter | `results.filtered_view_shown` event minus page-load event |
| Share of results-page sessions using a saved filter | 0 (feature absent) | ≥ 40 % by 60 days post-GA | `filter.saved.applied` / results sessions |

## 3. Scope

### In scope
- Save the current filter combination under a name, per user.
- Apply, rename, delete a saved filter.
- Default saved filter per user, applied automatically on page load.

### Non-goals
- Sharing saved filters across users or teams (tracked separately as PAD-2026-14).
- Saving sort order or column layout.
- Saved filters anywhere other than the Test Results page.

### Forbidden changes
- No change to the existing filter query semantics or the results API contract.
- No change to permissions: a saved filter never grants access to environments the user can't already see.

## 4. Domain Terms

| Term | Definition |
|---|---|
| Filter combination | The set of currently applied values across environment, time range, and status, as a single unit. |
| Default filter | The one saved filter per user that applies automatically when the Test Results page opens without filter parameters in the URL. |

## 5. User Stories & Acceptance Criteria

### US-1: As an operations engineer, I want to save my current filters so I can re-apply them later.

- **AC-1.1** Given at least one filter is applied, When the user chooses "Save filter" and enters a name of 1–60 characters, Then the filter combination is saved under that name and appears in the saved-filters list.
- **AC-1.2** Given no filters are applied, When the user opens the save action, Then the action is unavailable and shows the reason "Apply at least one filter to save".
- **AC-1.3** Given a saved filter with the same name already exists for this user, When the user saves, Then they are asked to overwrite or choose a different name; nothing is saved until they choose.

### US-2: As an operations engineer, I want one saved filter to apply automatically.

- **AC-2.1** Given a user has marked a saved filter as default, When they open the Test Results page with no filter parameters in the URL, Then that filter is applied before results load and the filter bar shows the saved filter's name.
- **AC-2.2** Given a default filter references an environment the user no longer has access to, When the page loads, Then the filter applies the remaining values, shows a notice naming the dropped environment, and does not error.
- **AC-2.3** Given the URL contains filter parameters, When the page loads, Then the URL parameters win and the default filter is not applied.

### US-3: As an operations engineer, I want to manage my saved filters.

- **AC-3.1** Given a saved filter exists, When the user renames it to a valid name, Then the new name appears immediately in the list and in the filter bar if it is currently applied.
- **AC-3.2** Given a saved filter is the default, When the user deletes it, Then they are asked to confirm, and on confirm there is no default until they set a new one.

## 6. Behavior Details

### Empty / zero states
Saved-filters list with none saved shows one line explaining what saving does and how to do it.

### Validation & error handling
Names: 1–60 characters, trimmed, any Unicode; duplicates per user are case-insensitive. If saving fails, the applied filters stay applied and the user sees a retry option; nothing is partially saved.

### Permissions & roles
Saved filters are private to the creating user. Admins have no visibility into other users' saved filters in this release.

### Concurrency, limits, timeouts
Max 25 saved filters per user; attempting a 26th explains the limit. Two tabs editing the same filter: last write wins, no merge.

### Defaults
No default filter until the user sets one. New saved filters are not default.

## 7. Data & Contracts

| Contract | Read / Write / New / Changed | Notes |
|---|---|---|
| Per-user saved filter store | New | Name, filter combination, is-default, created/updated timestamps |
| Test Results query | Read, unchanged | Forbidden to change — see §3 |

## 8. Non-Functional Requirements

- **Performance:** applying a saved filter adds no more than 100 ms over applying the same filters manually (p95).
- **Security & privacy:** saved filters are user-scoped; filter values may include environment names and are not considered sensitive beyond existing access rules.
- **Telemetry:** events `filter.saved.created`, `filter.saved.applied` (with source: manual | default), `filter.saved.deleted`, plus `results.filtered_view_shown` if not already emitted.
- **Accessibility, localization, compliance:** constitution defaults.

## 9. Rollout & Rollback

- **Feature flag:** `saved_filters`, default off.
- **Rollout:** internal → 10 % → 100 % over two weeks.
- **Rollback trigger:** error rate on the Test Results page above 2× baseline for 30 minutes, or any report of a filter exposing an unauthorized environment.
- **Rollback procedure:** flag off hides the UI; stored filters are retained and inert. No migration to reverse.

## 10. Dependencies & Risks

| Item | Type | Owner | Mitigation |
|---|---|---|---|
| Session-replay sampling may not be representative of baseline | Risk | PM | Re-measure baseline with telemetry once `results.filtered_view_shown` ships, before comparing |

## 11. Open Questions

- [NEEDS CLARIFICATION: Should the default filter also apply when arriving from a dashboard deep link that has no filter params, or only from top-level navigation?] — PM, asked 2026-11-09

## Appendix A — Suggested implementation notes (non-binding)
None.

## Appendix B — Changelog

| Date | Change | By |
|---|---|---|
| 2026-11-09 | Initial approved version from PAD-2026-11 | M. Kent |
