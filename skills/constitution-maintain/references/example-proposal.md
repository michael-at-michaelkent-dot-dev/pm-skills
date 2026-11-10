# Worked example — quarterly proposal (abridged)

For the example repo used across the sibling skills. Note the shape: a handful of amendments with counts behind each, one deletion block, a net length reduction, and downstream effects named.

---

# Constitution Amendment Proposal — 2027-02-02

| | |
|---|---|
| Constitution | v1.2, last amended 2026-09-30, 348 lines |
| Period reviewed | 2026-11-01 → 2027-01-31 |
| Evidence | 9 specs, 11 validation reports, 7 plan reviews, 134 PRs, CI config |
| Proposed by | Claude for M. Kent |
| Adoption path | §10: PM lead + eng lead approve; 5-day comment window; version bump |

## Summary

Drift more than bloat this quarter, though both are present. The glossary is missing three terms that six specs each had to define; one of them ("view") is defined inconsistently. Validation reports flagged the same reinterpretation — unspecified list ordering — four times across three specs, which a single default in the constitution would have prevented. Two §6 standards have no tooling and under 40 % compliance. Six amendments, two deletions; 348 → 301 lines.

## Amendments

### A1 — Add glossary terms *saved view*, *default view*, *environment scope* · Add

**Symptom.** Each defined in ≥ 3 specs (014, 016, 018 for *saved view*/*default view*; 013, 017, 019 for *environment scope*) with compatible meanings; constitution lacks all three.
**Evidence.** specs/014,016,018 §4; specs/013,017,019 §4.
**Before.** —
**After.**
> **Saved view** — a named, per-user filter combination on a results or list page. **Default view** — the one saved view per user per page applied automatically when the page opens without filter parameters. **Environment scope** — the set of environments a user is permitted to see; every query and view is bounded by it.
**Expected effect.** Six specs drop their §4 entries; future specs reference instead of redefine.
**Risk.** None material.

### A2 — Reconcile *view* vs *saved view* · Change

**Symptom.** Spec 016 uses *view* to mean a saved filter; specs 014 and 018 use *saved view*; the UI copy shipped as "Saved filters". Three names for one thing across two shipped features.
**Evidence.** specs/014,016,018; launch-pack for 014 (`specs/014/launch/enablement.md`).
**After.** Internal term *saved view* (A1); customer-facing name "saved filter" recorded in §3 as the public form.
**Expected effect.** Stops the next spec picking a fourth name.
**Risk.** Spec 016 needs a §4 edit (downstream).

### A3 — Add UX default: list ordering · Add

**Symptom.** Four Reinterpreted findings across specs 014, 016, 018 where the spec was silent on list order and the agent chose (MRU twice, alphabetical once, creation-date once).
**Evidence.** validation reports 014 (11-16), 016 (12-09), 018 (01-14, 01-21).
**After.** New §7 line:
> Lists of user-created items default to most-recently-used order unless the spec states otherwise.
**Expected effect.** Specs stop restating it; agents stop guessing; validation stops flagging it.
**Risk.** Spec 016 shipped alphabetical; either accept the inconsistency in 016 or file a follow-up.

### A4 — Relax §4 boundary on results-query module · Change

**Symptom.** "Results query module must not change" was crossed with approval in 3 of 3 PRs that touched it this quarter (all additive, all reviewed by the data owner).
**Evidence.** PRs #491, #508, #522; validation reports note "forbidden, approved by data owner".
**After.**
> The results-query module's existing behavior and contract must not change. Additive changes are permitted with review by the data owner (CODEOWNERS enforced).
**Expected effect.** The rule describes what the team actually does, so it regains authority. CODEOWNERS entry added.
**Risk.** Low; makes implicit practice explicit.

### A5 — Remove §6 rule "all PRs include a changelog fragment" · Remove

**Symptom.** Not tooled; 11 of 30 sampled PRs comply. Release notes are now generated from specs via `launch-pack`, which made the fragment redundant.
**Evidence.** PR sample Jan 2027; CI config has no check.
**After.** —
**Expected effect.** −6 lines; one less rule agents read and ignore.
**Risk.** None; replaced by a better process.

### A6 — Add non-negotiable: environment scope in every query · Add

**Symptom.** Validation report 018 (01-14) marked Not ready: a default view could issue a query outside the user's environment scope. Near miss; no rule covers it explicitly.
**Evidence.** specs/018/validation/2027-01-14.md.
**After.** §8:
> Never issue a query or render data outside the requesting user's environment scope, including from saved or default views.
**Expected effect.** Agents treat it as a hard stop rather than inferring it from permissions language.
**Risk.** None.

## Deletions

| Removed | Lines | Reason |
|---|---|---|
| §6 changelog-fragment rule (A5) | 6 | redundant; replaced by launch-pack |
| §4 paragraph describing the retired `legacy-export` service | 14 | service removed Nov 2026 |
| §6 duplicate of the testing rule (also in §9 step 5) | 9 | duplicate |
| §7 three-paragraph rationale for the p95 target | 18 | moved to a linked ADR; one line kept |

## Length delta

| Section | Before | After |
|---|---|---|
| §3 Glossary | 41 | 49 |
| §4 Architecture | 62 | 50 |
| §6 Standards | 71 | 56 |
| §7 NFR defaults | 48 | 32 |
| §8 Non-negotiables | 9 | 11 |
| Other | 117 | 103 |
| **Total** | **348** | **301** |

## Downstream effects

| Spec | Conflict | Action |
|---|---|---|
| 016 | uses *view*; shipped alphabetical order | §4 edit to *saved view*; decide whether to align ordering (follow-up spec or accept) |
| 014, 018 | define terms now in constitution | drop §4 entries; changelog row |
| 013, 017, 019 | define *environment scope* | drop §4 entries |

## Decision log entries (ready to paste on adoption)

| Date | Decision | Why | Supersedes |
|---|---|---|---|
| | A1–A6 as above | quarterly review 2027-02-02 | v1.2 text |
