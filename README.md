# ai-teams-base

> Workshop 3: from a team to an organisation.

The frontier. Multiple teams coordinated as an organisation of teams, with full
governance and scheduled playbooks running at scale. Parallel work across teams,
every rule and permission in git, the repo as the company.

## What is in here
- `CLAUDE.md`: the master router. Decides which team handles a request and how
  teams combine.
- `MEMORY.md`: the shared, org-wide memory.
- `context/`: shared canon loaded on demand via `context/INDEX.md`. `VOICE.md`
  (how to write) and `PROFILE.md` (who you are: what you do, where you live),
  filled in by the first-run setup.
- `teams/<team>/MEMORY.md`: what each team has learned.
- `teams/<team>/<role>/`: one folder per role, each with `ROLE.md` (what it does)
  and `MEMORY.md` (what it has learned). Executive: `cos` (front door), `ea`,
  `qa`. Engineering: `architect` (front door), `frontend`, `backend`, `qa`.
- `.claude/agents/`: the subagents, named `<team>-<role>` (the front doors and
  specialists the router and teams delegate to).

## How to start
1. Fork or clone this repo.
2. Open it in Claude Code.
3. Say **"Hi"** to kick off. On the first run that triggers a quick setup (the
   welcome wizard). After that the router counts the teams a request touches:
   one team, the session wears that team's front-door hat and spawns its
   specialists; more than one, it coordinates the front doors and fans out
   across teams.

## Concepts in this repo
- **Master router** (`CLAUDE.md`) — the entry point is a router, not a team; it
  counts the teams a request touches and routes accordingly.
- **Multiple teams** (`teams/`) — an executive team and an engineering team,
  each with its own front door, roles, and memory.
- **Team front doors** — the Chief of Staff fronts the executive team; the
  Architect fronts engineering. Each breaks work down for its specialists.
- **Cross-team handoff** — for work spanning teams, the router calls the front
  doors **plan-only** (they return briefs, do not spawn), picks a lead team and a
  handoff contract, then fans out and synthesises.
- **Team-prefixed subagents** (`.claude/agents/<team>-<role>.md`) — every role is
  a named subagent with its own tools allow-list and model.
- **Three-layer memory** — global (`MEMORY.md`), team (`teams/<team>/MEMORY.md`),
  and role (`teams/<team>/<role>/MEMORY.md`); the narrowest layer wins.
- **The QA loop (the Verifier)** — each team runs its own, up to 3 rounds, before
  work ships.
- **Governance** — `context/GOLDEN-RULES.md` (the constitution, loaded first),
  `PERMISSIONS.md` (the action-tier grants matrix across both teams), and
  `context/PRINCIPLES.md` (how you like work done). A capability is real only when
  granted in both `PERMISSIONS.md` and the role's allow-list (the two-place
  model), with a `.claude/settings.json` floor denying what should never happen.
- **Context loaded on demand** (`context/INDEX.md`) and **git as the store**.

## Claude Code files (and Codex equivalents)
Most of this repo is plain Markdown that copies to any agent runner unchanged:
`MEMORY.md`, `context/`, the `teams/<team>/<role>/` docs, and `playbooks/`
(playbooks are just SOPs written down, not the Claude "skills" feature). Only a
few things are genuinely Claude Code-specific:

| This repo (Claude Code) | Codex equivalent |
|---|---|
| `CLAUDE.md` (master router / auto-loaded startup) | `AGENTS.md` |
| `.claude/agents/*.md` (named subagents) | `.codex/agents/*.toml` (custom agents; not auto-delegated, you delegate explicitly) |
| the `tools:` allow-list in a subagent | `approval_policy` + `sandbox_mode` in `config.toml` (an approval/sandbox model, not a literal tool allow-list) |

## The progression
[`ai-agent-base`](https://github.com/gbergeret/ai-agent-base) (one agent) ->
[`ai-team-base`](https://github.com/gbergeret/ai-team-base) (a team with a
router) -> `ai-teams-base` (an organisation of teams). This is step 3.
