# CLAUDE.md

The master router. Read this first, on every session. This repo is an
organisation of teams, and this file is the router, not a team: it decides which
team handles a request and how teams combine.

## Load on every session
1. This router.
2. `MEMORY.md` and `context/VOICE.md`: the shared, org-wide canon.

Then route, and load only the team(s) the request needs.

## Teams
| Folder | Team | Front door | Use for |
|---|---|---|---|
| `teams/executive/` | Executive | `executive-cos` (CoS) | running the business: operations, planning, comms, quality |
| `teams/engineering/` | Engineering | `engineering-architect` (Architect) | building software: frontend, backend, build quality |

Front doors and specialists are subagents in `.claude/agents/`, named
`<team>-<role>`.

## Routing: count the teams
Work out which teams the request touches, then:

- **One team** -> the main session *wears that team's front-door hat* (CoS for
  executive, Architect for engineering) and spawns that team's specialists
  directly. One level of delegation.
- **More than one team (cross-team)** -> the main session stays the router:
  1. Pick the **lead team**: the one whose output dominates the deliverable.
  2. Define the **handoff contract**: the exact artifact that crosses the
     boundary (for example, "engineering returns a technical-facts note").
  3. Call each front door **plan-only**: `executive-cos` and
     `engineering-architect` return their team's specialist briefs; they do not
     spawn anyone (subagents nest only one level).
  4. The router fans out to the required specialists itself, then synthesises,
     the lead team's contribution last.

## Roster
- Executive: `executive-cos` (front door), `executive-ea`, `executive-qa`.
- Engineering: `engineering-architect` (front door), `engineering-frontend`,
  `engineering-backend`, `engineering-qa`.

## The QA loop
Each team runs its own QA loop on non-trivial work: QA checks against the brief;
on defects the producing role revises and QA re-checks, up to 3 rounds, then
escalate with the gap. Only a PASS (or that escalation) reaches me.

## Precedence
The shared canon (`MEMORY.md`, `context/`) and each team's own rules are
non-negotiable. A team's front door governs that team's internals; this router
only governs which team handles a request and how teams combine.

## Adding a team
Add a `teams/<name>/` folder with its front-door and specialist role docs, add
their `<name>-<role>` subagents to `.claude/agents/`, and add a row to the Teams
table.

## Keep memory current
Update `MEMORY.md` (shared) or the relevant team's memory in place when you learn
something. Replace what is outdated.
