---
name: product-spec
description: Turn a PM's problem framing (a PAD, a PRD, a brief, meeting notes, or a few paragraphs) into an agent-ready feature specification for AI-native, spec-driven development — plus a repo-level constitution when none exists. Use this whenever a product manager or engineer wants to write, draft, convert, review, tighten, or "make agent-ready" a spec, PRD, feature requirements, acceptance criteria, user stories, or a Spec Kit `spec.md`; whenever someone says a spec is "too vague for the agent" or asks what to hand Claude Code before building; and whenever a team is setting up spec-driven development, a project constitution, or `.specify/` for the first time. Trigger even if they only say "write up the requirements" or paste a PAD and ask what's next.
---

# Product Spec (agent-ready)

In AI-native development the spec is the control surface for coding agents, not a persuasion document for humans. Agents don't absorb ambiguity in standups — they fill every gap with a plausible default. This skill produces specs where the gaps are closed, the behavior is testable, and the agent knows what it may *not* do.

It is built to sit downstream of a problem-framing document (a Miro-style PAD, a PRD, or similar) and upstream of an agent's plan and tasks (GitHub Spec Kit, Kiro, or plain Claude Code). It is compatible with Spec Kit: the output drops into `specs/<NNN>-<slug>/spec.md` and the constitution into `.specify/memory/constitution.md`.

## Workflow

### 1. Classify the request

- **Constitution missing** — the repo has no `.specify/memory/constitution.md`, `CONSTITUTION.md`, `CLAUDE.md`, or equivalent describing scope, domain glossary, and architectural boundaries. Offer to write one first from `assets/constitution-template.md`; a feature spec without a constitution forces every spec to restate the same rules and they drift apart. If the user declines, proceed but note which constitution-level facts the spec is carrying temporarily.
- **New feature spec** — the main path, steps 2–6.
- **Review / tighten an existing spec** — skip to step 5 and run the checklist against what they have.
- **Convert a PAD or PRD** — same as a new spec, but mine the source document first (step 2) and quote nothing you can't trace to it.

### 2. Gather inputs (and refuse to guess the ones that matter)

Read whatever the user supplied: the PAD/PRD, repo context, Jira tickets, design links, meeting notes. Then check for the inputs below. The first four are gates — a spec written without them is a spec the agent will complete with invented intent, so stop and ask rather than fill them in yourself. Ask all missing gate questions in one message; don't drip-feed.

| Input | Gate? | Why the agent needs it |
|---|---|---|
| Problem statement and target user/persona | Yes | Grounds every UX trade-off the agent will make |
| Success metric(s) with a baseline or target | Yes | Lets the agent and reviewers choose between valid implementations |
| Non-goals and forbidden changes | Yes | The scope fence. Negative constraints are the highest-leverage lines in an agent spec |
| At least one happy-path user story | Yes | Everything else hangs off it |
| Domain glossary and business rules | Pull from constitution if present | Prevents invented semantics for domain terms |
| Data/API contracts touched (or "none") | Ask | Agent needs to know where the boundaries are |
| Non-functional requirements (perf, security, compliance, telemetry, accessibility) | Ask; default to constitution | Otherwise defaulted to nothing |
| Rollout, feature-flag, and rollback expectations | Ask | Testable checkpoints are a spec section now, not an eng afterthought |
| Open questions | Collect | Marked questions make the agent stop and ask instead of guessing |

What the PM should **not** supply, and what you should strip if they do: implementation design, file paths, library or framework choices, class names, database schema. Those belong to the plan phase and to engineering. If the source contains them, move them to a "Suggested implementation notes (non-binding)" appendix or drop them, and say which you did.

### 3. Draft the spec

Use `assets/spec-template.md` exactly — same section order, same headings. Consistency across specs is what lets agents and reviewers find things fast. Conventions:

- **Behavior, not solution.** "When a user with no saved launchers opens the Launchers page, the page shows the empty state with a 'Create launcher' action" — not "render EmptyState component".
- **Given/When/Then acceptance criteria** on every story, numbered `AC-<story>.<n>` so tests and PRs can cite them. Each criterion must be checkable by a test or a human in under a minute. "Should be fast" is not a criterion; "p95 < 400 ms at 500 concurrent sessions" is.
- **Cover the unhappy paths explicitly**: empty state, invalid input, permission denied, partial failure, concurrency, timeout. An agent that isn't told what happens on failure will pick something.
- **Define every domain term** either inline or by `→ constitution glossary`. Never introduce a term you haven't defined.
- **Mark uncertainty honestly.** Anything you were not told goes in Open Questions as `[NEEDS CLARIFICATION: ...]`, the Spec Kit convention, so the agent halts there. Do not resolve a question by choosing the sensible-sounding answer; that is exactly the failure mode this skill exists to prevent.
- **One feature per spec**, a few hundred lines at most. If it runs past that, the feature is two features. Split it and say so. Agents lose the thread in long specs and teams that kept specs lean are the ones that got value from the practice.
- **Write for a reader who has the constitution open.** Don't restate coding standards, repo layout, or architectural rules that belong there; reference them.

### 4. Place it

Default to Spec Kit layout:

```
specs/<NNN>-<kebab-slug>/spec.md
.specify/memory/constitution.md   (only when you wrote one)
```

Use the next free `NNN` in `specs/`. If the repo uses a different convention (a `docs/specs/` folder, Kiro's `.kiro/specs/<name>/requirements.md`), follow the repo. If there is no repo in the session, hand back the files and say where they go. Never put the spec only in a Confluence page or chat message — a spec that doesn't live next to the code is the one that drifts.

### 5. Self-review against the checklist

Run `references/review-checklist.md` against the draft before showing it. Fix what you can; anything you can't fix becomes an Open Question. Summarize the review in one short block at the end of your reply: what passed, what you changed, what's still open. A spec that fails the "no implementation details" or "every AC testable" items is not ready to hand to an agent regardless of how complete it looks.

### 6. Hand off

Tell the user what happens next in their pipeline, in one or two lines: for Spec Kit, `/speckit.plan` then `/speckit.tasks`; for plain Claude Code, "read specs/NNN-slug/spec.md and the constitution, then propose a plan before writing code." Offer to generate the plan prompt. Don't start implementing.

## Working with a PAD (Product Alignment Document)

PADs have three stages: problem framing, solution framing, post-launch recap. This skill consumes a problem framing that has passed its alignment review and produces the spec that *is* the solution-framing deliverable. Mapping:

| PAD section | Goes to spec section |
|---|---|
| What / why / target audience | Problem & Users |
| Success metrics | Success Metrics (must carry baseline or target) |
| Competitive landscape, directions explored | Context (brief) — do not import the rejected directions as requirements |
| Chosen direction | User Stories & Acceptance Criteria |
| Risks & dependencies | Constraints, NFRs, Rollout & Rollback |
| Anything missing | Open Questions, not invented |

If the PAD hasn't been through its review (no "proceed" outcome recorded), say so and ask whether to continue. A well-specified feature nobody approved is the expensive failure mode when agents make building cheap.

## Review mode

When handed an existing spec, don't rewrite it wholesale. Run the checklist, then return: (1) a numbered list of ambiguities an agent would resolve silently, each with the question the PM needs to answer; (2) implementation details to move out; (3) acceptance criteria that aren't testable, with a rewritten version of each; (4) missing unhappy paths. Offer to apply the fixes. The PM owns the intent; you're surfacing the places where intent is missing.

## Bundled files

- `assets/spec-template.md` — the feature spec skeleton. Copy it; don't reinvent sections.
- `assets/constitution-template.md` — repo-level constitution skeleton (scope, glossary, architecture boundaries, standards, non-negotiables).
- `references/review-checklist.md` — the pre-handoff checklist used in step 5 and in review mode.
- `references/example-spec.md` — a worked example of a finished spec, for calibrating tone and granularity. Read it the first time you use this skill in a session.
