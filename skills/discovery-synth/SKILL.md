---
name: discovery-synth
description: Synthesize raw discovery material — customer interview notes or transcripts, support tickets, Slack or community threads, NPS verbatims, sales call notes, analytics exports, competitor observations — into the sourced, confidence-rated evidence table and open-questions list a PAD needs (§1.4 and §1.8), with themes, counts, and verbatim anchors, without inventing claims the material doesn't support. Use this whenever a PM wants to synthesize, summarize, theme, code, or "make sense of" research or feedback; asks "what are customers telling us about X"; pastes or attaches interview notes, a ticket export, a CSV of feedback, or call summaries; asks what the evidence says for or against a problem; or is starting a PAD and needs the evidence section filled. Also for turning a single customer conversation into evidence rows. Trigger even when the user just drops a pile of notes and says "what's in here".
---

# Discovery Synthesis

A PAD lives or dies on §1.4 — the evidence table where every claim about users carries its source and a confidence level. Most PM time goes into producing that table from messy raw material, and most of the shortcuts (asserting from memory, counting the loudest voice as a trend) are what alignment reviewers catch. This skill does the synthesis honestly: it themes the material, counts what can be counted, anchors every claim to a source, rates confidence by the evidence's actual strength, and keeps assumptions labelled as assumptions.

It feeds `product-pad` Stage 1 and can refresh evidence for Stage 2 or Stage 3. It reads from wherever the material is — attached files, connected tools (Gmail, Slack, Jira, HubSpot, Drive, Apollo/Lusha conversations), or pasted text.

## Principles

- **Claims come from the material, not from the skill.** If the notes don't say it, the table doesn't say it. A theme the PM expected but the material doesn't support is reported as "not observed", which is itself evidence.
- **Count what can be counted; say what can't.** "14 of 22 interviews" beats "most". "Several tickets" is a smell — go count them. When the material isn't countable (one long call), say n=1 and rate accordingly.
- **Separate observation from interpretation.** "Operators described rebuilding filters each session" is observation. "Operators want saved views" is interpretation. Both can go in the table, labelled; interpretation gets at most Medium confidence until tested.
- **Verbatims anchor, they don't argue.** One short quote per theme, chosen for clarity not drama, with its source id. Never a wall of quotes.
- **Sources are traceable.** Every row says which interview, ticket id, thread, or query produced it, so a reviewer can go look. Anonymize people (I-07, T-2291), never customers' names in the PAD unless the team's convention allows it.
- **Confidence is a function of evidence, not conviction.** See `references/confidence-rubric.md`. The same claim backed by 3 interviews is Low; by 14 interviews plus ticket volume is High.

## Workflow

### 1. Scope the question

Ask (once) what problem or opportunity the synthesis is for if it isn't obvious. Synthesis without a question produces a theme list; synthesis for a PAD produces evidence for and against a specific framing. Record the question at the top of the output. If the user has a PAD already, read §1.1 and §1.8 and treat those as the questions.

### 2. Inventory the material

List every source with an id, type, date, and size: `I-01 interview, 2026-10-14, 45 min notes`; `T-export tickets tagged results-filters, Aug–Oct, 931 rows`; `S-ops-channel Slack thread, 2026-11-02, 38 msgs`. Note gaps: no interviews with admins; tickets only from EU accounts; analytics not available. Gaps are findings — they become §1.8 open questions.

For connected tools, pull what's needed and no more: ticket exports filtered by tag and window, Slack threads the user named, call transcripts the user pointed at. Don't trawl.

### 3. Code the material

Read everything once, tagging passages with short theme codes as they emerge (don't pre-decide the codes). Second pass: merge near-duplicate codes, split codes that hold two ideas, drop codes with a single mention unless the mention is decisive (a security issue, a churn reason). Keep a code → source-ids map; that's where the counts come from.

For large ticket or feedback exports, do the coding in code: load into a dataframe, tag with keyword rules plus a sampled manual pass to check them, report precision of the rules on the sample. Don't eyeball 900 rows.

### 4. Build the evidence table

Use `assets/evidence-template.md`. One row per claim. Each row: the claim as a sentence, observation or interpretation, the count ("14 of 22 interviews; 310 tickets/mo"), the source ids, one verbatim anchor (optional, short), confidence per the rubric, and what would raise confidence.

Order rows by relevance to the PAD question, then by confidence. Include the **counter-evidence** rows — material that cuts against the framing — in the same table, not a footnote. A reviewer who finds counter-evidence the PM didn't surface stops trusting the whole table.

### 5. Derive open questions

Everything the material couldn't answer, every gap from step 2, every interpretation stuck at Medium or Low, becomes a question with a suggested method (more interviews with whom, an analytics query, a prototype test via `product-prototype`). These go to PAD §1.8.

### 6. Write the brief

Use `assets/synthesis-brief-template.md`: question, sources, themes with counts, evidence table, counter-evidence, open questions, what's still unknown. Two pages maximum for a PAD-sized synthesis. No recommendations — that's the PAD's job. If asked for a recommendation, give it as a separate clearly-labelled paragraph after the evidence.

### 7. Hand off

Offer to paste the evidence rows into PAD §1.4 and questions into §1.8 (via `product-pad`), or to create the PAD if none exists. Save the brief to `pads/discovery/<YYYY-MM-DD>-<slug>.md` so the PAD can link it and Stage 3 can revisit what was believed at the start.

## Common failure modes to avoid

- **Theme inflation.** Twelve themes from eight interviews means the codes weren't merged. Aim for 4–7.
- **Frequency ≠ importance.** A theme mentioned by 3 of 22 can still be the one that matters if those 3 are the target persona and the issue is churn-adjacent. Say both the count and the persona fit.
- **Recency bias from Slack.** Last week's thread feels like a trend. Weight by breadth of sources, not loudness.
- **Laundering the PM's hypothesis.** If the PM said "I think it's X" at the start, X needs the same evidence bar as everything else. Report "X — not observed in material" plainly when that's the case.
- **Quotes doing the arguing.** If a row needs three quotes to feel supported, the count is doing less work than it should.

## Bundled files

- `assets/evidence-template.md` — the row format that pastes straight into PAD §1.4.
- `assets/synthesis-brief-template.md` — the two-page synthesis brief.
- `references/confidence-rubric.md` — how to rate High / Medium / Low / Assumption from evidence type, count, and consistency, with examples.
- `references/coding-guide.md` — practical thematic coding: first pass, merge/split rules, handling large exports in code, anonymization.
