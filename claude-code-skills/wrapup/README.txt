# wrapup

A Claude Code skill that captures end-of-session state so the next session in the same project picks up cleanly — without losing context, decisions, or in-progress work.

## What it does

When invoked, `/wrapup` writes the following to your project root:

- **`tasks/SESSION.md`** — latest wrap-up with what you worked on, current state, in-progress work, decisions, open questions, and a "Pick up here" pointer (overwritten each run).
- **`tasks/ROADMAP.md`** — living list of Now / Next / Later / Done items (updated each run).
- **`tasks/sessions/YYYY-MM-DD-HHMM.md`** — archived snapshot of the session (new each run, never deleted).
- **`CLAUDE.md`** — ensures a one-line pickup pointer exists so future sessions read `SESSION.md` and `ROADMAP.md` automatically.

It also handles in-progress git state by asking whether to commit, save as in-progress, or abort. It never pushes, never resets, never force-anything.

## Why use it

Claude Code sessions are stateless. Without something like this, every new session starts cold — you spend the first few minutes re-orienting Claude (or yourself) on what you were doing, what worked, what didn't, and where to resume. `wrapup` makes that orientation a 5-second read instead. While the native /resume skill does some of this, if the context is overloaded it can cause important details to be lost - wrapup ensures nothing is lost so you can pick up exactly where you left off

## Install

Drop the skill into your Claude Code skills directory:

```bash
# macOS / Linux
mkdir -p ~/.claude/skills/wrapup
curl -o ~/.claude/skills/wrapup/SKILL.md https://raw.githubusercontent.com/ksimback/Research-Public/claude-code-skills/wrapup/main/SKILL.md
```

```powershell
# Windows (PowerShell)
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills\wrapup"
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/ksimback/Research-Public/claude-code-skills/wrapup/main/SKILL.md" `
  -OutFile "$env:USERPROFILE\.claude\skills\wrapup\SKILL.md"
```

Or clone the repo into the skills directory:

```bash
git clone https://github.com/ksimback/Research-Public/claude-code-skills/wrapup.git ~/.claude/skills/wrapup
```

The skill is registered as soon as `SKILL.md` is present at `~/.claude/skills/wrapup/SKILL.md`.

## Usage

In any Claude Code session, invoke:

```
/wrapup
```

Or just say one of these and Claude will pick it up:

- "wrap up"
- "stop here for today"
- "save this session"
- "let's pick this up next time"

Claude will:

1. Capture git state (branch, recent commits, diff stats).
2. If there's uncommitted work, ask how to handle it (commit / save as in-progress / abort).
3. Draft `tasks/SESSION.md` with concrete details from the session.
4. Update `tasks/ROADMAP.md`.
5. Archive a timestamped copy under `tasks/sessions/`.
6. Wire up `CLAUDE.md` so the next session reads everything automatically.

## Safety guarantees

The skill is intentionally conservative with git:

- Never runs destructive commands (`reset --hard`, `clean -f`, `checkout .`, force-push).
- Never uses `git add -A` or `git add .` — files are named explicitly.
- Never skips hooks (`--no-verify`).
- Never pushes to any remote.
- Never stages or commits without explicit user confirmation.

If a pre-commit hook fails, it falls back to "Save as in-progress" rather than bypassing the hook.

## Project layout after first run

```
your-project/
├── CLAUDE.md                    # pickup pointer (created/extended)
└── tasks/
    ├── SESSION.md               # latest session (overwritten)
    ├── ROADMAP.md               # living roadmap
    └── sessions/
        ├── 2026-05-04-1430.md   # archived snapshots
        └── 2026-05-05-0915.md
```

## License

MIT
