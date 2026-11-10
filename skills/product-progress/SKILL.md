---
name: product-progress
description: Produce a PM-facing progress brief across every in-flight feature spec and PAD — which acceptance criteria are satisfied, tested, open, or drifting; which PADs are stuck between stages; which milestones are slipping; and which decisions need the PM today — by reading the repo's specs/ and pads/ folders plus Jira, GitHub, Linear, or whatever tracker and code host are connected. Use this whenever a PM asks for status, a standup or weekly update, "where are we", "what's blocked", "what needs me", "what did the agents ship", a roadmap health check, or a brief to send to leadership; when setting up a recurring daily or weekly product brief; or when there are too many agent PRs and notifications to read. Trigger even when the user just says "progress" or "status" with no object.
---

# Product Progress

In spec-driven, agent-assisted development the unit of progress is **acceptance criteria passing**, not tickets closed or story points burned. Tickets measure activity; ACs measure whether the thing the PM specified exists. This skill reads the repo's specs and PADs as the source of truth, overlays the tracker and code host for what's moving, and produces a brief a PM can act on in five minutes: what shipped, what's drifting, what's blocked, and the decisions only the PM can make.

It is the fourth skill in the chain. `product-pad` decides, `product-spec` defines, `spec-validate` confirms a single PR, `product-progress` shows the whole board.

## Sources, in priority order

1. **`specs/*/spec.md`** — status, ACs, open questions, milestone dates (from the linked PAD), changelog. This is the truth about *what* is being built.
2. **`pads/*.md`** — stage, Decision Log outcomes, milestones (§2.7). Truth about *whether* and *when*.
3. **Code host** (GitHub via connector or `gh`, GitLab, local git) — PRs and branches referencing spec IDs, merge state, CI state, review state. Truth about *what's moving*.
4. **Tracker** (Jira, Linear, Asana via connector) — tasks linked to spec IDs, assignees, blocked flags. Truth about *who* and *blocked by what*.
5. **Prior `spec-validate` reports** if saved in the repo (`specs/<id>/validation/*.md`) — last known AC verdicts.
6. **Chat/Slack** only when the user points at a channel; otherwise skip — it's noisy and rarely changes the brief.

Use what's connected; say plainly which sources were unavailable and what the brief therefore can't see. A brief built only from the repo is still useful; a brief that guesses at tracker state is not.

## Workflow

### 1. Scope

Default scope is every spec not in status Shipped/Closed and every PAD not Closed, for the whole repo. Narrow when the user names a team, stream, spec, or time window ("since Monday", "this sprint"). For a leadership brief, roll up to PAD level and hide AC detail; for a standup, show AC detail for anything that changed.

### 2. Collect

For each in-flight spec:
- Parse ACs and open questions from `spec.md`; note status and last changelog date.
- Find linked PRs/branches by spec ID in title, body, or branch name. Record merged / open / draft, CI status, age.
- Find linked tracker items by spec ID or label. Record status, assignee, blocked flag, last update.
- Pull the most recent `spec-validate` report if present; otherwise infer AC state conservatively from merged PRs that reference specific ACs. Never mark an AC Satisfied without either a validation report or a merged PR whose tests name it.

For each PAD:
- Current stage, last Decision Log outcome and date, next milestone from §2.7 and whether it's past.

### 3. Compute the signals

These are what the brief is *for*. Each has a definition so the numbers mean the same thing every day.

| Signal | Definition |
|---|---|
| **AC completion** | satisfied / total per spec, with untested shown separately. Not a percentage of tickets. |
| **Drift** | A merged PR touches a spec's area with no spec changelog entry in the same window; or a spec's status is Approved but a linked PR changed behavior the ACs describe. Flag it; don't resolve it — that's `spec-validate`'s job. |
| **Stale** | Spec or PAD with no changelog, PR, or tracker movement in N days (default 7 for specs, 14 for PADs). |
| **Blocked** | Tracker blocked flag, or an open question in the spec that a merged/open PR depends on, or a PR waiting on review > 2 business days. |
| **Slipping** | Any PAD §2.7 milestone date in the past without the corresponding status change. |
| **Needs PM** | Open questions with no owner or past their asked date; `spec-validate` reports with pending PM decisions; PADs awaiting a Decision Log entry after a scheduled review; unspecified work awaiting keep/remove. |
| **Agent volume** | Count of agent-authored PRs opened and merged in the window, so the PM can see the firehose without reading it. |

### 4. Write the brief

Use `assets/brief-template.md`. Lead with **Needs you today**, because that's the only section that requires the reader; everything else is context. Then shipped since last brief, then the per-spec table, then PAD stage board, then risks. Keep the whole thing under one screen for a daily brief and two for weekly. Every claim links to its source (PR, ticket, spec section). Numbers come from the signals above — don't improvise new ones.

Tone: factual, no cheerleading, no hedging. "AC completion 5/8, two untested, one reinterpreted pending PM decision (see validation 2026-11-16)" — not "good progress on saved filters!"

### 5. Deliver

- Interactive ask → reply in chat, brief inline.
- "Send to the team / leadership" → also write it to `pads/briefs/YYYY-MM-DD-<scope>.md` so it's in the repo, and post to the channel or doc the user names via whatever connector exists. Confirm before posting anything.
- Recurring ("every weekday at 8", "Monday mornings") → set up a scheduled task that runs this skill with the same scope and posts to the same place. State which approval mode it got. Keep the brief files so week-over-week comparison is possible.

### 6. Follow-through

Offer the obvious next actions, one line each: run `spec-validate` on a drifting PR, open `product-pad` to record a missing Decision Log entry, ask the open-question owners. Don't do them unasked.

## What this skill deliberately doesn't do

- **Doesn't grade code or PRs.** It reports that a PR exists and what it claims; `spec-validate` decides whether it satisfies the spec.
- **Doesn't nag individuals.** Blocked and stale items name the artifact and the owner field as recorded; it doesn't editorialize about people.
- **Doesn't invent velocity.** No story points, no burn-down, no "on track" badges unless derived from the signals above. If the data isn't there, the brief says so.
- **Doesn't replace the PAD recap.** Shipped items get one line here; the outcome assessment belongs in Stage 3.

## Bundled files

- `assets/brief-template.md` — daily/weekly brief skeleton. Same sections every time so readers can scan by position.
- `references/signals.md` — fuller definitions, thresholds, and how to compute each signal from each source type, plus how to degrade gracefully when a source is missing.
- `references/example-brief.md` — a worked weekly brief covering the example PAD/spec from the sibling skills.
