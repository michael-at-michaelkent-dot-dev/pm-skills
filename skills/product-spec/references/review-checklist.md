# Spec Review Checklist

Run before handing a spec to an agent, and in review mode on specs you didn't write. Each item has a *why* because the point is judgment, not box-ticking. Report results as pass / fixed / open.

## A. Gates — stop if any fail

1. **Problem statement names a user and a cost.** Without it the agent optimizes for whatever is easiest to build.
2. **At least one success metric has a baseline or target.** "Improve engagement" cannot arbitrate between two valid implementations.
3. **Non-goals section is non-empty and specific.** An empty non-goals section means scope will be decided by the agent.
4. **Every user story has at least one Given/When/Then acceptance criterion.** Stories without criteria are wishes.

## B. Ambiguity — the agent will resolve these silently

5. **No undefined domain terms.** Every noun with product meaning is in Section 4 or the constitution glossary.
6. **No "should", "appropriate", "reasonable", "fast", "user-friendly", "etc.", "and so on", "as needed".** Each one is a decision being delegated by accident. Rewrite or move to Open Questions.
7. **Every AC is checkable in under a minute** by a test or a human looking at the running product. If checking it needs a judgment call, it isn't a criterion yet.
8. **Unhappy paths are covered**: empty state, invalid input, permission denied, partial failure, concurrent edits, timeout / offline. "N/A" is acceptable; silence is not.
9. **Defaults are stated** for every choice the UI or API exposes.
10. **Open Questions are real questions**, each with an owner, and none of them has been quietly answered elsewhere in the spec by picking the plausible option.

## C. Separation of concerns

11. **No implementation details in the body**: no file paths, class names, framework or library choices, schema definitions, algorithm selection. Move to Appendix A or remove. The plan phase owns these, and a spec that pre-decides them stops the agent from proposing something better.
12. **Nothing restated from the constitution** (coding standards, repo layout, architecture rules). Reference, don't repeat — repeated rules drift.
13. **Design is referenced, not transcribed.** Link to flows/mocks; describe behavior the mocks don't show.

## D. Operability

14. **Telemetry requirement exists** that makes Section 2 metrics measurable after launch.
15. **Rollback trigger is an observable condition**, not "if there are problems".
16. **Forbidden changes are listed** for anything adjacent that is easy to break — auth, billing, public contracts, shared components.

## E. Shape

17. **Follows the template section order and headings.** Agents and reviewers navigate by structure.
18. **Length is a few hundred lines or less.** Longer means two features; split.
19. **One spec ID, next free number, kebab slug**, and the file lives next to the code.
20. **Changelog has an entry** for this revision.

## Reporting format

```
Review: <spec id>
Pass: A1–A4, B5, B7–B9, C11–C13, D14–D16, E17–E20
Fixed: B6 (rewrote 3 vague ACs), B10 (moved assumed default to Open Questions)
Open: D14 — no telemetry owner named; asked PM
```
