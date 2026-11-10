# Constitution Health Signals

How to measure each symptom and when it's worth an amendment. Thresholds are starting points; a team can set its own in the constitution's §10.

## Glossary gaps

**Measure.** Collect every term from every `specs/*/spec.md` §4. Group by normalized name. Count how many specs define each.

**Threshold.** A term defined in ≥ 3 specs with compatible meanings → promote to the constitution. Defined in 2 → watch. Defined in 1 → leave in the spec.

**Amendment shape.** Add the term with the most-used definition; note which specs can now drop their §4 entry.

## Glossary conflicts

**Measure.** Same collection; flag terms whose definitions differ materially across specs or from the constitution.

**Threshold.** Any conflict involving a term the constitution already has is an amendment. Conflicts between specs only: amendment if the term is used in ≥ 2 specs; otherwise flag to `product-spec` for the spec owners to reconcile.

**Amendment shape.** One definition wins; the proposal names the losing specs as downstream effects.

## Boundaries routinely crossed with approval

**Measure.** From `spec-validate` reports' forbidden-changes sections and from PR review history: how many PRs touched each restricted area in the period, and how many were approved anyway.

**Threshold.** Crossed with approval ≥ 3 times in a quarter → the boundary as written is wrong. Either relax it with the condition ("…except by a migration reviewed by the data owner") or make it a real gate (CI check, CODEOWNERS). Keeping a rule that is routinely waived teaches agents the whole document is advisory.

## Boundaries hit by mistake

**Measure.** Repeated findings in `plan-review` and `spec-validate` of the same violation class across different specs.

**Threshold.** Same class ≥ 2 times across different specs → the rule is unclear or buried. Rewrite plainly, move it earlier, add the concrete example that keeps failing.

## Rules nobody follows

**Measure.** For each §6 standard: is it enforced by tooling (lint, CI, pre-commit)? If not, sample recent PRs for compliance.

**Threshold.** Not tooled and < 50 % compliance in the sample → remove it or tool it. Prose rules that aren't followed are bloat with a cost: they dilute the ones that matter.

## Repeated reinterpretations

**Measure.** From validation reports: categorize Reinterpreted findings (default values, sort orders, error behavior, copy, limits). Count per category across specs.

**Threshold.** Same category ≥ 3 times → the constitution needs a default for it (e.g. "Lists default to most-recently-used order unless the spec says otherwise", "On save failure, preserve user input and offer retry"). This is the most valuable amendment type: one line prevents a class of future drift.

## Stale structure

**Measure.** Compare §4 components/services and §5 layout against the repo tree and recent PRs. Anything named that doesn't exist, or exists elsewhere.

**Threshold.** Any stale reference is an amendment. Agents navigate by these sections.

## Bloat

**Measure.** Total lines; lines per section; duplicated rules (same requirement in two sections); rules traceable to a single incident with no recurrence; explanations that could be a link.

**Threshold.** > 300 lines total, or any section > 60 lines, or any rule duplicated → cut. Target net shrinkage each cycle until under 300.

**Amendment shape.** Deletions with the reason recorded in the decision log. Cutting is not losing: the decision log keeps the history, and a rule that recurs can come back shorter.

## Missing non-negotiables

**Measure.** Validation reports with verdict Not ready due to forbidden changes or permissions/data exposure; incidents or postmortems in the period.

**Threshold.** Real harm or near-miss with no rule covering it → one short prohibition. Resist adding a rule for every scare; non-negotiables lose force past ~8 entries.

## Reading the whole document as an agent

After drafting amendments, read the post-amendment constitution top to bottom once, asking: if I had only this and a spec, would I know what not to touch, what words mean, where things go, and what "done" means? Anything that doesn't serve one of those four questions is a candidate for the next deletion.
