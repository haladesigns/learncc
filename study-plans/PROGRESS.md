# Progress (update after every completed heading)

**Last updated:** 2026-10-03

## Where we are
- **Current file:** `00-overview.md`
- **Current heading:** Sandbox setup. **The user is building it themselves** (their first exercise). Waiting for their report.
- **Next up:** tick "Sandbox ready" in `00-overview.md`, then start `01-getting-started.md` → Start session

## What to ask the user tomorrow
1. The exact prompt they used to create the todo CLI.
2. What Claude did (did they see explore → act → verify?).
3. Anything that surprised them.
Then check `sandbox/` yourself: `todo.py` works (`add`, `list`, `done`), tests pass with `python -m unittest`, one commit exists.

## Completed sections
(none yet; no Done boxes ticked)

## Environment facts
- Python 3.14.5, git 2.54, Claude Code 2.1.288. No `pytest` (use `unittest`), no `gh` (install when we reach PRs in `06-common-workflows.md`).
- Repo: https://github.com/haladesigns/learncc (branch `main`). Local git identity: `haladesigns` / `hala.francis@gmail.com`.
- `sandbox/` is git-ignored here and has its own separate git repo.
- Resume this session: `cd learncc` then `claude --resume claude-code-study`.

## Notes to self / open questions
- Ask before every push (outward-facing). Committing locally after each section is fine.

## Session log
| Date | What we covered | Left off at |
|---|---|---|
| 2026-10-03 | Read PDF; wrote 8 study-plan files (`00`–`07`), `CLAUDE.md`, `PROGRESS.md`; `git init`, connected to GitHub and pushed (PDF tracked); checked tools; practised `/rename` + resume | Sandbox setup in progress (user building it) |
