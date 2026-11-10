# PAD Review Checklist

Run for the current stage before presenting a draft, and in review mode on PADs you didn't write. Each item carries its *why*; the goal is the judgment a good alignment-meeting reviewer brings, not box-ticking. Report as pass / fixed / open.

## Stage 1 — Problem framing

### Gates — the meeting will send it back without these
1. **The problem is stated as a problem, not a feature.** "Users need X" is a solution wearing a disguise. Test: could a stranger propose a different solution from this statement alone?
2. **Audience is a named persona or segment**, with who is excluded. "Users" means the team hasn't chosen.
3. **Business "why" names a pillar, OKR, or segment.** Without it leadership can't rank this against other PADs.
4. **At least one success metric has a baseline or a measurement plan.** A metric with no baseline is a hope.
5. **Two or more genuinely different directions.** Variants of one idea don't count. Single-direction PADs get approved as specs by accident.

### Quality
6. **Every claim in Evidence has a source and a confidence.** Reviewers trust a PAD that labels its assumptions; they distrust one that smooths them over.
7. **"Do nothing" cost is stated** in 1.2. If doing nothing is cheap, that's a finding.
8. **No UI, flow, or architecture language in 1.1–1.6.** Move it to 1.7 as one option or cut it.
9. **Competitive section describes how users cope today**, not just a list of competitors.
10. **Open questions have a discovery method** (interviews, analytics, prototype test), not just a question mark.
11. **Fits on two pages.** Longer means the team is substituting detail for decisions.

## Stage 2 — Solution framing

### Gates
12. **Decision Log shows a Stage 1 outcome of Proceed or Approved with corrections.** Advancing an unreviewed framing defeats the point of the PAD.
13. **Course corrections from Stage 1 are visibly absorbed** in 2.1.
14. **Chosen direction is one from 1.7**, with why the others lost. A new direction appearing here means Stage 1 was incomplete.
15. **Design, eng, and GTM owners are named.** Solution framing is a team deliverable; a PM-only Stage 2 is a wish list.
16. **Risks & dependencies is non-empty.** Every real project has some; an empty table means nobody looked.

### Quality
17. **No acceptance criteria, Given/When/Then, edge cases, or data contracts.** Those are the feature spec's job (`product-spec`). Their presence here means the PAD is trying to be two documents.
18. **Engineering approach is at the risk-judgment level**, not a design doc.
19. **Scope has explicit outs with a "when".**
20. **Telemetry needed for metrics is named** in 2.8, so measurement isn't an afterthought.
21. **Milestones include the recap date.** Teams that don't schedule the recap don't do it.

## Stage 3 — Post-launch recap

### Gates
22. **Every Stage 1 metric has an actual value and a verdict.** Dropping a metric that missed is the most common recap failure.
23. **What shipped is compared to 2.5**, with cuts and additions named.

### Quality
24. **Learnings are written for the next team**, not as justification. Test: does it say what we'd do differently?
25. **"What's next" is a decision**: follow-on PAD, iterate, or close.
26. **Fits on one page.**

## Reporting format

```
Review: PAD-2026-011 (Stage 1)
Pass: 1, 2, 3, 6, 7, 9, 11
Fixed: 4 (added baseline from ticket volume), 8 (moved dashboard language to direction B)
Open: 5 — only one real direction; asked PM for alternatives. 10 — two questions lack a method.
```
