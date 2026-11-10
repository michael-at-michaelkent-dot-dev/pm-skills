# Confidence Rubric

Confidence is a property of the evidence, not of how much the team believes the claim. The same sentence can be High in one PAD and Assumption in another. Rate each row with this rubric and say in the "Raises confidence" column what would move it up.

## Levels

**High** — Multiple independent source types agree (e.g. interviews *and* ticket volume *and* behavioral data), the count is a clear majority of a reasonably sized sample (≥ ~10 for interviews; hundreds for tickets), the claim is an observation, and the sample matches the target persona.

**Medium** — One strong source type, or two weak ones; a plurality rather than majority; or an interpretation that follows closely from a High observation. Also the ceiling for any claim resting on stated preference ("they said they'd use it") without behavioral evidence.

**Low** — Single source type with a small count (≤ 3 interviews, a handful of tickets, one thread); or persona mismatch (the people saying it aren't the target); or an interpretation two steps from the observation.

**Assumption** — The team believes it and the material doesn't address it, or addresses it with n=1. Always written as "Assumption — validate by <method>". Assumptions are allowed in a PAD; unlabelled assumptions are not.

## Modifiers

- **Persona fit.** A claim from 3 of 3 target-persona interviewees outranks 8 of 22 mixed. Note fit explicitly.
- **Prompted vs unprompted.** Unprompted mentions are worth roughly double. Record which.
- **Behavioral > stated.** Replay, analytics, and ticket volume are behavior. Interview statements about future behavior are the weakest evidence type and cap at Medium.
- **Consistency.** If sources disagree, confidence drops a level and the disagreement goes in Counter-evidence.
- **Recency and breadth.** One intense recent Slack thread is Low regardless of message count; breadth across accounts and time matters more than volume in one place.
- **Vendor-reported or secondhand.** A sales rep saying "customers want X" is one source of type "secondhand", Low until corroborated.

## Examples

| Claim | Evidence | Level | Why |
|---|---|---|---|
| Operators rebuild filters every session | 14/22 interviews unprompted + 310 tickets/mo + replay median 6 clicks | High | three source types agree, majority, behavioral |
| 20 % of sessions abandon before filtering | replay n=120, one window | Medium | behavioral but single source, modest n, sample skew |
| Operators would use named saved views | 9/22 stated, no behavior | Medium | stated preference caps here |
| Admins want standard team views | 2/22, both admins | Low | tiny count; though persona-relevant, needs admin interviews |
| Mobile access matters | 1 ticket | Assumption | n=1 |
| Operators alternate environments mid-session | not in material | Assumption | not addressed; flag analytics query |

## Writing the "Raises confidence" column

Name the specific action and roughly what it would take: "5 admin interviews", "analytics: filter changes per session, 30 days", "prototype test n≈8". This column is what `product-pad` turns into §1.8 open questions and what `product-prototype` picks up.
