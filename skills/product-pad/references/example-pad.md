# Worked example (abridged)

The PAD that preceded the `014-saved-test-filters` spec in the `product-spec` example. Note the line between the two documents: this one argues *whether* and *which direction*; it never writes acceptance criteria. Note also how Stage 1 names three real directions and Stage 2 explains why one won.

---

# PAD-2026-011: Operators lose their result filters every session

| | |
|---|---|
| Stage | Solution framing |
| Status | Proceed (Stage 2, 2026-11-07) |
| Owner (PM) | M. Kent |
| Team / stream | Results & Insights |
| Design | J. Alvarez |
| Eng lead | P. Novak |
| GTM | S. Reid |
| Linked spec | `specs/014-saved-test-filters/spec.md` |
| Created | 2026-10-21 |
| Last updated | 2026-11-09 |

---

# Stage 1 — Problem framing

## 1.1 The problem / opportunity

Operations engineers check test results for a specific environment several times a day. Each time they open the Test Results page they rebuild the same combination of environment, time-range, and status filters, and any navigation away discards it. In session recordings the median time from page load to the first useful filtered view is ~40 seconds and 6 clicks; roughly one in five sessions ends before a filtered view is reached.

## 1.2 Why it matters

Daily-active use of Test Results is the leading indicator in our "operational trust" pillar (Q4 OKR 2: raise weekly active operators per account from 2.1 to 3.0). Friction at the top of this page suppresses the habit we're trying to build. Doing nothing: the metric stays flat and the top support theme for the page persists at ~310 tickets/month.

## 1.3 Target audience

Operations engineer persona (constitution glossary): responsible for 1–5 named environments, opens Test Results 3–8 times a day, usually via a bookmark or dashboard link. Not the audience: account admins configuring tests; executives viewing summary dashboards.

## 1.4 Evidence

| Claim | Evidence / source | Confidence |
|---|---|---|
| Operators rebuild the same filters each session | 14 of 22 interviews (Oct 2026) described it unprompted; session replay sample n=120 | High |
| ~40 s / 6 clicks to first filtered view | Session replay sample, median | Medium (sample skews to larger accounts) |
| 1 in 5 sessions abandon before filtering | Same sample | Medium |
| It's the top support theme for the page | Ticket tag `results-filters`, 310/mo over 3 months | High |
| Operators would use saved views if offered | 9 of 22 interviews asked for it; no usage data | Assumption — validate with prototype |

## 1.5 Success metrics

| Metric | Baseline | Target | How measured |
|---|---|---|---|
| Median time to first filtered view | ~40 s (replay sample) | < 5 s for users with a saved view | New telemetry event; instrument before build |
| Results sessions abandoned before filtering | ~20 % | < 10 % | Same |
| `results-filters` tickets per month | 310 | < 150 | Support tag |

## 1.6 Competitive & alternative landscape

Today operators cope with browser bookmarks containing filter URLs (fragile; break when filters change), or keep a tab open all day (loses state on refresh). Two competitors offer per-user saved views; one offers team-shared views. Neither applies a default view automatically, which interviews flagged as the actual time sink.

## 1.7 Potential directions

| Direction | What it bets on | Rough cost / effort | Main risk |
|---|---|---|---|
| A. Per-user saved filters with an optional default | The repeat pattern is personal and stable | S–M | Low adoption if saving is one more thing to set up |
| B. Remember last-used filters automatically, no saving | Operators want *last* state, not *named* states | S | Wrong for operators who alternate between environments |
| C. Team-shared standard views managed by admins | Consistency across a team matters more than personalization | M–L | Admin setup burden; conflicts with per-user habits |
| D. Do nothing; improve filter UI speed | Clicks are the cost, not re-entry | S | Doesn't address abandonment on navigation |

## 1.8 Open questions for discovery

- Do operators alternate between environments within a session? → analytics query on filter changes per session.
- Would a default view be welcomed or surprising? → clickable prototype with 8 interviewees.
- Is team consistency (C) a real ask or an admin assumption? → 5 admin interviews.

---

# Stage 2 — Solution framing

## 2.1 Chosen direction and why

**A. Per-user saved filters with an optional default.** Discovery showed 70 % of operators stay on one environment per session but a meaningful minority alternate, which rules out B as the only mechanism. Admins did not ask for C unprompted; it stays as a follow-on (PAD-2026-014). Prototype testing of a default view was positive in 7 of 8 sessions. Stage 1 course correction ("show the metric can be measured before building") absorbed: telemetry event ships first.

## 2.2 Key flows

Design: Figma link. From the operator's view: apply filters once, save with a name, optionally mark as default; next visit the page opens already filtered and shows which saved view is active. Managing views is a short list with rename/delete.

## 2.3 Engineering approach

Adds a per-user store for saved filter combinations. Depends on the existing filter-state model; does not change the results query or its contract. Must not alter permissions — a saved view can never expose an environment the user cannot already see. Feature-flagged.

## 2.4 Go-to-market

Positioning: "Open to what matters." Launch tier: minor. Enablement: one help article plus in-app tip on first filter use. No pricing impact.

## 2.5 Scope for this release

In: save, apply, rename, delete, default per user. Out: sharing across users (PAD-2026-014, Q1), saving sort/column layout (later, no date), saved views on other pages (later).

## 2.6 Risks & dependencies

| Item | Type | Likelihood / impact | Owner | Mitigation |
|---|---|---|---|---|
| Baseline from replay sample unrepresentative | Risk | Med / Med | PM | Re-baseline with telemetry before build completes |
| Default view surprising to returning users | Risk | Low / Med | Design | Visible "saved view active" indicator; one-click clear |
| Filter-state model refactor in flight | Dependency | Med / High | Eng lead | Sequence after refactor merges; spec notes forbidden changes |

## 2.7 Milestones

Spec approved 11/12 · telemetry live 11/14 · build complete 12/05 · beta 12/08 · GA 12/19 · recap 02/20.

## 2.8 Success metrics check

Metrics from 1.5 stand. Telemetry required: `results.filtered_view_shown`, `filter.saved.created`, `filter.saved.applied` (source: manual | default).

---

# Decision Log

| Date | Stage reviewed | Outcome | Corrections / follow-ups / required work | Reviewers |
|---|---|---|---|---|
| 2026-10-28 | Stage 1 | Approved with course corrections | Prove the metric can be measured before build; validate default-view assumption with prototype | Product leadership, 11 PMs |
| 2026-11-07 | Stage 2 | Proceed | — | Product leadership |

# Changelog

| Date | Change | By |
|---|---|---|
| 2026-10-21 | Stage 1 draft | M. Kent |
| 2026-11-05 | Stage 2 draft after discovery | M. Kent |
| 2026-11-09 | Linked spec 014 | M. Kent |
