---
name: product-pad
description: Write, advance, and review a Product Alignment Document (PAD) — the Miro-style three-stage artifact (problem framing → solution framing → post-launch recap) that gates what gets built before any spec or code exists. Use this whenever a PM wants to frame an opportunity or problem, pitch a feature idea, prepare for a product alignment or product review meeting, write a one-pager / problem statement / opportunity brief / discovery doc, decide whether something is worth building, record a leadership decision on a proposal, or write a post-launch recap or retrospective on a shipped feature. Also trigger on "is this worth doing", "help me frame this", "we need to align on", "the exec review is Thursday", or any pasted idea, customer complaint, or support theme the PM wants turned into a proposal. It hands off approved problem framings to the `product-spec` skill.
---

# Product Alignment Document (PAD)

The PAD is where a product team records the result of discovery and gets challenged on it before investing in a solution. In AI-native development it matters *more*, not less: agents make building cheap, so the expensive failure is no longer a bad implementation — it's a well-built feature nobody needed. The PAD's alignment review is the gate that prevents that. No spec gets written until a problem framing has a recorded "proceed."

This skill produces and advances a single document through three stages, and prepares the team for the alignment meeting at each one. It is the upstream half of a pair: `product-pad` decides *whether and what*; `product-spec` pins down *exactly what* for agents and engineers.

## The three stages

| Stage | Question it answers | Review outcome needed to advance |
|---|---|---|
| **1. Problem framing** | Is this opportunity real, important, and ours to solve? Which directions are plausible? | Proceed → solution discovery begins |
| **2. Solution framing** | Which direction are we pursuing, what will it take, what could go wrong? | Proceed → `product-spec` writes the feature spec |
| **3. Post-launch recap** | Did we achieve the outcome? What did we learn? | Closed; learnings shared |

Stages are additive: one document, sections filled in over time, never rewritten from scratch. Earlier sections stay as written so the recap can be honest about what was predicted.

## Workflow

### 1. Classify the request

- **New PAD** — the user has an idea, a problem, a customer theme, or a leadership ask. Start Stage 1.
- **Advance a PAD** — a problem framing has a recorded outcome and they want solution framing; or a feature shipped and they want the recap. Read the existing PAD first and fill only the next stage.
- **Review a PAD** — they want the challenge the alignment meeting will give them, in advance. Skip to step 5 in review mode.
- **Prep for the meeting** — they need the 2–3 slide summary and the likely questions. Step 6.

### 2. Gather inputs (ask once, in one message)

Read what they supplied: customer notes, support tickets, analytics, competitive observations, strategy docs, OKRs. Then check the inputs for the current stage. Gates are the ones a reviewer will reject the PAD for lacking — stop and ask rather than guess. Everything else can be marked "unknown — to discover" and left visible.

**Stage 1 gates**
- A real problem or opportunity statement, with evidence: who has it, how often, what it costs them. Not a feature idea restated as a problem ("users need a dashboard").
- Why it matters to the business: which strategy pillar, OKR, or segment it serves.
- Target audience: a named persona or segment, not "users".
- At least one success metric the team would accept as proof.
- Two or more plausible directions. If the user arrives with one solution, make them name alternatives — a PAD with a single direction is a spec in disguise and the meeting will send it back.

**Stage 2 gates**
- A recorded Stage 1 outcome of "proceed" or "approved with course corrections" (and the corrections). No outcome → ask whether the review happened; don't advance an unreviewed framing.
- The chosen direction and why the others lost.
- Named owners for design, engineering, and go-to-market inputs.
- Known risks and dependencies — an empty list is a red flag, not a clean bill.

**Stage 3 gates**
- The Stage 1 success metrics and their actual values post-launch.
- Launch date and what actually shipped versus what Stage 2 described.

### 3. Draft the stage

Use `assets/pad-template.md` exactly — same section order and headings, so reviewers can navigate every team's PAD the same way. Conventions:

- **Stage 1 stays solution-free.** The whole point of problem framing is to keep the team open to directions. If a sentence describes UI, flow, or architecture, move it to "Potential directions" as one option among several, or cut it. Write the problem so that a stranger could propose a different solution to it.
- **Evidence over assertion.** Every claim about users or market carries its source: interview count, ticket volume, analytics query, competitor name. Where there is no evidence, say "assumption — validate by <method>" rather than smoothing it over. Reviewers trust a PAD that knows what it doesn't know.
- **Metrics have a baseline or a plan to get one.** "Increase adoption" is not a metric. "Share of accounts running ≥1 scheduled test weekly, currently 18 %, target 35 %" is.
- **Directions are genuinely different**, not variants of one idea. Include "do nothing" or "partner/buy" when honest. One or two lines each on what it would take and what it bets on.
- **Stage 2 is still not the spec.** It names the direction, the key flows (link to design), the engineering approach at the level of "what it depends on and what it changes", the GTM plan, and risks. Acceptance criteria, edge cases, and contracts belong in the feature spec that `product-spec` writes next. If you find yourself writing Given/When/Then here, stop and point at the handoff.
- **Stage 3 is honest or it's useless.** Report the metric values as measured, including misses. The learnings section is for what the team would do differently, not a defense of what they did.
- **Length:** Stage 1 fits on two pages. Stage 2 adds one to two more. The recap is a page. A PAD longer than that is hiding a lack of decisions behind detail.

### 4. Place it

If there is a repo or docs folder in the session, write to `pads/PAD-<YYYY>-<NNN>-<kebab-slug>.md` (or the team's existing convention — follow it). Otherwise hand the file back and say where it should live. The PAD and its eventual spec should reference each other by ID so the recap can be traced back to the framing.

### 5. Self-review against the checklist

Run `references/review-checklist.md` for the current stage before showing the draft. Fix what you can; mark the rest as open. Report in one short block: pass / fixed / open. In **review mode** (someone else's PAD), return instead: the questions a sharp reviewer would ask, grouped by section; claims with no evidence; places where a solution has leaked into problem framing; and the metric that would be hardest to defend. Offer to apply fixes; the PM owns the judgment.

### 6. Meeting prep

When the PAD is ready for review, offer the two artifacts the alignment meeting needs:

1. **A three-slide summary**: (a) problem, audience, why now, (b) evidence and success metrics, (c) directions considered — or for Stage 2, chosen direction, plan, risks. Under 30 minutes including Q&A is the norm, so the slides carry the argument and the PAD carries the detail. If a Slides artifact type or skill is available, make them there; otherwise give slide-by-slide text.
2. **Likely challenges** — five to eight questions a leadership reviewer would ask, with the one-line answer or the honest "we don't know yet".

After the meeting, record the outcome in the PAD's Decision Log using the four standard outcomes:

- **Proceed**
- **Approved with course corrections** (list them)
- **Directionally OK — follow up offline before proceeding** (name with whom, about what)
- **Not approved — additional work required** (what work)

### 7. Hand off

After a Stage 2 "proceed": tell the user the next step is the feature spec, and that the `product-spec` skill takes this PAD as its primary input (it will mine the problem, audience, metrics, chosen direction, and risks, and will refuse to proceed without a recorded outcome — so make sure the Decision Log is filled). Offer to start it.

After a Stage 3 recap: suggest the quarterly recap session and offer to extract the learnings into a two-line entry for the team's learnings log.

## Bundled files

- `assets/pad-template.md` — the three-stage document skeleton with Decision Log. Copy it; don't reinvent sections.
- `references/review-checklist.md` — per-stage checklist used in step 5 and in review mode.
- `references/example-pad.md` — a worked Stage 1 + Stage 2 example for calibrating tone, evidence density, and the line between PAD and spec. Read it the first time you use this skill in a session.
