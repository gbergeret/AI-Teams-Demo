# System architecture

The one map of how this organisation is wired — the router, the two teams, request
flow, gates, memory, permissions, and the repo layout. Read this to onboard or
audit; load the specifics on demand from the files it points to.

## Request flow
```
User
  └─▶ Router  (CLAUDE.md, runs in the main session — counts the teams a request touches)
        │
        ├─ One team ─▶ main session wears that team's front-door hat
        │                 ├─ Executive → CoS ─▶ EA (executes)
        │                 └─ Engineering → Architect ─▶ Frontend / Backend (build)
        │                       ├─ QA loop   (graded findings; only [BLOCK] gates; up to 3 rounds)
        │                       └─ security  (after QA, only when security-relevant)
        │
        └─ Cross-team ─▶ router stays the router:
              pick a lead team, define the handoff contract, call each front door
              plan-only (briefs, no spawning), fan out to specialists, synthesise.
        ◀─ integrates ─ returns to you  (only a PASS or an escalation reaches you)
```

## Teams and roles
Each role has `teams/<team>/<role>/ROLE.md` (mandate) **and**
`.claude/agents/<team>-<role>.md` (runtime manifest) — both must agree (see "Adding
a role" in `CLAUDE.md`). The front doors run in the main session when it wears their
hat; the specialists run as subagents.

| Team | Role | Job | Reads/writes |
|---|---|---|---|
| Executive | CoS (front door) | triage / orchestrate the business | this repo |
| Executive | EA | executes the CoS's briefs | this repo + granted connectors |
| Executive | QA | quality gate | read-only |
| Executive | CISO | org-wide policy security gate (after QA) | read-only |
| Engineering | Architect (front door) | break down / delegate the build | this repo |
| Engineering | Frontend | UI / client-side build | this repo |
| Engineering | Backend | API / data / server-side build | this repo |
| Engineering | Security Engineer | hands-on hardening (after QA) | this repo |
| Engineering | QA | build quality gate | read-only |

## Gates (the order is invariant)
1. **QA** — always in the path for non-trivial work; each team runs its own, grades
   findings, only `[BLOCK]` gates resubmission.
2. **Security** — conditional, **after** QA, only when a change is security-relevant.
   Two scopes: the **Security Engineer** hardens inside engineering (no executive
   sign-off needed); the **CISO** reviews org-wide *policy* changes (permissions,
   connector grants, `.claude/settings.json`, agent allow-lists) — by change type,
   not by team. Both are read-only in judgement: they flag, the producing role fixes.

A gate is never an entry point and always runs on QA-passed work.

## Memory
- **Shared** `MEMORY.md`; **team** `teams/<team>/MEMORY.md`; **role**
  `teams/<team>/<role>/MEMORY.md` — the more specific wins.
- Org-level work in flight lives in `projects/` (a one-line pointer in `MEMORY.md`,
  Executive team; Engineering uses a ticket system); context is loaded on demand via
  `context/INDEX.md`.

## Permissions
Deny-by-default, **action tiers** (READ/ANNOTATE/EDIT/CREATE/TRANSITION/DELETE, with
DELETE never granted), the **two-place model** (`PERMISSIONS.md` grant + the agent's
`tools:` allow-list, stricter wins), and the `.claude/settings.json` floor. The
constitution is `context/GOLDEN-RULES.md` (including the prompt-injection guardrail).

## Repo layout
```
AI-Teams-Demo/
├── CLAUDE.md            # the master router: which team, how teams combine
├── MEMORY.md            # shared org memory (one-line pointer per project)
├── PERMISSIONS.md       # action-tier grants matrix (both teams)
├── projects/           # org-level work in flight (Executive), one folder per project
├── context/            # GOLDEN-RULES, INDEX, PRINCIPLES, PROFILE, VOICE, SYSTEM-ARCHITECTURE
├── teams/<team>/       # per-team MEMORY.md + <role>/ (ROLE.md + MEMORY.md)
│   ├── executive/      # cos (front door), ea, qa, ciso
│   └── engineering/    # architect (front door), frontend, backend, security, qa
├── playbooks/          # welcome wizard + update-from-upstream + daily summary
├── scripts/            # token-cost report
└── .claude/
    ├── agents/          # <team>-<role> manifests + _TEMPLATE.md
    └── settings.json    # permissions floor
```
