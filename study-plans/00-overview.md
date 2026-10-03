# Claude Code Study Plan: Overview

Source: `claude-code-notes.pdf`. Each section file below maps to one major part of the notes.

## How each topic works (hands-on first)
1. **Try**: you do the hands-on task in the sandbox before I explain anything.
2. **Discuss**: we talk through what happened, what surprised you, and what the notes say.
3. **Check**: you answer the check questions in your own words.
4. **Done**: I tick the box only when you've shown real understanding. We don't move on before that.

## Progress tracker
- [ ] `01-getting-started.md`: Start session, Essential commands, Pro tips, Agentic loop, Tools
- [ ] `02-sessions-and-safety.md`: Sessions, branches, resume/fork, context, checkpoints, permissions
- [ ] `03-extending-claude-code.md`: CLAUDE.md, memory, permissions, CLI tools, skills, MCP, subagents, agent teams, hooks, plugins, layering, context costs
- [ ] `04-plan-mode-and-thinking.md`: Plan Mode, extended thinking, adaptive thinking, effort
- [ ] `05-sessions-and-worktrees.md`: Resume, naming, session picker, Git worktrees
- [ ] `06-common-workflows.md`: Tests, PRs, @ references, pipe in/out
- [ ] `07-best-practices.md`: Verification, context, session management, automation, safe autonomy, failure patterns

## Sandbox setup (do this first)
A tiny throwaway project used for every exercise. Nothing here matters, so break things freely.

1. Create `learncc/sandbox/` and `cd` into it.
2. `git init`
3. Ask Claude (or write yourself) a minimal Python todo CLI: `todo.py` with `add`, `list`, `done`, storing tasks in `tasks.json`.
4. Add a small `tests/test_todo.py` (pytest or unittest).
5. `git add -A && git commit -m "initial todo cli"`

**Done when:** `python todo.py add "x"` and `python todo.py list` work, tests pass, and you have one commit.

- [ ] Sandbox ready

## Ground rules
- Risky features (hooks, permissions, `--dangerously-skip-permissions`, worktrees) are tried only inside `sandbox/`.
- Keep a short `notes-to-self` list per section of anything unclear. We revisit it at the end.
- Say "skip" or "go deeper" at any point and I'll adjust.
