# PERMISSIONS.md

The policy layer: who may do what, with which tool. **Deny-by-default** —
anything not listed here stays read-only. A capability is real only when it is
granted here **and** listed in the role's `.claude/agents/<role>.md` allow-list.
Both must agree; the stricter wins. **DELETE is never granted.**

## Action tiers
| Tier | Means |
|---|---|
| READ | view, search, fetch |
| ANNOTATE | add or edit comments / notes |
| EDIT | change existing items |
| CREATE | make new items |
| TRANSITION | change state (complete, archive, move) |
| DELETE | remove permanently — never granted |

## Grants
| Role | Surface | Granted |
|---|---|---|
| executive-cos | repo files | READ, EDIT, CREATE |
| executive-ea | repo files | READ, EDIT, CREATE |
| executive-ea | external connectors (e.g. calendar) | READ |
| executive-qa | everything | READ only |
| engineering-architect | repo files | READ, EDIT, CREATE |
| engineering-frontend | repo files | READ, EDIT, CREATE |
| engineering-backend | repo files | READ, EDIT, CREATE |
| engineering-qa | everything | READ only |

Anything not listed stays READ-only. Connectors start read-only; grant a scoped
write both here and in the role's allow-list, deliberately, when you need it.
