# Spec Validation: `<NNN>-<slug>` ↔ <PR #n / branch / commit range>

**Verdict: Ready / Ready with spec update / Not ready**

One sentence on why. If not ready, name the blocking item(s).

| | |
|---|---|
| Spec | `specs/<NNN>-<slug>/spec.md` @ <commit or date> |
| Code | <PR link or branch> @ <commit> |
| Constitution | `.specify/memory/constitution.md` v<x> |
| Validated | YYYY-MM-DD by <name / Claude> |

## Acceptance criteria

| AC | Verdict | Implementation evidence | Test evidence | Note |
|---|---|---|---|---|
| AC-1.1 | Satisfied | `path/file.ext:L120` | `tests/…:L40` | |
| AC-1.2 | Implemented, untested | `…` | — | |
| AC-1.3 | Reinterpreted | `…` | `…` | Spec says X; code does Y |
| AC-2.1 | Missing | — | — | |
| AC-2.2 | Blocked by open question | `…` | — | Resolves OQ-1 as "<answer>" without decision |

Summary: N satisfied · N untested · N reinterpreted · N missing · N blocked, of N total.

## Unspecified work

Behavior the diff adds that no story or AC asked for. Each gets a PM decision: keep & add to spec, or remove.

| What | Where | Recommendation |
|---|---|---|
| | | Keep → add AC / Remove |

## Forbidden changes & constitution

| Rule | Touched? | Where |
|---|---|---|
| Spec §3: <forbidden item> | No / **Yes** | |
| Constitution §8: <non-negotiable> | No / **Yes** | |

## Operability (spec §8–§9)

| Item | Spec says | Code | OK? |
|---|---|---|---|
| Telemetry events | `event.a`, `event.b` | `event.a` present; `event.b` absent | No |
| Feature flag | `flag_name`, default off | | |
| Rollback | <procedure> | migrations reversible? | |

## Open questions

| OQ | Status | Resolved in code as | Decision needed |
|---|---|---|---|
| OQ-1 | Unanswered in spec | `<answer>` | PM to confirm or change |

## Recommended actions

Ordered, each owned.

1. **Eng:** …
2. **PM:** update spec — AC-x.y to read "…"; changelog entry "…".
3. **PM:** decide OQ-1.

## Spec changes applied

List edits made to the spec as part of this validation, or "none".
