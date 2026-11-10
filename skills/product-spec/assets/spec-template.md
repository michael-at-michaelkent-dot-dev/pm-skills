# Feature Spec: <Feature name>

| | |
|---|---|
| Spec ID | `<NNN>-<kebab-slug>` |
| Status | Draft / In review / Approved / Shipped |
| Owner (PM) | |
| Eng lead | |
| Design | |
| Source PAD / PRD | <link> |
| Constitution | `.specify/memory/constitution.md` |
| Last updated | YYYY-MM-DD |

## 1. Problem & Users

**Problem.** One paragraph: what is broken or missing, for whom, and what it costs them today. No solution language.

**Primary persona.** Who, in one or two lines, including the context they are in when they hit this problem.

**Secondary personas.** Anyone else affected, or "none".

## 2. Success Metrics

Each metric needs a baseline (or "no baseline — instrument first") and a target. These are what the agent and reviewers use to choose between valid implementations.

| Metric | Baseline | Target | How measured |
|---|---|---|---|
| | | | |

## 3. Scope

### In scope
- 

### Non-goals
Things a reasonable person might expect this feature to include and it does not. Be specific.
- 

### Forbidden changes
Code, behavior, data, or contracts the implementation must not touch, even if it seems helpful.
- 

## 4. Domain Terms

Terms used in this spec that are not already in the constitution glossary. Every term used below must be defined here or there.

| Term | Definition |
|---|---|
| | |

## 5. User Stories & Acceptance Criteria

Number stories `US-1`, `US-2`, …; criteria `AC-1.1`, `AC-1.2`, …. Every criterion is Given/When/Then and checkable by a test or a human in under a minute.

### US-1: <As a persona, I want X so that Y>

- **AC-1.1** Given … When … Then …
- **AC-1.2** Given … When … Then …

### US-2: …

## 6. Behavior Details

Only what the stories don't already pin down. Error states, empty states, permissions, concurrency, limits, ordering, defaults. If a section does not apply, write "N/A" rather than deleting it — the absence is itself information for the agent.

### Empty / zero states
### Validation & error handling
### Permissions & roles
### Concurrency, limits, timeouts
### Defaults

## 7. Data & Contracts

Interfaces this feature reads, writes, or changes. "None" is a valid answer. Shape, not schema — fields and semantics, not column types.

| Contract | Read / Write / New / Changed | Notes |
|---|---|---|
| | | |

## 8. Non-Functional Requirements

State each as a measurable requirement or defer explicitly to the constitution.

- **Performance:** 
- **Security & privacy:** 
- **Compliance / audit:** 
- **Accessibility:** 
- **Telemetry / observability:** what events or metrics must exist for Section 2 to be measurable.
- **Localization:** 

## 9. Rollout & Rollback

- **Feature flag:** name or "none", and default state.
- **Rollout stages:** 
- **Rollback trigger:** the observable condition that means "turn it off".
- **Rollback procedure:** what reverting looks like; data migrations that make rollback hard go here.

## 10. Dependencies & Risks

| Item | Type (dependency / risk) | Owner | Mitigation |
|---|---|---|---|
| | | | |

## 11. Open Questions

Anything unresolved. The agent must halt on these rather than choose.

- [NEEDS CLARIFICATION: …] — owner, date asked

## Appendix A — Suggested implementation notes (non-binding)

Optional. Ideas from the PM or source document about *how* to build it. The plan phase may ignore these.

## Appendix B — Changelog

| Date | Change | By |
|---|---|---|
| | | |
