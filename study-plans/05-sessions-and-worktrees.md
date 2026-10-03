# 05: Sessions and Worktrees

PDF pages 24–27. Progress: 0/8

## Resume previous conversations
**Concept:** `claude --continue` (most recent in this directory), `claude --resume` (picker or by name), `claude --from-pr 123`, and `/resume` from inside a session. Sessions are stored per project directory, and the picker also shows worktrees of the same repo.
**Hands-on:** Make two sessions in the sandbox, exit, then use each of `--continue`, `--resume`, and `/resume`.
- [ ] Done

### Name your sessions
**Concept:** `/rename auth-refactor`, then `claude --resume auth-refactor` or `/resume auth-refactor`. R in the picker also renames.
**Hands-on:** Name both sessions and resume one by name from the CLI.
- [ ] Done

### Use the session picker
**Concept:** Shortcuts: arrows, Enter, P (preview), R (rename), search, A (all projects), B (this branch), Esc. Forked sessions are grouped under their root.
**Hands-on:** Use P, R, A and B in the picker. Fork a session and find it grouped.
- [ ] Done

## Run parallel Claude Code sessions with Git worktrees
**Concept:** A worktree is a separate directory + branch sharing the same repo history. `claude --worktree <name>` (or `-w`) creates `.claude/worktrees/<name>` on branch `worktree-<name>`. With no name, one is auto-generated. You can also ask Claude to "work in a worktree".
**Hands-on:** In two terminals run `claude -w feature-priority` and `claude -w bugfix-done`. Make a different change in each, with no collisions.
**Discuss:** Why not just branch-switch in one directory?
- [ ] Done

### Subagent worktrees
**Concept:** "Use worktrees for your agents" or `isolation: worktree` in agent frontmatter. They're auto-cleaned if there are no changes.
**Hands-on:** Ask Claude to run two subagents in parallel on separate features using worktrees, then inspect the results.
- [ ] Done

### Worktree cleanup
**Concept:** No changes → removed automatically. Changes or commits → you're prompted to keep or remove. Add `.claude/worktrees/` to `.gitignore`.
**Hands-on:** Exit one worktree with no changes and one with a commit. Compare. Update `.gitignore`.
- [ ] Done

## Manage worktrees manually
**Concept:** Use plain git for control over location and branches. Initialise the dev environment in each worktree (deps, venv).
**Hands-on:** `git worktree add ../sandbox-manual -b manual-test`, run `claude` there, then `git worktree list` and `git worktree remove ../sandbox-manual`.
- [ ] Done

## Create a worktree with a new branch
**Concept:** `git worktree add ../project-feature-a -b feature-a` (new branch) vs `git worktree add ../project-bugfix bugfix-123` (existing branch).
**Hands-on:** Do both variants, then clean up. Explain the difference in your own words.
- [ ] Done
