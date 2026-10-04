# Progress (update after every completed heading)

**Last updated:** 2026-10-04 (Tools done; `01-getting-started.md` complete)

## Where we are
- **Current file:** `02-sessions-and-safety.md`
- **Current heading:** Work with sessions (not begun)
- **Next up:** `02-sessions-and-safety.md` → Work with sessions. Remember to write a `### Summary` block per heading before moving on (see CLAUDE.md).

## Completed sections
- `00-overview.md` → Sandbox setup (2026-10-04). Summary is in that file. Sandbox repo is pushed to `learncc-sandbox` (`806595b`).
- `01-getting-started.md` → Start session (2026-10-04). Summary is in that file.
- `01-getting-started.md` → Essential commands (2026-10-04). Summary is in that file.
- `01-getting-started.md` → Pro tips (2026-10-04). Summary is in that file. Sandbox changes were reverted with `git restore .`.
- `01-getting-started.md` → The agentic loop (2026-10-04). Summary is in that file. The `delete` command and tests are in the `sandbox/` working tree, uncommitted. Follow-up summary there covers the observed failure loop.
- `01-getting-started.md` → Tools (2026-10-04). Summary is in that file. Code intelligence needed `npm install -g pyright` (the separate language server) plus the `pyright-lsp` plugin; first run saw `LSP` hover calls and a `Found 2 new diagnostic issues` line.

## Environment facts
- Python 3.14.5, git 2.54, Claude Code 2.1.288, pyright 1.1.414 (npm global), `pyright-lsp` plugin enabled. No `pytest` (use `unittest`), no `gh` (install when we reach PRs in `06-common-workflows.md`).
- Repo: https://github.com/haladesigns/learncc (branch `main`). Local git identity: `haladesigns` / `hala.francis@gmail.com`.
- `sandbox/` is git-ignored here and has its own separate git repo (remote: https://github.com/haladesigns/learncc-sandbox.git, same local identity).
- Resume this session: `cd learncc` then `claude --resume claude-code-study`.

## Notes to self / open questions
- Ask before every push (outward-facing). Committing locally after each section is fine.
- Open: the concept says Claude is "scoped to the directory you launch it from", but `CLAUDE.md` also loads from parent directories (tested). Check what the PDF says. *Expectation (docs, memory):* `CLAUDE.md` loads from the launch directory and every directory above it at startup, and from subdirectories on demand. The PDF line is probably a simplification.
- Open: is a conversation cleared with `/clear` still resumable via `claude -r`? *Expectation (docs, sessions):* yes. Test it. `claude commit` not yet tried (optional).
- Open: how permissions treat files outside the launch directory (try in the permissions section). *Expectation (docs, permissions):* default mode prompts for reads and edits outside the working directory.
- Resolved (2026-10-04): the failure loop. With a failing `test_main_add`, Claude fixed `todo.py:58` and re-ran the tests on its own. With all tests green it changed nothing. Summary is in `01-getting-started.md` → The agentic loop (follow-up).
- Open: does Claude stop and ask after repeated failed fixes? *Expectation (docs, interrupt behaviour):* it keeps trying; Esc interrupts. Not tested, not yet checked in the docs.
- Open: does the new `sandbox/CLAUDE.md` testing rule ("every command needs a test through `main()`") change what Claude does unprompted? *Expectation:* it follows project instructions, but adherence isn't guaranteed. Test by asking for a new command, e.g. `clear`. Not yet checked in the docs.
- Open: does an LSP diagnostics line also appear after hover-only lookups (seen once, no edit)? *Expectation (docs, code-intelligence):* docs describe diagnostics after edits only. Unconfirmed.
- Open: a web tool called directly in the main session (permission prompt) and where the UI shows a subagent's tool calls (`/agents`, Ctrl+O) were skipped in Tools. *Expectation:* a first direct WebFetch asks permission; `/agents` or the transcript view shows subagent calls.
- Open: keep or `git restore .` the uncommitted `delete` work in `sandbox/`. *Expectation:* Claude won't commit unless asked.
- Open: Tab completion not tried. *Expectation (docs, interactive-mode):* Tab accepts the autocomplete suggestion when one is showing; `@` starts file-path completion.

## Session log
| Date | What we covered | Left off at |
|---|---|---|
| 2026-10-03 | Read PDF; wrote 8 study-plan files (`00`–`07`), `CLAUDE.md`, `PROGRESS.md`; `git init`, connected to GitHub and pushed (PDF tracked); checked tools; practised `/rename` + resume | Sandbox setup in progress (user building it) |
| 2026-10-04 | Sandbox built via plan mode, tests pass, pushed (rebased over GitHub's initial commit); discussed plan mode and rebase vs force push; added summary-before-moving-on rule to CLAUDE.md | Ready to start `01-getting-started.md` → Start session |
| 2026-10-04 (later) | `01-getting-started.md` → Start session: compared launches from `sandbox/` and `learncc/`; on-demand file reading; `CLAUDE.md` loads from parent dirs | Ready for Essential commands |
| 2026-10-04 (later) | `01-getting-started.md` → Essential commands: `claude "..."`, `-p`, `-c`, `-r`, `/help`, `/clear`, `exit` vs Ctrl+C | Ready for Pro tips |
| 2026-10-04 (later) | `01-getting-started.md` → Pro tips: vague vs specific prompts, step-by-step list, analyze first, shortcuts; added "expectation for every open item" rule to CLAUDE.md; reverted sandbox changes | Ready for The agentic loop |
| 2026-10-04 (later) | `01-getting-started.md` → The agentic loop: labelled gather/act/verify on a "add delete command and test" request; pure question needed no tools | Ready for Tools |
| 2026-10-04 (later) | Failure loop observed: all-green run changed nothing (no test covered `main add`); with `test_main_add` failing, Claude fixed `todo.py:58` and re-ran tests itself; checked `unittest discover` claims; added `sandbox/CLAUDE.md` testing rule | Tools: code intelligence left |
| 2026-10-04 (later) | `01-getting-started.md` → Tools finished: code intelligence needed a separately installed server (`npm install -g pyright`); saw `LSP` hover and a 2-error diagnostics line; restated file ops / search / execution; cleaned `todo.py:58` | Ready for `02-sessions-and-safety.md` → Work with sessions |
