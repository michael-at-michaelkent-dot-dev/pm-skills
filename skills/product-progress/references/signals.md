# Signals — definitions, thresholds, computation

The brief is only trustworthy if the same word means the same thing every day. These are the definitions. Thresholds are defaults; if the team sets different ones in the constitution (§9 workflow) or asks, use theirs and say so in the brief header.

## AC completion

**Definition.** For a spec, count of ACs with verdict Satisfied over total ACs. Report "Implemented, untested" as a separate count, never folded into done.

**Sources, best to worst.**
1. Latest `spec-validate` report in `specs/<id>/validation/`. Use its per-AC verdicts directly.
2. Merged PRs whose body or test names reference the AC id (`AC-2.1`) — count as Satisfied only if the PR is merged and CI passed. Count as Implemented-untested if merged with no test referencing the AC.
3. Nothing else counts. A tracker ticket marked Done is not evidence an AC is satisfied; show it under tracker status instead.

**Degraded mode.** No validation reports and no AC references in PRs → report "AC state unknown; N PRs merged referencing this spec" and recommend running `spec-validate`.

## Drift

**Definition.** Code describing a spec's behavior changed without the spec changing. Two detectors:
- A PR referencing the spec merged after the spec's last changelog date **and** the PR body or `spec-validate` report mentions a reinterpretation, changed default, or new behavior.
- A PR touches files in the spec's §7 contracts or §3 forbidden areas (when those name paths or modules) with no corresponding spec changelog entry.

Drift is a flag, not a verdict. The brief says "possible drift, run spec-validate"; it does not decide.

## Stale

**Definition.** No changelog entry, PR activity, or tracker update within the threshold.
- Specs: 7 days default.
- PADs: 14 days default in Stage 1 or 2; Stage 3 pending recap: flag when the §2.7 recap date is past.

Stale is informational unless combined with an open milestone, in which case it's also slipping.

## Blocked

**Definition.** Any of:
- Tracker item linked to the spec carries a blocked status or flag.
- An open question in the spec whose subject appears in an open PR (the PR is waiting on a decision, whether or not anyone said so).
- A PR referencing the spec has requested review and no review activity for > 2 business days.
- A `spec-validate` report with verdict Not ready whose actions are all assigned to PM and unaddressed.

Name the blocker, not a person: "blocked on OQ-1 (owner: PM)", "blocked on review of #482 (requested 11-14)".

## Slipping

**Definition.** A PAD §2.7 milestone date is in the past and the corresponding state hasn't happened:

| Milestone | Corresponding state |
|---|---|
| Spec approved | spec status Approved |
| Build complete | all ACs Satisfied or Implemented-untested |
| Beta / GA | spec status Shipped, or flag rollout recorded |
| Recap | PAD Stage 3 filled |

Report the days late. Don't forecast a new date — that's the team's call.

## Needs PM

**Definition.** The union of:
- Open questions with no owner, or whose asked date is > 5 business days old.
- Pending PM actions in any `spec-validate` report (items labelled PM in "Recommended actions" with no spec change applied since).
- Unspecified work awaiting keep/remove.
- PADs with a review date passed (from the user's calendar if connected, else from a "review scheduled" note in the PAD) and no Decision Log entry.
- Any spec whose status is Draft for > 10 business days.

This section is ordered by cost of delay: things blocking a PR first, then slipping milestones, then hygiene.

## Agent volume

**Definition.** PRs in the window whose author is a known agent account or whose body carries an agent attribution trailer. Report opened, merged, and awaiting review > 2 business days, plus the oldest unreviewed. This is the firehose gauge; it doesn't judge the PRs.

## Source-specific notes

**GitHub (connector or `gh`).** Search PRs by spec id in title/body/branch: `gh pr list --search "014-saved-test-filters" --state all --json number,title,state,mergedAt,reviewDecision,statusCheckRollup,author,body`. Verify flags against `gh --help` output if unsure; don't assume.

**Jira (connector).** JQL on a spec-id label or a custom field; fall back to text search on the id. Pull status, assignee, flagged, updated.

**Linear / Asana.** Equivalent: search by id text, read state, assignee, blocked relations.

**Local git only.** `git log --since=<window> --grep=<spec-id>` and branch names. No review or CI state; say so.

## Graceful degradation

Always list unavailable sources in the brief header. Then:
- No code host → show spec/PAD state and tracker state; mark AC completion as unknown.
- No tracker → omit assignee and blocked-by-tracker; keep blocked-by-open-question and blocked-by-review.
- No repo access → the brief can't run; ask for the repo or for the specs folder to be attached.
