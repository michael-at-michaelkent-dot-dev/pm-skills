# Consistency Checklist

Run across all four pieces together before delivering. Each inconsistency is a future support ticket or a frontline team contradicting the docs.

## Naming
1. The customer-facing feature name is spelled and capitalized identically in all four pieces and matches the spec's chosen name.
2. Domain terms match the constitution glossary's customer-facing form (some internal terms have a different public name — check).

## Scope
3. Every behavior in the announcement appears in the help article.
4. Every step in the help article traces to a spec AC that the validation report marks Satisfied (or Implemented-untested, if the team ships those).
5. No piece describes an AC the validation report marked Missing or deferred.
6. Non-goals (spec §3) and "later" items (PAD §2.5) appear in enablement, and in the help FAQ where a customer would hit them. None of them appears as a feature anywhere.

## Facts
7. Date / version is the same everywhere, or `[TBD]` everywhere.
8. Pricing / packaging statement is the same everywhere.
9. Limits (counts, sizes) match spec §6 exactly.
10. Any number in any piece has a source in the spec (§2, §6, §8) or the PAD (§1.4, §1.5). Delete numbers without one.

## Tone
11. No superlatives without a sourced number. No "coming soon" without a date from the PAD.
12. No internal identifiers (PAD ids, spec ids, AC numbers, flag names) in release notes, help article, or announcement. They're fine in enablement.
13. Second person, plain language, house voice if a style guide exists.

## Placeholders
14. Every `[TBD: …]` has an owner named in the handoff note.
15. Every `[Screenshot: …]` names the state and the AC that defines it.

## Final
16. Read the enablement FAQ as a support agent: can you answer each question from the help article alone? If not, one of them is incomplete.
