# Release notes entry

## Format

```
### <Customer-facing feature name>   <version or date>

<One sentence: what you can now do and why it matters.>

- <Behavior 1, user's point of view>
- <Behavior 2>
- <Behavior 3>

<Optional one line on a limitation customers will hit, or where to learn more: [docs link].>
```

Three to eight lines total. Written for a customer skimming a list of changes; if they read only the heading and first sentence they should know whether it affects them.

## Good vs bad lines

| Bad | Why | Good |
|---|---|---|
| "Improved filtering experience" | says nothing | "Save a filter combination and apply it with one click" |
| "Given at least one filter is applied, the save action…" | AC text, not prose | "Save your current filters under a name you choose" |
| "Blazing fast saved views" | unsupported superlative | "Opens your default view before results load" |
| "Team sharing coming soon" | roadmap promise without a date | (omit, or move to enablement with the real date) |
| "Fixed a bug where…" | this is a feature entry; bugs go in their own section | — |

## Sources

Heading name from step 2 of the skill. First sentence from PAD §2.4 positioning and §1.1 problem. Bullets from spec §5 stories (one per story that shipped — check the validation report). Limitation line from spec §3 or §6. Docs link `[TBD]` until live.
