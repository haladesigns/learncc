# 02: Sessions and Safety

PDF pages 6–7. Progress: 0/7

## Work with sessions
**Concept:** Every message, tool use and result is stored. This enables rewind, resume and fork. Each new session starts with a fresh context window. Auto memory and CLAUDE.md are what persist.
**Hands-on:** Start a session and tell Claude a fact ("my favourite colour is teal"). `/clear` or exit, start a new session and ask for it. Then repeat after saving it to CLAUDE.md.
**Discuss:** What carried over and what didn't, and why?
- [ ] Done

## Work across branches
**Concept:** Sessions are tied to the directory. Switching branches changes the files Claude sees, but the conversation history stays.
**Hands-on:** In the sandbox: `git switch -c feature-x`, change a file, ask Claude about it. Switch back to `main` and ask again. Notice it remembers the conversation but sees different files.
**Discuss:** What could go wrong if you switch branches mid-session? How do worktrees help?
- [ ] Done

## Resume or fork sessions
**Concept:** `--continue`/`--resume` reuse the same session ID. `--fork-session` branches off. Session-scoped permissions are not restored. Two terminals on one session interleave messages.
**Hands-on:**
1. Approve a permission, exit, then `claude --continue`. Is it asked again?
2. `claude --continue --fork-session`, then try a different approach.
3. (Optional) Resume the same session in two terminals and watch it get jumbled.
**Discuss:** When fork vs resume? Why can't two terminals share a session cleanly?
- [ ] Done

## Manage context with skills and subagents
**Concept:** Skills load on demand (descriptions at start, full content when used). `disable-model-invocation: true` hides a skill until you invoke it. Subagents get a fresh, separate context and return only a summary.
**Hands-on:** Run `/context` before and after asking a subagent to "investigate every file in the repo and summarize". Compare the main context's growth with doing the same thing inline.
**Discuss:** Why is the subagent version cheaper for the main context?
- [ ] Done

## Checkpoints and permissions
**Concept:** Two safety mechanisms: checkpoints undo file changes, and permissions control what Claude can do without asking.
**Hands-on:** Run `/permissions` and read what's allowed. Trigger a permission prompt (a shell command) and choose "allow once" vs "always".
**Discuss:** What's the difference between undoing and preventing?
- [ ] Done

## Undo changes with checkpoints
**Concept:** Claude snapshots files before editing. Esc Esc (or `/rewind`) restores the conversation, the code, or both.
**Hands-on:** Ask Claude to refactor `todo.py` in a questionable way. Press Esc twice, restore code only, and verify with `git diff`. Then try restoring conversation only.
**Discuss:** How is this different from git? What won't checkpoints undo?
- [ ] Done

## Work effectively with Claude Code
**Concept:** Ask Claude about itself, use `/init`, `/agents` and `/doctor`. Be conversational, interrupt and steer, be specific upfront, explore before implementing (Shift+Tab twice for plan mode), and delegate rather than dictate.
**Hands-on:** Ask "how do I set up hooks?". Run `/init`, `/doctor` and `/agents`. Interrupt a long-running task with a correction mid-way.
**Discuss:** What does "delegate, don't dictate" look like in a prompt? Show me one of each.
- [ ] Done
