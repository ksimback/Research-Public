---
name: wrapup
description: |
  End-of-session capture so the next session in the same project picks up cleanly.
  Writes a session log, updates a roadmap, archives history, handles in-progress
  git state, and ensures the project's CLAUDE.md pulls everything in next start.
  Use when the user says "wrap up", "stop here for today", "save this session",
  "let's pick this up next time", or invokes /wrapup.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Grep
  - Glob
  - AskUserQuestion
---

## What this skill does

Captures the state of the current project so a future session resumes
without losing context. Writes (all paths relative to the project root):

- `tasks/SESSION.md` — latest wrap-up (overwritten each run)
- `tasks/ROADMAP.md` — living list of what's next (updated each run)
- `tasks/sessions/YYYY-MM-DD-HHMM.md` — archived snapshot (new each run)
- `CLAUDE.md` — ensures a one-line pickup pointer exists

Strictly read-only with respect to git **unless** the user explicitly opts in
to a commit during step 2. Never pushes, never resets, never force-anything.

## Workflow

Run steps in order. Don't batch — each step's output informs the next.

### 1. Capture project state

Run in parallel:
- `git status --short`
- `git branch --show-current`
- `git log -10 --oneline`
- `git diff --stat`
- `git diff --stat --cached`

If not in a git repo, skip git commands and note that in SESSION.md.

Also check whether `tasks/SESSION.md` already exists. If it does, read it —
the prior session's "Pick up here" section is useful context for framing
the new entry.

### 2. Handle in-progress work (BEFORE writing any files)

If `git status --short` shows uncommitted or untracked changes, **stop and
ask the user** via AskUserQuestion. Do not write any files yet.

Question: "Found in-progress work: [list files briefly]. How should I
handle this before wrapping up?"

Options:
- **Commit now** — draft a commit message, confirm with user, then
  `git add` the specific files and `git commit`. Do not push. Do not use
  `-A` or `.`. Do not skip hooks. If a hook fails, fix the underlying issue
  or fall back to "Save as in-progress" — never `--no-verify`.
- **Save as in-progress** — leave git untouched. Capture the file list and
  one-line intent for each in SESSION.md's "In-progress work" section so
  the next session knows what was mid-flight.
- **Skip wrapup** — abort the skill cleanly. Write nothing. Tell the user
  to handle git first and re-run /wrapup.

If the working tree is clean, skip this step.

### 3. Draft `tasks/SESSION.md`

Use this template. Pull values from step 1 and conversation context. Be
specific — vague entries are useless next session. If you can't be
specific about something, ask the user before writing rather than guessing.

```markdown
# Session: {YYYY-MM-DD HH:MM}

**Branch:** {branch}
**Last commit:** {short sha + subject}

## What we worked on
- {1-3 concrete bullets — actual tasks, not vibes}

## Current state
- {what works / what's verified}
- {what's broken or unverified}

## In-progress work
{If user chose "Save as in-progress" in step 2: list files + 1 line of
intent each. If they committed: "Committed in {sha}". If clean: "None —
clean working tree."}

## Decisions made / dead ends
- {non-obvious choices, approaches tried that didn't work — so we don't
  repeat them next session}

## Open questions
- {anything unresolved that needs an answer to move forward}

## Pick up here
{1-3 sentences. Where to start next session. Reference specific files,
functions, or commands. Should be enough that a fresh session can act
without re-asking the user "what were we doing?"}
```

### 4. Update `tasks/ROADMAP.md`

If it exists: read it, then update — add new items that surfaced this
session, mark/remove completed items, reorder if priorities shifted.
Preserve existing structure and tone.

If it doesn't exist: create it with this template:

```markdown
# Roadmap

_Updated: {YYYY-MM-DD}_

## Now
- {actively in flight}

## Next
- {queued, ready to start}

## Later
- {known but not prioritized}

## Done (recent)
- {completed in last few sessions, for context}
```

### 5. Archive the session

Copy the SESSION.md you just wrote to
`tasks/sessions/{YYYY-MM-DD-HHMM}.md`. Create `tasks/sessions/` if it
doesn't exist. Never delete old archives — they're cheap and worth keeping.

### 6. Ensure pickup pointer in project CLAUDE.md

Check for `CLAUDE.md` at the project root.

- **If it doesn't exist:** create it:
  ```markdown
  # Project Context

  At session start, read `tasks/SESSION.md` and `tasks/ROADMAP.md` if they
  exist — they contain the prior session's wrap-up and current priorities.
  ```

- **If it exists and already references `tasks/SESSION.md`:** leave it alone.

- **If it exists but doesn't reference `tasks/SESSION.md`:** append:
  ```markdown

  ## Session continuity
  At session start, read `tasks/SESSION.md` and `tasks/ROADMAP.md` if they
  exist — they contain the prior session's wrap-up and current priorities.
  ```

### 7. Confirm to the user

One short message. List files written, confirm CLAUDE.md pickup is wired
up, and (if applicable) note any commit you made and its sha. Stop there —
no fluff.

## Failure modes — do NOT

- Do not write to `~/.claude/` or any global location. Wrap-ups are
  per-project; project state belongs in the project.
- Do not run destructive git commands (`reset --hard`, `clean -f`,
  `checkout .`, force-push). Never.
- Do not stage or commit without explicit user confirmation from step 2.
- Do not use `git add -A` or `git add .` — name files explicitly so you
  don't accidentally include `.env`, credentials, or stray artifacts.
- Do not skip hooks (`--no-verify`). If a hook fails, fix the cause or
  switch to "Save as in-progress".
- Do not write vague SESSION.md entries like "worked on the app". If you
  can't be specific, ask the user before writing.
- Do not delete old session archives.
- Do not push to any remote.
