---
name: plan-review
description: Review an implementation plan or task breakdown (a Spec Kit plan.md and tasks.md, a Kiro design doc, a Claude Code plan-mode output, an engineer's design doc, or a pasted plan) against the feature spec it implements, from the product owner's side — checking that every acceptance criterion is covered, no scope was added or quietly dropped, no open question was resolved by assumption, forbidden changes are respected, and UX decisions that belong to the PM weren't made by the planner. Use this whenever a PM or lead asks to review, sanity-check, approve, or "look over" a plan, design doc, technical approach, or task list before build starts; asks "does this plan match the spec", "is the agent about to build the right thing", "anything missing before we start"; or when a plan has just been generated and someone asks what to do next. Trigger even when the user just pastes a plan and a spec ID.
---

# Plan Review

Between the spec and the code sits the plan — the agent's (or engineer's) statement of how it intends to satisfy the spec. It is the cheapest place to catch a misread: a wrong assumption fixed here costs a comment; fixed after implementation it costs a rebuild and a round of `spec-validate`. The engineering review of a plan asks "is this a sound way to build it." This skill asks the other question: **"is this a plan to build what we specified, and only that?"** The two reviews are different and both are needed; this skill stays strictly on the product side.

Chain position: `product-spec` → **plan** → `plan-review` → tasks → build → `spec-validate`.

## What it checks

1. **Coverage.** Every AC in the spec is addressed by at least one plan step or task. Steps that don't trace to an AC, a §6 behavior detail, or a §8 NFR are either unspecified scope or infrastructure — label which.
2. **Fidelity.** Where the plan restates behavior, it matches the AC. Watch for plans that paraphrase an AC into something slightly different ("show an error" where the spec says "keep filters applied and offer retry").
3. **Assumption leaks.** Any `[NEEDS CLARIFICATION]` in the spec that the plan treats as decided. The plan should route around open questions or stop at them, not answer them.
4. **Scope discipline.** Additions the spec didn't ask for (adjacent features, "while we're in there" refactors that change behavior) and omissions (ACs deferred to "a follow-up" without the spec saying so).
5. **Forbidden changes and constitution.** Steps that touch spec §3 forbidden items or constitution non-negotiables, including indirectly (a shared component, a contract, permissions).
6. **PM-owned decisions made by the planner.** Defaults, copy, ordering, what happens on failure, which persona gets which experience — anything in spec §6 territory the spec left silent and the plan filled in. These aren't wrong to raise; they're wrong to decide silently. Each becomes a spec addition or an explicit PM call.
7. **Operability presence.** Telemetry from §8, flag and rollback from §9, and the tests the plan intends — specifically whether each AC has a planned test. A plan with no testing section is incomplete regardless of architecture quality.
8. **Size.** A plan whose task list implies weeks of work for a spec that reads as days is a signal that either the spec is bigger than it looks or the plan is over-building. Flag it; don't resolve it.

## Workflow

### 1. Get both sides

Read the spec (`specs/<id>/spec.md`) fully; build the AC list and open-question list. Read the constitution's forbidden/non-negotiable sections. Read the plan and tasks — Spec Kit's `plan.md` and `tasks.md`, Kiro's `design.md`/`tasks.md`, a Claude Code plan-mode output, a design doc, or pasted text. If the plan references files or modules, glance at them only enough to understand what the step changes; this is not a code review.

If there is no spec, stop: there's nothing to review against, and a plan reviewed on its own merits is an engineering review. Point at `product-spec`.

### 2. Build the trace

Make a two-way map: AC → plan step(s) → planned test(s), and plan step → AC(s) it serves. Do it as a table, even if rough; the gaps are visible only when both directions are laid out. Use `assets/review-template.md`.

### 3. Walk the eight checks

Go through the list above against the trace. For each finding, record: what the plan says, what the spec says, and the action (plan change / spec change / PM decision / fine as is). Be specific enough that the planner can act without asking what you meant.

### 4. Write the review

Lead with a one-line verdict:

- **Approve** — all ACs covered with tests planned, no assumption leaks, no forbidden touches, no silent PM decisions. Notes allowed.
- **Approve with changes** — list exact edits to the plan (and any spec edits to make PM decisions explicit). Build may start on unaffected tasks.
- **Rework** — coverage gaps, assumption leaks on blocking questions, or forbidden changes. Name the smallest set of fixes that would get to Approve-with-changes.

Then the trace table, then findings grouped by check, then actions owned by role. Keep it to one screen. Say nothing about code style, framework choice, or architecture elegance — those belong to engineering's review, and mixing them in makes both reviews less sharp.

### 5. Apply spec changes if agreed

If a finding is "the plan made a reasonable call the spec left open" and the PM agrees, promote it into the spec now: edit §6 Defaults or the relevant AC, add a changelog row. That way the agent builds from a spec that says it, and `spec-validate` later won't flag it as a reinterpretation. Show the diff. Never edit silently.

### 6. Hand off

Tell the user what happens next: planner applies changes → tasks → build. Offer to post the review as a comment on the plan PR or in the plan file (`specs/<id>/plan-review.md`) so it's in the repo next to the plan it reviewed.

## Working with agent-generated plans

- **They're confident and complete-looking.** Read the trace, not the prose. A well-formatted plan with a missing AC is the common case.
- **They answer open questions by default.** Search the plan for the subject of each `[NEEDS CLARIFICATION]`.
- **They add tests for what they built, not what was asked.** Check that planned tests reference ACs, not just functions.
- **They scope-creep helpfully.** "Also added keyboard shortcut" is a PM decision, not a plan detail.
- **They sometimes restructure.** A plan that proposes refactoring something in the forbidden list "to make this cleaner" needs a human yes before it's a task.

## Bundled files

- `assets/review-template.md` — verdict, trace table, findings by check, actions by owner.
- `references/example-review.md` — a worked "Approve with changes" review of a plan for the `014-saved-test-filters` example spec, showing the trace table and the tone.
