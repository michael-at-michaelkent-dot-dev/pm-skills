# Product OS Skills

Nine skills for spec-driven, agent-assisted product development. Together they take a feature from idea to launch with the spec as the single source of truth and the PM owning intent rather than implementation.

```
discovery-synth ─► product-pad ─► product-prototype ─► product-pad (Stage 2)
                                                             │
                                                      product-spec
                                                             │
                                                   plan ─► plan-review ─► tasks ─► build
                                                             │
                                                      spec-validate ─► launch-pack ─► product-pad (Stage 3)

product-progress        reads specs/, pads/, PRs, tracker → "needs you today"
constitution-maintain   quarterly: reads everything above → amendment proposal
```

| Skill | Question it answers |
|---|---|
| `discovery-synth` | What does the evidence actually say? |
| `product-pad` | Should we build this, and which direction? |
| `product-prototype` | Can we test a direction without building it? |
| `product-spec` | Exactly what are we building? |
| `plan-review` | Is the agent's plan a plan for *our* spec? |
| `spec-validate` | Did this PR build what we specified? |
| `launch-pack` | What do we tell customers and frontline teams? |
| `product-progress` | Where is everything, and what needs me? |
| `constitution-maintain` | Is the constitution still lean, current and obeyed? |

All nine share one repo contract: `pads/`, `specs/<id>/`, `.specify/memory/constitution.md`. Each ships with a worked example of the same feature (PAD-2026-011 → spec 014), so the nine example files read as one case study.

## Install

### With the `skills` CLI (Claude Code, Cursor, Codex, and ~40 other agents)

```bash
# all nine, into this project's agent config
npx skills add michael-at-michaelkent-dot-dev/pm-skills

# one skill
npx skills add michael-at-michaelkent-dot-dev/pm-skills --skill product-spec

# target an agent explicitly, or install globally for every project
npx skills add michael-at-michaelkent-dot-dev/pm-skills -a claude-code
npx skills add michael-at-michaelkent-dot-dev/pm-skills -g

# later
npx skills check && npx skills update
```

### As a Claude Code plugin marketplace

```
/plugin marketplace add michael-at-michaelkent-dot-dev/pm-skills
/plugin install product-os@pm-skills
```

### Manually

Copy any `skills/<name>/` folder into `.claude/skills/` (project) or `~/.claude/skills/` (global).

### Claude Desktop / claude.ai

Zip a `skills/<name>/` folder and upload it under Settings → Capabilities → Skills, or use the `.skill` packages from Releases.

## Layout

```
skills/<name>/
├── SKILL.md          # frontmatter (name, description) + workflow
├── assets/           # templates the skill fills in
└── references/       # checklists, rubrics, worked examples
.claude-plugin/
└── marketplace.json  # plugin manifest for Claude Code
```

## Contributing a change

Edit the `SKILL.md` or the templates in place, bump the version in `.claude-plugin/marketplace.json`, and open a PR. Keep each `SKILL.md` under 500 lines; move detail to `references/`.
