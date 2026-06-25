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

## The progression
[`ai-agent-base`](https://github.com/gbergeret/ai-agent-base) (one agent) ->
[`ai-team-base`](https://github.com/gbergeret/ai-team-base) (a team with a
router) -> `ai-teams-base` (an organisation of teams). This is step 3.
