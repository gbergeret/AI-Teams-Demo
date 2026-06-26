# Playbooks

A playbook is a saved procedure the organisation can run: a short Markdown file
with a clear goal and steps. Keep them readable.

## Naming
Numbered for a stable order: `NNN-short-name.md`. `000` is reserved for
first-run setup.

## How they run
- **On demand:** you ask for it by name.
- **Scheduled:** some run on a cadence (e.g. daily); set them up as a recurring
  routine in Claude Code. See each playbook's **Cadence** line.
- **One-time:** some playbooks run once and then delete themselves (see `000`).

## Playbooks here
- `000-welcome-wizard.md`: first-run onboarding. Runs once, then deletes itself.
- `001-daily-summary.md`: a morning briefing from the calendar (scheduled, read-only).
