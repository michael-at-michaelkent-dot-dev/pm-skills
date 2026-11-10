---
name: constitution-maintain
description: Periodically audit and tend the repo's constitution (the stable context every spec and coding agent reads — scope, glossary, architecture boundaries, standards, non-negotiables, workflow) against what the specs, plans, PRs, and validation reports actually show, and propose a small set of amendments — terms to add or reconcile, boundaries to clarify or relax, rules nobody follows to remove, and bloat to cut — so the document stays lean, current, and obeyed. Use this whenever someone asks to review, update, clean up, prune, amend, or "garden" the constitution, CLAUDE.md, project rules, or agent instructions; when specs keep redefining the same term or agents keep hitting the same boundary; when the constitution has grown past ~300 lines; when setting up a quarterly review; or when a new team or repo is adopting an existing constitution. Also when someone asks why agents keep making the same mistake.
---

# Constitution Maintain

The constitution is the highest-leverage document in a spec-driven repo: every spec references it and every agent reads it before touching code. It also has two failure modes that creep in quietly. It **bloats** — every incident adds a rule, every spec adds a term, until agents lose the thread and start ignoring it. And it **drifts** — the glossary says one thing, the last six specs say another; a boundary is written down but every PR crosses it with approval, so it's not really a boundary. A constitution that is long and partly fictional trains agents to treat all of it as optional.

This skill is the gardener. It runs on a cadence (quarterly is typical) or when symptoms appear, reads the evidence of what the team actually does, and proposes a short amendment set. It never edits the constitution on its own — amendments go through whatever process §10 of the constitution defines.

## What it looks for

| Symptom | Evidence source | Typical amendment |
|---|---|---|
| **Glossary gaps** — specs define terms the constitution lacks | `specs/*/spec.md` §4 Domain Terms across all specs | Promote recurring terms to the constitution glossary |
| **Glossary conflicts** — the same term defined differently in two specs, or differently from the constitution | Same | Reconcile; pick one; note the change in the decision log |
| **Boundaries routinely crossed** — forbidden or restricted areas that PRs touch with approval every time | `spec-validate` reports (forbidden-changes sections), PR history, plan reviews | Either the boundary is wrong (relax it with the condition under which crossing is fine) or enforcement is missing (say so; don't keep a rule nobody keeps) |
| **Boundaries agents keep hitting by mistake** | `plan-review` and `spec-validate` findings repeating the same violation | The rule is unclear or buried; rewrite it plainly, move it up, add the example that keeps failing |
| **Rules nobody follows** — standards in §6 with no evidence in recent PRs | PR history, CI config, test layout | Remove, or convert to a tooling check (lint, CI) and delete the prose |
| **Repeated reinterpretations** — `spec-validate` keeps flagging the same kind of default or behavior as Reinterpreted across specs | Validation reports | Add a default to the constitution's NFR or UX defaults section so specs stop having to restate it and agents stop guessing |
| **Stale structure** — repo layout, components, or services in §4/§5 that no longer exist or have moved | Repo tree vs. constitution text | Update; delete references to things that are gone |
| **Bloat** — length over ~300 lines, duplicated rules, long explanations where a link would do, rules that only mattered for one incident | The document itself | Cut; link out; collapse duplicates. Record what was cut in the decision log so it's recoverable |
| **Missing non-negotiables** — a class of mistake that caused real harm and has no rule | Incidents, validation reports marked Not ready for forbidden changes | Add one short prohibition. Short. |

## Workflow

### 1. Establish the baseline

Read the constitution fully. Note its version, amendment date, length, section sizes, and its §10 amendment process (that's how proposals get adopted). If there's no §10, propose one as the first amendment — a constitution anyone can edit casually stops being one.

### 2. Gather evidence

Read, as available:
- All `specs/*/spec.md` — collect every §4 term with its definition and every §3 forbidden item.
- All `specs/*/validation/*.md` (`spec-validate` reports) — collect forbidden-change findings, repeated Reinterpreted patterns, operability gaps.
- All `specs/*/plan-review.md` — repeated findings.
- PR history for the period (code host connector or `git log`) — which constitution areas were touched, how often, with what review outcome.
- CI/lint config — which §6 standards are actually enforced by tooling.
- Incidents or postmortems if the team keeps them.

Count things. "Term 'session' defined 4 ways across 6 specs" is an amendment; "glossary feels thin" isn't.

### 3. Draft the amendment set

Use `assets/amendment-proposal-template.md`. Each amendment: the symptom with its counts, the evidence, the proposed text change (before/after), the expected effect, and the risk of making it. Aim for **5–10 amendments, not 30** — a long amendment list is itself bloat, and the §10 process won't absorb it. Prioritize by how many specs or PRs the symptom touched.

Include at least one **deletion** in any amendment set for a constitution over 200 lines. If you can't find anything to cut, look harder; the document has been accreting.

Include the **length delta**: what the constitution will be after the set is applied. The target is net shrinkage or neutral.

### 4. Check the amendments against each other

Reconciled glossary terms must be used consistently in the amendments themselves. A relaxed boundary must not contradict a non-negotiable. A new default must not duplicate an existing one. Read the proposed post-amendment constitution once as if you were an agent seeing it fresh.

### 5. Deliver and route

Write the proposal to `.specify/memory/amendments/<YYYY-MM-DD>-proposal.md` (or alongside the constitution per repo convention) and hand it to the user. Point at §10 for how it gets adopted. Offer to prepare the post-amendment constitution as a diff or a PR once the team has decided, and to update the decision log and version. Don't apply the amendments unasked.

### 6. Downstream effects

When amendments are adopted, note which existing specs now conflict (a term renamed, a boundary moved) and offer to list them for `product-spec` review. Agents read the constitution, so a change here is a change to every future plan; say that plainly.

## Setting up the cadence

If asked to make this recurring, propose a scheduled task: quarterly, or monthly while the team is new to spec-driven work, producing the proposal file and a one-line summary to the PM. Pair it with `product-progress`'s weekly brief so symptoms (repeated findings) are already visible before the quarterly pass.

## Bundled files

- `assets/amendment-proposal-template.md` — proposal format: summary, amendments with before/after text and evidence, length delta, downstream effects.
- `references/health-signals.md` — fuller definitions of each symptom, how to measure it from each source, and the thresholds that make it worth an amendment.
- `references/example-proposal.md` — a worked quarterly proposal for the example repo used across the sibling skills.
