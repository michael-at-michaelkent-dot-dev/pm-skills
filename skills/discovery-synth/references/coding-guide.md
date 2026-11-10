# Coding Guide

Practical thematic coding for PM discovery. The goal is a code → source-ids map you can count from, not an academic codebook.

## First pass — open coding

Read each source once, start to finish. Tag passages with short codes as they occur to you (`rebuild-filters`, `lost-on-nav`, `wants-saved-view`, `wants-team-view`, `slow-ui`). Don't pre-decide codes from the hypothesis; if the PM's hypothesis shows up, it earns a code like anything else. Mark each tag with source id and whether the mention was prompted (P) or unprompted (U).

Keep a running list; expect 15–30 codes after the first pass.

## Second pass — merge and split

- **Merge** codes that are the same idea in different words (`rebuild-filters` + `same-four-clicks`).
- **Split** codes that hold two ideas (`wants-saved-view` into `wants-personal-saved-view` and `wants-team-saved-view` if both appear).
- **Drop** codes with one mention unless the mention is decisive — a security or compliance issue, a churn reason, a hard blocker. Keep those as single-mention rows with Low confidence.
- Target 4–7 themes. More means you haven't merged; fewer means you've lost distinctions a reviewer will ask about.

## Counting

For each surviving code: number of distinct sources (not mentions — one interviewee saying it five times is 1), prompted vs unprompted split, persona breakdown. Counts go in the evidence row verbatim: "14 of 22 (11 U, 3 P); 12 operators, 2 admins".

## Large exports (tickets, NPS, feedback CSVs)

Don't read 900 rows by eye. Load into pandas:

1. Sample 50 rows; hand-code them with the codes from your interview pass plus anything new.
2. Write keyword/regex rules per code; run on the sample; compute precision and recall against your hand codes. Iterate until precision ≥ 0.8 for the codes you'll cite.
3. Run on the full export; report counts per code **and** the precision you measured ("310/mo tagged `re-entry`, rule precision 0.86 on n=50 sample").
4. Pull 2–3 verbatim anchors per code from the full set, chosen for clarity.

Put the script in `pads/discovery/scripts/` so Stage 3 can re-run it.

## Anonymization

Interviewees: `I-nn`. Tickets: the ticket id is fine (internal). Slack: `S-<channel>-<date>`, no usernames. Customer company names: omit from the PAD unless the team's convention allows; use "a large EU account" or a persona label instead. Never put names, emails, or account ids in the evidence table.

## Verbatim anchors

One per theme, ≤ 15 words, chosen because it states the theme plainly — not because it's the angriest. Attribute to source id. If no short clear quote exists, skip the anchor; the count carries the row.

## Writing observations vs interpretations

Observation: a sentence a skeptic would accept as a description of the material. "14 interviewees described rebuilding filters each session."

Interpretation: a sentence about what it means or what people would do. "Operators want saved views." Keep interpretations one step from their observation and label them; the PAD review will push on any that are two steps away.

## Counter-evidence

Actively look for it on the second pass: sources that contradict the main themes, personas who don't have the problem, tickets that blame something else. Put them in the table with the same rigor. A table with no counter-evidence rows reads as advocacy.
