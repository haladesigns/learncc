# Progress (update after every completed heading)

**Last updated:** 2026-10-04

## Where we are
- **Current file:** `01-getting-started.md`
- **Current heading:** Start session (not begun)
- **Next up:** `01-getting-started.md` → Start session. Remember to write a `### Summary` block per heading before moving on (see CLAUDE.md).

## Completed sections
- `00-overview.md` → Sandbox setup (2026-10-04). Summary is in that file. Sandbox repo is pushed to `learncc-sandbox` (`806595b`).

## Environment facts
- Python 3.14.5, git 2.54, Claude Code 2.1.288. No `pytest` (use `unittest`), no `gh` (install when we reach PRs in `06-common-workflows.md`).
- Repo: https://github.com/haladesigns/learncc (branch `main`). Local git identity: `haladesigns` / `hala.francis@gmail.com`.
- `sandbox/` is git-ignored here and has its own separate git repo (remote: https://github.com/haladesigns/learncc-sandbox.git, same local identity).
- Resume this session: `cd learncc` then `claude --resume claude-code-study`.

## Notes to self / open questions
- Ask before every push (outward-facing). Committing locally after each section is fine.

## Session log
| Date | What we covered | Left off at |
|---|---|---|
| 2026-10-03 | Read PDF; wrote 8 study-plan files (`00`–`07`), `CLAUDE.md`, `PROGRESS.md`; `git init`, connected to GitHub and pushed (PDF tracked); checked tools; practised `/rename` + resume | Sandbox setup in progress (user building it) |
| 2026-10-04 | Sandbox built via plan mode, tests pass, pushed (rebased over GitHub's initial commit); discussed plan mode and rebase vs force push; added summary-before-moving-on rule to CLAUDE.md | Ready to start `01-getting-started.md` → Start session |
