# Fidelity Guide

The right prototype is the cheapest one that answers the question. Fidelity is a cost with two parts: time to build, and the bias polish introduces in testers. Both argue for less.

## Match fidelity to the question

**Comprehension questions** ("Would operators understand what a saved filter is from this page?") want low fidelity: grey boxes, real labels and copy, no color, no interaction beyond page-to-page. Testers read words; anything else distracts.

**Task questions** ("Can an operator save and re-apply a filter without help?") want medium fidelity: real-looking controls that respond, 3–6 screens, one happy path, fake data that makes sense in context. Enough to feel the flow; not enough to argue about button radius.

**Preference questions between directions** ("Named saved views, or auto-remember last state?") want two medium-fidelity prototypes built from the same screens and data, differing only in the interaction model. Give half the testers A first. Never a single prototype with a toggle — whichever state is default wins.

**Fit questions** ("Does this belong in the existing Results page?") are the one case for styled fidelity, because the question is about context. Use the product's design tokens read-only if a design system is available. Still fake data, still a banner.

**Leadership questions** ("Will the room get it in 30 seconds?") want a storyboard or a 60-second scripted click path, not a full prototype. Reviewers in an alignment meeting react to narrative; detail invites debate about the wrong layer.

## Over-building — the common failure

Signs you've gone too far:
- A build step, a framework, or more than one file per prototype.
- Any real API call.
- Handling of error states the open question didn't ask about.
- Pixel-matching the brand.
- Anyone saying "we could just ship this."

The cost isn't only time. Polished prototypes get kinder feedback (testers assume it's decided), and they tempt the team to skip the spec. If a prototype grew past the question, cut it back before testing.

## Under-building — the rarer failure

Signs:
- Testers ask "what am I supposed to do here?" more than once per session.
- The flow stops before the moment the question is about.
- Fake data is so fake it confuses ("Lorem ipsum" in a field that should hold an environment name).

Fix by adding the one missing screen or the one realistic data set, not by raising fidelity overall.

## Fake data that works

- Names that look real but can't be mistaken for a customer: `Env-Alpha`, `Env-Bravo`, `Contoso-Prod`, `Run-0042`.
- Numbers in plausible ranges for the persona's world.
- Dates relative to "today" in the prototype, so it doesn't look stale next week.
- Never paste from a real tenant, ticket, or screenshot.

## Comparing directions fairly

- Same screens, same data, same test script, same session length.
- Counterbalance order across testers.
- Ask the closing question identically.
- Report completion and hesitation per direction, then preference — in that order, because stated preference is the least reliable of the three.

## Discarding

Prototypes are evidence-generating devices. Once the evidence is in the PAD, the device has done its job. Archive the folder with the PAD id (or delete it), and point anyone who asks for "the prototype" at the spec. If the team keeps wanting to open the prototype after Stage 2, the spec's §5 stories probably aren't vivid enough — fix the spec, not the prototype.
