# Verdict Guide

How to classify each acceptance criterion. The categories exist because "done" hides four different states that need four different responses. When torn between two, pick the one that triggers the more cautious action; a false "Satisfied" ships a bug, a false "Reinterpreted" costs a conversation.

## Satisfied

The diff implements the Given, the When, and the Then; a test exercises that path and asserts the Then. A manual check counts only when the spec itself marked the AC as manually verified (visual layout, say) and the PR describes the check performed.

*Example.* AC-1.1: "Given at least one filter is applied, When the user saves with a 1–60 character name, Then the combination is saved and appears in the list." Code: save handler guards on non-empty filters, validates name length, persists, list re-renders. Test: applies a filter, saves "Prod EU", asserts list contains it. → Satisfied.

Not satisfied if the test only asserts no exception, or covers one of two branches in the Then.

## Implemented, untested

The behavior is visibly in the code and plausibly correct, but no automated test or stated manual check covers it. It will work today and regress silently. Report it even when the team's definition of done doesn't require tests; the point is the PM knowing which behaviors are protected.

*Example.* AC-3.1 rename logic is present; no test renames anything. → Implemented, untested.

## Reinterpreted

Something was built in the AC's territory, but it isn't what the AC says. Common forms:

- **Different default.** Spec: new saved filters are not default. Code: first saved filter becomes default. 
- **Narrower condition.** Spec: "any filter applied" enables save. Code: requires an environment filter specifically.
- **Different failure behavior.** Spec: on save failure, keep filters applied and offer retry. Code: clears filters and shows a toast.
- **Different limit or boundary.** Spec: 1–60 chars. Code: 1–50.
- **Different ordering/visibility.** Spec: dropped-environment notice names the environment. Code: generic "some filters couldn't be applied."

Reinterpretations are sometimes improvements. That doesn't change the verdict; it changes the action. The fix is either revert the code or update the spec with a changelog entry — never accept the mismatch silently, because the next agent reads the spec, not the code.

## Missing

No code path corresponds to the AC. Check tests too: an AC with a test but no implementation is still Missing (the test is probably skipped or asserting the wrong thing). Check that it isn't hiding behind a flag or in a file the diff view collapsed before calling it Missing.

## Blocked by open question

The spec has a `[NEEDS CLARIFICATION: …]` whose subject this AC depends on, the question has no recorded answer, and the code chose one. The code may even be right. The verdict exists so the decision gets an owner. Report the answer the code implies and ask the PM to confirm or change; on confirm, promote it into the spec and the AC becomes Satisfied or Reinterpreted on the next pass.

## Cross-cutting items are not AC verdicts

Unspecified work, forbidden changes, and operability gaps go in their own sections. Don't fold them into AC rows — a PR can be "all ACs Satisfied" and still Not ready because it touched a forbidden contract.

## Deciding the overall verdict

- Any Missing, any Blocked, any unagreed Reinterpreted, or any forbidden change → **Not ready**.
- Only Reinterpreted items that the PM is willing to adopt → **Ready with spec update**, and list the exact edits; the verdict becomes Ready when they're made.
- Everything Satisfied or Implemented-untested, nothing forbidden → **Ready**, with untested ACs and unspecified work listed as notes for eng and PM respectively.

Operability gaps (missing telemetry, wrong flag default) are Not ready when the spec marked the item as required for launch, otherwise a note.
