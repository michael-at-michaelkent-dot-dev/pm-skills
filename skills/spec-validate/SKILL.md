---
name: spec-validate
description: Check implemented code against its feature spec — a pull request, branch, diff, or merged commit range versus the `specs/NNN-slug/spec.md` it claims to implement — and report, per acceptance criterion, what is satisfied, what has no test, what was silently reinterpreted, and what was built that no criterion asked for. Use this whenever someone asks to review, verify, validate, or accept a PR or branch against a spec, PRD, requirements, or acceptance criteria; asks "does this match the spec", "is this done", "did the agent build what we asked", "what's missing before we ship"; wants to catch spec drift; or asks a PM-side review of agent-written code. Also use before marking a spec Shipped, and when a spec needs updating because behavior changed on purpose. Trigger even when the user just pastes a PR link and a spec ID.
---

# Spec Validate

Agents write code faster than anyone can read it, and every PR an agent opens looks plausible. This skill is the PM-side check that keeps the spec the source of truth: not "is the code good" (engineering's review) but "is this the thing we specified." It reads the spec's acceptance criteria as the contract and the diff as the delivery, and reports the gap in a form a PM can act on in five minutes.

It is the third skill in a chain: `product-pad` decides, `product-spec` defines, `spec-validate` confirms. Its output also feeds the spec's changelog, so intentional behavior changes update the spec instead of quietly orphaning it.

## What it checks

For every acceptance criterion in the spec:

| Verdict | Meaning |
|---|---|
| **Satisfied** | The diff implements the behavior and a test (or clearly stated manual check) exercises it. |
| **Implemented, untested** | Behavior is there; nothing verifies it. Will regress. |
| **Reinterpreted** | Something is implemented *near* the AC but not what it says — a different default, a narrower condition, an error handled differently. The most dangerous category: looks done, isn't. |
| **Missing** | No evidence of the behavior in the diff. |
| **Blocked by open question** | The spec's `[NEEDS CLARIFICATION]` covering this AC was never answered, and the code picked an answer. |

Plus three cross-cutting checks:

- **Unspecified work** — behavior, UI, endpoints, or data the diff adds that no story or AC asked for. Agents fill gaps generously; each item is either scope creep to remove or a missing requirement to add to the spec.
- **Forbidden changes** — anything in the spec's §3 forbidden list or the constitution's non-negotiables that the diff touched.
- **NFR and operability** — telemetry events named in §8 present or not; feature flag from §9 present and defaulted as specified; rollback still possible given any migrations.

## Workflow

### 1. Get the two sides

**Spec side.** Locate the spec: the ID in the PR title/body or branch name (`014-saved-test-filters`), or ask. Read `spec.md` fully. Read the constitution's forbidden/non-negotiable sections. Build the list of ACs (`AC-x.y`) and open questions. If the spec has no ACs, stop — there is nothing to validate against; point the user at `product-spec`.

**Code side.** Prefer the smallest honest surface: the PR diff plus the test files it adds or changes. Use what the session offers — a GitHub connector or `gh pr diff <n>`, a local branch (`git diff main...HEAD`), or a pasted diff. For large diffs, read tests first (they say what the author believed they built), then the implementation. Don't run the full suite unless asked; this is a correspondence check, not CI.

If either side is unavailable, say exactly which and what would unblock it. Never infer the diff from a PR description — descriptions are what the author *meant*.

### 2. Map ACs to evidence

For each AC, find the code path and the test that correspond to it. Record the file and line for both so the report is checkable. Be literal about the Given/When/Then: a test that covers the When and Then but not the Given condition is partial evidence. A default that differs from §6 "Defaults" is a reinterpretation even if it's arguably better — the right fix is a spec update with a changelog entry, not silent acceptance.

Where the spec says "N/A" for an unhappy path and the diff handles it anyway, that's unspecified work (fine, but say so). Where the spec specifies an unhappy path and the diff doesn't handle it, that's missing.

### 3. Run the cross-cutting checks

Walk the diff once more for things that have no AC: new routes, new settings, new tables or fields, changed copy, changed permissions. List each. Then compare touched paths against the forbidden list. Then check NFR items that are mechanically verifiable (event names in code, flag name and default, migration reversibility).

### 4. Write the report

Use `assets/report-template.md`. Lead with the verdict line, then the table, then the three cross-cutting sections, then recommended actions. Keep it to one screen. Every row cites a file (and line where you can). Wording is about the correspondence, never about code quality, naming, or style — that's engineering's review and mixing them dilutes both.

Verdict line options:

- **Ready** — all ACs satisfied, no forbidden changes, no blocking open questions. Unspecified work and untested ACs may remain as notes.
- **Ready with spec update** — behavior is acceptable but differs from the spec; list the exact changelog entries and AC rewrites needed, and don't call it Ready until they're made.
- **Not ready** — anything Missing, Reinterpreted without agreement, Blocked by open question, or a forbidden change.

### 5. Close the loop on the spec

If the report calls for spec changes and the user agrees, make them: edit the AC text, add the changelog row with date and reason, bump the status. If the diff resolved an open question by choosing an answer, either promote the answer into the spec (if the PM agrees) or flag it as a decision the PM must make. If the verdict is Ready and the user wants it, set the spec's status to Shipped and add the changelog row. Never edit the spec silently; show the diff of what you changed.

### 6. Hand off

Tell the user what the report means for the PAD milestones and, if Ready, that the next step is launch and the Stage 3 recap. Offer to post the report as a PR comment if a connector is available, keeping it factual and in the template's format.

## Working with agent-authored PRs

A few patterns worth knowing when the author is an agent:

- **Over-delivery is common.** Agents often implement adjacent conveniences. Treat each as unspecified work; the PM decides keep-and-spec or remove.
- **Tests that assert the implementation, not the requirement.** A test named `test_saved_filter_applies` that only checks the function returned without error is not evidence for an AC. Read the assertions.
- **Quiet defaults.** The most frequent reinterpretation is a default value or sort order the spec left unstated. Report it, and recommend the spec state it going forward so the next agent doesn't guess differently.
- **Open questions get answered in code.** Search the diff for the subject of every `[NEEDS CLARIFICATION]`. If it's there, it's a decision someone needs to own.

## Bundled files

- `assets/report-template.md` — the validation report skeleton. Use its exact sections so reports are comparable across specs and over time.
- `references/verdict-guide.md` — how to decide between Satisfied, Implemented-untested, Reinterpreted, Missing, and Blocked, with examples of each.
- `references/example-report.md` — a worked report for the example spec from `product-spec`, calibrating tone and detail.
