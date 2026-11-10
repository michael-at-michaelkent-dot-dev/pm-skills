---
name: launch-pack
description: Generate the launch collateral for a shipped or shipping feature — release notes entry, help-center article, internal enablement one-pager for sales/support/CS, and a short announcement — directly from the feature spec (acceptance criteria, behavior details, rollout) and the PAD (problem, audience, positioning, GTM plan), so everything is accurate by construction and consistent with each other. Use this whenever a PM asks for release notes, a changelog entry, what's-new copy, a help or docs article, a support FAQ, a sales or CS enablement brief, a launch announcement or customer email, "launch comms", or "the launch pack"; when a spec reaches Shipped or a spec-validate report says Ready; or when someone asks "what do we tell customers about this". Trigger even when they only name one piece — offer the rest.
---

# Launch Pack

Launch collateral written from memory drifts from what shipped. Collateral written from the spec can't: the acceptance criteria *are* the behavior, the PAD *is* the positioning, and the validation report *is* the list of what actually made it in. This skill generates the four standard pieces from those sources, keeps them consistent with each other, and flags anything the sources don't settle (pricing, dates, screenshots) rather than inventing it.

Chain position: after `spec-validate` says Ready and before (or alongside) the `product-pad` Stage 3 recap.

## The four pieces

| Piece | Audience | Source of truth | Length |
|---|---|---|---|
| **Release notes entry** | Existing customers skimming what changed | Spec §5 stories, §6 behavior, §3 non-goals; validation report for what actually shipped | 3–8 lines |
| **Help article** | A user trying to do the thing | Spec §5 ACs and §6 details, step by step; §6 unhappy paths become the FAQ | 1–2 screens |
| **Enablement one-pager** | Sales, CS, support | PAD §1.1 problem, §1.3 audience, §2.4 positioning; spec §3 scope and non-goals; known limitations | 1 page |
| **Announcement** | Customers (email / in-app / blog) | PAD §2.4 positioning line, §1.1 problem in the customer's words, one key flow | 120–200 words |

A piece the user doesn't need can be skipped; a piece they do need shouldn't be written without the other three being at least offered, because a help article that disagrees with the release note is worse than no help article.

## Workflow

### 1. Confirm what shipped

Read the spec. Read the latest `spec-validate` report if one exists — it's the only reliable record of what actually landed versus what was specified. If an AC was deferred, the collateral must not describe it. If a behavior was reinterpreted and the spec updated, use the spec as now written. If there's no validation report and the spec isn't marked Shipped, say so and ask whether to write for the spec as written (pre-launch draft) or wait.

Read the PAD for positioning (§2.4), problem and audience (§1.1, §1.3), and scope-outs with their "later" notes (§2.5) — those become the "not yet" lines in enablement.

### 2. Collect the facts the sources don't have

Make one list and ask once: release date or version, pricing/packaging impact (confirm §2.4's "none"), flag rollout stages and who sees it when, where screenshots will come from, docs URL, support contact, any legal/compliance review needed, naming (is "saved filters" the final customer-facing name?). Mark anything unanswered as `[TBD: …]` in the drafts rather than guessing — a wrong date in release notes is a support ticket.

### 3. Draft from the templates

Use `assets/` templates in order: enablement first (it forces the positioning and limitations to be explicit), then release notes, help article, announcement. The enablement one-pager becomes the reference the other three stay consistent with.

Conventions:
- **Customer-facing name** is used identically across all four pieces. Pick it once in step 2.
- **Describe behavior from the user's point of view**, in the spec's terms translated to plain language. Never paste AC text; ACs are contracts, not prose.
- **Limitations are features of honesty.** Non-goals from spec §3 and "later" items from PAD §2.5 appear in enablement and, where a customer would hit them, in the help article FAQ. Don't bury them.
- **No superlatives, no roadmap promises.** "Faster" needs a number from the spec's §2 or §8; "coming soon" needs a date from §2.7 or stays out.
- **Match house voice** if a style guide or `my-writing-style` skill is available; otherwise plain, direct, second person.
- **Screenshots**: mark placements `[Screenshot: <state>]` with the spec AC that defines the state, so whoever captures them knows exactly what to show.

### 4. Cross-check

Read the four pieces together once. Same name, same scope, same limitations, same date. Any claim in the announcement must be findable in the help article; any step in the help article must trace to an AC. Fix inconsistencies before showing the user. Use `references/consistency-checklist.md`.

### 5. Deliver

Write the pieces to `specs/<id>/launch/` (`release-notes.md`, `help-article.md`, `enablement.md`, `announcement.md`) so they version with the spec, and hand them to the user. If a docs connector (Confluence, Notion, HubSpot, Google Docs) is connected and the user wants them there, publish as drafts and say where. Don't send emails or post announcements — hand the text over and let the humans ship it.

### 6. Hand off

Note what's `[TBD]`, who owns it, and that the Stage 3 recap (`product-pad`) is next once the success-metric window has passed. Offer to add the launch date to the PAD's §3.1.

## Bundled files

- `assets/enablement-template.md` — one-pager: problem, who it's for, what it does, how to position, limitations, FAQ for frontline teams, where to send questions.
- `assets/release-notes-template.md` — entry format with examples of good and bad lines.
- `assets/help-article-template.md` — task-oriented article skeleton: before you start, steps, what you'll see, FAQ from unhappy paths.
- `assets/announcement-template.md` — 120–200 word customer announcement, with email/in-app/blog variants.
- `references/consistency-checklist.md` — the cross-piece check from step 4.
