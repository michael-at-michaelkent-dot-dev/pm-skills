# Product Progress — <scope> — <YYYY-MM-DD> (<daily | weekly>)

Window: <since last brief / date range>. Sources: specs/, pads/, <GitHub>, <Jira>. Not available: <none | list>.

## Needs you today

Decisions only the PM can make. Each with the artifact, the question, and what happens if it waits.

1. **<spec/PAD id>** — <decision>. Source: <link>. Waiting since <date>.
2. …

*(None — say so.)*

## Shipped since last brief

| Spec | What | PR | Validation |
|---|---|---|---|
| `NNN-slug` | one line | #n | Ready (date) / not run |

## In-flight specs

| Spec | Status | AC done / total | Untested | Open Qs | PRs open / merged | Last movement | Flags |
|---|---|---|---|---|---|---|---|
| `NNN-slug` | Approved | 5 / 8 | 1 | 1 | 1 / 3 | 2026-11-15 | drift · blocked |

Flags: **drift** (code changed, spec didn't) · **stale** (no movement N days) · **blocked** (what) · **slipping** (milestone past).

## PAD stage board

| PAD | Stage | Last outcome | Next milestone | Status |
|---|---|---|---|---|
| PAD-2026-011 | Solution framing | Proceed (11-07) | Spec approved 11-12 | on time / slipping / stale |

## Risks & drift

Anything from the flags above that needs more than a cell. Two or three lines each, with the recommended next action and which skill does it.

## Agent activity

<n> agent PRs opened, <n> merged, <n> awaiting review > 2 days. Oldest unreviewed: #n (<age>).

## Next actions (offered, not taken)

- Run `spec-validate` on #n (drift on `NNN-slug`)
- Record Stage 2 Decision Log entry for PAD-2026-0xx
- …
