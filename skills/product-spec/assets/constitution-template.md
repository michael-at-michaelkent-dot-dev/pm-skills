# <Project / Repo> Constitution

The constitution is the stable context every feature spec and every coding agent reads before working in this repo. It changes rarely and deliberately. Feature specs reference it instead of repeating it. Keep it under ~300 lines; when it grows past that, agents start losing the thread.

**Version:** 1.0 · **Ratified:** YYYY-MM-DD · **Last amended:** YYYY-MM-DD

## 1. Purpose & Scope

What this product (or service, or repo) is for, in a paragraph. What it is explicitly *not* — the adjacent problems other systems own.

## 2. Users & Context

The personas this product serves and the environment they operate in. Enough that an agent can make a sane UX call without a PM in the room.

## 3. Domain Glossary

Every domain term with one canonical definition. Specs use these words exactly. Disagreement here is the root of most "the agent built the wrong thing" incidents.

| Term | Definition | Not to be confused with |
|---|---|---|
| | | |

## 4. Architectural Boundaries

The map agents must stay inside.

- **Components / services and their responsibilities:** 
- **What talks to what, and through which interface:** 
- **Layers agents may not cross** (e.g. UI must not call the database directly): 
- **Places that require a human decision before change** (auth, billing, data migrations, public API contracts): 

## 5. Repository Layout

Where things live and where new things go. Point at folders; don't list every file.

```
src/          
tests/        
specs/        feature specs (one folder per spec)
.specify/     spec-driven-development scaffolding
```

## 6. Engineering Standards

Only the rules that an agent would otherwise get wrong. Link to longer style guides instead of pasting them.

- **Languages & versions:** 
- **Testing:** what must have tests, where they live, what "done" means (e.g. every AC in a spec maps to at least one test).
- **Error handling & logging conventions:** 
- **Dependencies:** how new ones are approved.
- **Commits & PRs:** branch naming, PR must cite the spec ID and the ACs it satisfies.

## 7. Non-Functional Defaults

Baseline requirements every feature inherits unless its spec says otherwise.

- **Performance:** 
- **Security & privacy:** 
- **Accessibility:** 
- **Telemetry:** 
- **Compliance:** 

## 8. Non-Negotiables

Short list. Things no spec may override. Written as prohibitions.

- Never …
- Never …

## 9. Spec-Driven Workflow

How a change moves through this repo. Adjust to the team's tooling.

1. Problem framing (PAD / PRD) → reviewed and approved.
2. Feature spec written to `specs/<NNN>-<slug>/spec.md` using the spec template; PM owns intent and acceptance criteria.
3. Plan generated from spec + constitution; eng reviews the plan, not a hand-written design doc.
4. Tasks broken out; implementation by people or agents.
5. Validation: merged code checked against the spec's acceptance criteria; the spec is updated if behavior intentionally changed, so it stays the source of truth.
6. Post-launch recap against Section 2 metrics of the spec.

## 10. Amendment Process

Who can change this document and how. A constitution that anyone edits casually stops being one.

## Appendix — Decision Log

| Date | Decision | Why | Supersedes |
|---|---|---|---|
| | | | |
