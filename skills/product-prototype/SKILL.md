---
name: product-prototype
description: Build a disposable, clearly-labelled prototype — clickable HTML mock, wireframe, fake-data walkthrough, or scripted demo — of one or more candidate directions from a PAD, so the team can test with users and show something real in the alignment meeting, and then record the result as a row in the PAD's evidence table. Use this whenever a PM wants to prototype, mock up, wireframe, sketch, "make this tangible", "show what option B would feel like", run a concept test, build a throwaway demo, or compare directions side by side before committing. Also trigger when a PAD has reached Stage 1 §1.7 with two or more directions and the open questions call for a prototype test. Never for production UI or anything that will be kept — that's the spec and the plan.
---

# Product Prototype

In AI-native development, prototypes are the thing most likely to go wrong in a quiet way. Agents make building so cheap that a "quick mock" turns into the real feature, Stage 1 collapses into Stage 2, and the alignment meeting ends up reviewing code instead of a problem. This skill keeps prototypes in their lane: they exist to produce **evidence** for a PAD — a user reaction, a measured task time, a "no, that's not it" in the meeting — and nothing else. The output of a prototype is a row in §1.4 Evidence, not a component in `src/`.

It sits between `product-pad` Stage 1 (directions) and Stage 2 (chosen direction). It can also serve Stage 2 §2.2 to make the chosen direction concrete for design review, but still as a throwaway.

## Non-negotiables

- **Never writes to the product's source tree.** Output goes to `pads/prototypes/<pad-id>/<direction>/` or a scratch folder, never to `src/`, `app/`, or wherever the constitution says product code lives. If asked to "just put it in the real codebase", decline and explain: the moment it's in `src/` it becomes an implementation decision that bypassed the spec.
- **Labelled disposable, visibly.** Every prototype carries a banner or header: "PROTOTYPE — PAD-2026-011 direction A — not production — fake data". People forward screenshots; the label travels with them.
- **Fake data, obviously fake.** Names like "Env-Alpha", "Test-Run-0042", dates in the future or clearly placeholder. Real customer data never goes in a prototype.
- **No backend, no auth, no persistence** beyond in-memory or `localStorage` for the duration of a click-through. If a direction can't be evaluated without real data flow, that's a finding — write it down and prototype the part that can be clicked.
- **One direction per prototype.** Comparing directions means two prototypes side by side, not one with toggles — toggles bias testers toward whichever is default.

## Workflow

### 1. Anchor to the PAD

Read the PAD. Identify which direction(s) from §1.7 are being prototyped and which open question from §1.8 the prototype is meant to answer. If there's no PAD, or no open question that a prototype would resolve, ask what question the prototype is for — a prototype without a question produces opinions, not evidence. Write the question at the top of the prototype's README.

### 2. Choose fidelity by question

| Question type | Fidelity | Form |
|---|---|---|
| "Do users understand what this is?" | Low | Static wireframe, grey boxes, real copy |
| "Can users complete the task?" | Medium | Clickable HTML with 3–6 screens, fake data, happy path only |
| "Which of A/B feels right?" | Medium, two prototypes | Same screens, same fake data, different interaction model |
| "Does this fit the existing product?" | Medium, styled | Clickable mock using the product's visual tokens if a design system is available (read-only use, never modified) |
| "Will leadership get it in 30 seconds?" | Low–medium | Three-frame storyboard or a 60-second scripted click path |

Default to the lowest fidelity that answers the question. Higher fidelity costs more to build and, worse, makes testers react to polish instead of concept.

### 3. Build

- Single self-contained HTML file per prototype (inline CSS/JS) in `pads/prototypes/<pad-id>/<direction-letter>-<slug>/index.html`, plus a `README.md` with: the PAD id, direction, question being tested, test script, and the "disposable" statement.
- Happy path first. Add one unhappy path only if the open question is about it.
- Use the `frontend-design` skill if available for visual quality — but stop at "looks like a credible product", not "pixel-matched to brand". Credible is enough to get honest reactions.
- Include a visible step counter or breadcrumb so testers and reviewers know where they are in the flow.
- Build a short **test script** into the README: 3–5 tasks a tester attempts, what to observe for each, and the one question to ask afterwards. Scripts make results comparable across testers.

### 4. Run or hand off the test

If the user is testing it themselves, give them the script and a results table to fill (`assets/test-log-template.md`). If testing happens elsewhere, hand over the folder. Don't claim test results you didn't observe.

### 5. Record evidence

When results come back, write them as PAD evidence:

- A row in §1.4: claim, source ("prototype test, direction A, n=8, 2026-11-14"), confidence.
- If the result resolves an §1.8 open question, note the answer there.
- If the prototype revealed a new question, add it to §1.8.

Then **recommend deleting or archiving the prototype** once the PAD advances past Stage 2. Say so explicitly in the handoff. A prototype that lingers becomes a reference implementation by accident; the spec is the reference.

### 6. Hand off

Tell the user what the evidence means for the directions table, and whether the PAD looks ready for its Stage 1 (or Stage 2) review. Point at `product-pad` for recording it.

## When someone wants to "just ship the prototype"

It happens; it's a compliment to the prototype. The answer is: the prototype resolved the question, now `product-spec` writes the spec, and the agent builds from the spec — which will take about as long as polishing the prototype would and will have acceptance criteria, tests, telemetry, and a rollback. Offer to start the spec with the prototype's flows as input to §5 user stories. Don't move the files.

## Bundled files

- `assets/prototype-readme-template.md` — the README every prototype folder carries: PAD link, question, direction, test script, disposable statement.
- `assets/test-log-template.md` — per-tester results table that rolls up into a PAD evidence row.
- `references/fidelity-guide.md` — longer guidance on matching fidelity to question, with examples of over- and under-building.
