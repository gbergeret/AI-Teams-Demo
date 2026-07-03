# CLAUDE.md

The master router. Read this first, on every session. This repo is an
organisation of teams, and this file is the router, not a team: it decides which
team handles a request and how teams combine.

## Load on every session
1. `context/GOLDEN-RULES.md`: the constitution. Read it first; it overrides
   everything here.
2. `PERMISSIONS.md`: who may do what, the grants matrix. Binding on every action.
3. This router.
4. `MEMORY.md`: the shared, org-wide memory.
5. `context/INDEX.md`: the map of shared context. Load context files on demand
   (e.g. `context/VOICE.md`, `context/PROFILE.md`), not everything every time.

Then route, and load only the team(s) the request needs, including that team's
`MEMORY.md` and the `MEMORY.md` of any role you act as.

## First run: the welcome wizard
On the first session, if `playbooks/000-welcome-wizard.md` exists and the name in
`context/PROFILE.md` is not set yet, run that playbook before anything else. It
onboards you, then deletes itself, so this happens only once.

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
- Executive: `executive-cos` (front door), `executive-ea`, `executive-qa`,
  `executive-ciso` (org security gate).
- Engineering: `engineering-architect` (front door), `engineering-frontend`,
  `engineering-backend`, `engineering-security`, `engineering-qa`.

## Connectors
Real tools are added through Claude Code (its connector directory), then granted
per role in that role's `.claude/agents/*.md` allow-list. Read-only first: add
the Google Calendar connector and give the executive EA only its read tools
(`mcp__Google_Calendar__list_events` and friends). Grant a write later,
deliberately, in both the allow-list and your permissions policy.

## The QA loop
Each team runs its own QA loop on non-trivial work: QA checks against the brief;
on defects the producing role revises and QA re-checks, up to 3 rounds, then
escalate with the gap. Only a PASS (or that escalation) reaches me.

## Security gate
Security runs **after** QA, never before — it reviews only QA-validated work, and
only when a change is security-relevant (touches permissions, connectors, auth,
data handling, or config). If a deliverable is not security-relevant, this stage
is skipped entirely. Two scopes, kept separate so engineering stays
self-sufficient:

- **Engineering security stays in engineering.** Once a build passes the
  **engineering QA loop**, if it is security-relevant the **Security Engineer**
  (`engineering-security`) does the hands-on security pass — hardening, fixes,
  remediation — on that QA-validated work. Engineering ships its own security
  work without leaving the team; no executive sign-off needed.
- **The CISO reviews org-wide *policy*, by change type — not by team.** When a
  QA-validated change touches the org's security posture (`PERMISSIONS.md`,
  connector grants, `.claude/settings.json`, the `.claude/agents` allow-lists),
  the router runs the **CISO** (`executive-ciso`) for a read-only sign-off. A
  deliberate, occasional gate on policy, not a routine dependency on every task.

The order is always QA first, then security. When security flags a fix, it goes
back through QA before security re-checks. The CISO advises and never edits; the
Security Engineer builds and remediates.

## Precedence
The shared canon (`MEMORY.md`, `context/`) and each team's own rules are
non-negotiable. A team's front door governs that team's internals; this router
only governs which team handles a request and how teams combine.

## Adding a team
Add a `teams/<name>/` folder with its front-door and specialist role docs, add
their `<name>-<role>` subagents to `.claude/agents/`, and add a row to the Teams
table.

## Effort
Each subagent sets its reasoning effort in its `.claude/agents/<team>-<role>.md`
frontmatter (`effort:` — one of `low`, `medium`, `high`, `xhigh`, `max`). Match
it to the role: heavier for the front doors and judgement work, lighter for
routine execution. The router runs in the main session, so its effort is your
Claude Code session effort.

## Playbooks
Playbooks in `playbooks/` are saved procedures (see `playbooks/README.md`). Some
run once (the welcome wizard), some run on a schedule (a Cadence). Available now:
- daily (scheduled) -> run `playbooks/001-daily-summary.md`: a morning calendar
  briefing, run by the executive EA (it holds the calendar read tools).

## Memory layers
Memory is layered:
- **Shared** — `MEMORY.md` (root): what the whole organisation has learned.
- **Team** — `teams/<team>/MEMORY.md`: what a team has learned.
- **Role** — `teams/<team>/<role>/MEMORY.md`: what a specific role has learned.
Load shared always, the team layer when working in that team, and the role layer
when acting as that role. Keep each current in place (replace what is outdated).
Put each learning at the narrowest layer it belongs to; on conflict the more
specific layer wins.

Org-level work in flight (the Executive team's) lives in `projects/` (one folder per
project; see `projects/README.md`), so memory holds only a one-line pointer — open a
project's folder only when the task needs it. Engineering tracks build work on a
ticket system, not in `projects/`.
