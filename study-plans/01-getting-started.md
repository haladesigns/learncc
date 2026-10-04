# 01: Getting Started

PDF pages 4–5. Progress: 4/5

## Start session
**Concept:** Claude Code runs in your terminal, scoped to the directory you launch it from.
**Hands-on:** `cd sandbox && claude`. Ask "what does this project do?" Then exit and relaunch from `learncc/` instead. Ask the same question.
**Discuss:** What differed between the two launches, and why does the launch directory matter?

### Summary
- **Task:** Launch Claude in `sandbox/` and in `learncc/`, ask "what does this project do?" each time, and compare.
- **What happened:**
  - First (session started in `learncc/`, then `cd sandbox`): Claude had to explore. It used Bash (`ls`, `git log`, `cat README*`, `.gitignore`) and Read (`todo.py`), and described the todo CLI. It never opened `tests/test_todo.py`.
  - Second (fresh launch in `learncc/`): no tool calls. It described the study repo, tutor rules and where we left off, because `CLAUDE.md` and the imported `PROGRESS.md` load at startup.
  - Third (fresh launch in `sandbox/`, run as a test): a hybrid. It showed "Read 1 file, listed 1 directory", so it explored on demand and then described the todo CLI in detail (commands, `tasks.json`, `unittest` tests). But it also described both `learncc/` (the parent, with the tutor setup) and `sandbox/` ("where we are now"), and said the todo app is "a small codebase for the exercises". So `learncc/CLAUDE.md` was picked up from the parent directory, and the launch directory still decided which files it opened for detail.
- **Your answers, in your words:**
  - Claude learns about a project through tool calls, and opens files only when that helps the task at hand (on-demand).
  - You first guessed it reads only the top-level folder. I corrected that: it skipped `tests/` by choice, not because it can't go into subfolders.
  - You noticed the `learncc/` answer had no tool calls.
  - Launch directory "sets the focus of the interaction as the conversation begins".
- **Corrections I made:**
  - Bash and Read are different tools, and `todo.py` was read with Read.
  - `.gitignore` was not how Claude understood the project.
  - My claim that the sandbox launch would behave like a standalone project was only partly right. `CLAUDE.md` is also loaded from parent directories, which the third test confirmed. The result was a mix of both: parent context from `CLAUDE.md` plus on-demand reading of the launch directory (your screenshot corrected my first note of "answered like the tutor").
  - The launch directory decides which `CLAUDE.md` files load, where Claude reads freely, and the default exploration scope.
- **Open questions:**
  - The concept line says Claude is "scoped to the directory you launch it from". The test shows instructions still load from parents. Does the PDF say anything about this? I haven't checked it yet. *Expectation (docs, memory):* `CLAUDE.md` loads from the launch directory and every directory above it at startup, and from subdirectories on demand when Claude reads files there. That matches the test, so the PDF's wording is probably a simplification.
  - Exactly how permissions treat files outside the launch directory (to try later in the permissions section). *Expectation (docs, permissions):* in default mode, reads and edits outside the working directory prompt for approval. A setting can turn those prompts into refusals.
- [x] Done

## Essential commands
**Concept:** The core launch commands and in-session commands you'll use daily.
**Hands-on:** Run each in turn and note what it does:
`claude "list the functions in todo.py"`, `claude -p "explain todo.py"`, `claude -c`, `claude -r`, then in-session `/help`, `/clear`, and exit with `exit` or Ctrl+C. (Try `claude commit` after making a trivial edit.)
**Discuss:** When would you pick `-p` over interactive mode? What is the difference between `-c` and `-r`? What does `/clear` actually discard?

### Summary
- **Task:** From `sandbox/`, run `claude "list the functions in todo.py"`, `claude -p "explain todo.py"`, `claude -c`, `claude -r`, then `/help`, `/clear`, `exit` and Ctrl+C.
- **What happened:**
  - `claude "list the functions in todo.py"`: two search calls (Glob for the filename, Grep with a regex for function definitions) instead of reading the whole file. It returned a table of line numbers and function names with parameters.
  - `claude -p "explain todo.py"`: one prompt, printed the answer and exited with no interactive session. It used Read on `todo.py` and grouped the explanation into storage, operations and CLI.
  - `claude -c`: loaded straight into the most recent session in this directory.
  - `claude -r`: opened a session picker to choose which session to resume.
  - `/help`: a tabbed menu with general shortcuts, common commands and custom commands.
  - `exit` quits immediately. Ctrl+C needs to be pressed twice.
- **Your answers, in your words:**
  - Pick `-p` when calling Claude from a script.
  - `-r` shows a page to select a session; `-c` goes directly into the last active session.
  - `/clear` clears the session's context; use it when starting a new task so that task isn't polluted with unnecessary data.
- **Corrections / additions I made:**
  - The "Search" calls were two tools: Glob (filenames) and Grep (content regex).
  - `claude "..."` opens an interactive session with that first prompt, while `-p` prints and exits. `-p` also works in pipes and CI.
  - `-c` is the most recent conversation in the current directory.
  - `/clear` only affects what Claude remembers. File edits and commits stay, and `CLAUDE.md` is still loaded afterward.
- **Not done / open:**
  - `claude commit` was not tried (optional).
  - Check whether a conversation cleared with `/clear` is still resumable with `-r`. *Expectation (docs, sessions):* yes, you can return to it after `/clear`. Passing a name (`/clear old-name`) keeps it easy to find. Test it to confirm.
- [x] Done

## Pro tips
**Concept:** The three habits from the notes: be specific, use step-by-step instructions, let Claude explore first. Plus keyboard shortcuts.
**Hands-on:**
1. Give a vague prompt ("fix the bug"), then a specific one on a bug you plant in `todo.py`. Compare results.
2. Give a 3-step list prompt (e.g. add a `priority` field → update `list` → update tests).
3. Ask Claude to "analyze the structure of todo.py" before asking for a change.
4. Try `?`, Tab completion, the up-arrow history, and typing `/` to list commands.
**Discuss:** What made the specific prompt better? When is a vague prompt actually fine?

### Summary
- **Task:** Plant a bug in `todo.py` and compare a vague prompt ("fix the bug") with a specific one. Then try a 3-step list prompt, an "analyze first" prompt, and the keyboard shortcuts.
- **What happened:**
  - Vague prompt: one Read on `todo.py`, found the bug easily.
  - Specific prompt (task, file, bug symptom: every task has the same id, expectation: unique ids): one Read, fixed it, ran the tests and confirmed they pass.
  - 3-step list (add `priority`, update `list` to show it, update the tests): followed in order with no issues. It described the new field, showed an example of the list output, and ran all the tests, which passed.
  - "Analyze the structure of todo.py": Read on `todo.py` and Bash (`git diff --stat && git diff && ls`). It summarised the functions as storage, operations and CLI.
  - Shortcuts: `?` shows keybindings, up-arrow shows prompt history (double Esc clears), `/` starts the slash-command search.
- **Your answers, in your words:**
  - Specific prompts are better because Claude isn't spending time and tokens on unnecessary work caused by shallow or generic prompts.
  - The vague prompt worked here, but this may be contrived. On a larger project, vague prompts would likely mean extra work and more tokens.
- **Corrections / additions I made:**
  - Here both prompts took one Read, so the file was too small to show a difference. The extra cost shows up when the answer isn't obvious: more Glob/Grep/Read calls, a wrong target, or clarifying questions.
  - A vague prompt is fine when the scope is tiny, when exploring, or when a wrong guess is cheap to undo.
  - The "analyze first" run used `git diff`, so it could see the uncommitted planted bug.
  - A full test run at the end of a multi-step prompt checks the result.
- **Not done / open (with my expectation for each):**
  - Tab completion was not reported. *Expectation (docs, interactive-mode):* Tab accepts the autocomplete suggestion when one is showing, such as slash commands, and `@` triggers file-path completion. Plain Tab completion of file paths without `@` isn't documented, so test it.
  - Whether the analysis mentioned the planted bug or the diff was not reported. *Expectation:* it likely did, since `git diff` showed the uncommitted change. Docs can't settle this; check the run's output.
  - Whether the tests were updated or newly added for `priority` was not checked. *Expectation:* "update the tests" should lead to new test cases that exercise `priority` plus adjusted existing ones. Verify with `git diff tests/`.
  - The `sandbox/` working tree has uncommitted changes (priority field and possibly the planted bug). *Expectation:* Claude doesn't commit unless asked, so they should still be sitting there. Decide whether to keep or `git restore .`.
  - Double Esc, from the docs: with text in the prompt it clears it and saves the draft so Up recalls it; with an empty prompt it opens the rewind menu.
  - Resolved at sign-off: `git diff --stat` showed `tests/test_todo.py` changed (+35, -1) along with `todo.py`, so the tests were edited as asked. The changes were then reverted with `git restore .`, and the tests pass again.
- [x] Done

## The agentic loop
**Concept:** Gather context → take action → verify, repeated and adaptive. Claude Code is the harness that provides tools, context management and execution.
**Hands-on:** Ask for a small feature ("add a `delete` command and a test"). Watch the output and label each step you see as *gather*, *act* or *verify*. Then ask a pure question and note that it only gathers.
**Discuss:** Which request needed which phases? What decides what happens next?

### Summary
- **Task:** Prompt "add a delete command and a test", labelling each step as gather, act or verify. Then ask a pure question: "what does `complete_task` do?"
- **What happened:**
  - Delete request: Gather (Read `todo.py` and `tests/test_todo.py`). Act (added `delete_task`, a `delete` subcommand, changed the catch-all `else` in `main` to `elif args.command == "done"`, and added 4 tests). Verify (ran the suite: 10 tests OK).
  - Pure question: no tool calls. The code was already in context from the earlier Read, so Claude answered directly. It was gather only, no edits and no tests.
- **Your answers, in your words:**
  - Delete request: Gather read 2 files to match the existing style. Act added `delete_task` and wired up the subcommand. Verify ran the tests.
  - Pure question: only the Read tool would be called (gather), and no other phase activates.
  - What decides what happens next: the tests passing. If any fail, Claude would alert the user and probably look for the bug itself.
- **Corrections I made:**
  - For the pure question there were no tool calls at all this time. Read would be needed only in a fresh session.
  - The result of each step decides the next one, not only the tests. On a failing test, Claude would normally loop back on its own (gather the failure, act on a fix, verify again) and stop to ask only if stuck or a decision is needed. The user can interrupt with Esc.
- **Not done / open (with my expectation):**
  - ~~The failure loop was described, not observed.~~ Resolved, see the follow-up below.
  - `sandbox/` has uncommitted changes (delete command and tests). *Expectation:* Claude doesn't commit unless asked. Decide whether to keep or `git restore .`.
- [x] Done

### Summary (follow-up: the failure loop, observed 2026-10-04)
- **Task:** A bug was planted at `todo.py:58` (`add_task(args.text, path, name)`, with `name` undefined). Fresh session in `sandbox/`, prompt: "run the tests in @tests\ and fix anything that fails".
- **What happened:**
  - Run 1 (no new test): all 10 tests passed, so Claude changed nothing. It correctly read the "No task with id 5" and "Deleted 1" lines as normal printed output, not failures. No existing test called `main(["add", ...])`, so the broken `add` command went unnoticed. (I wrongly predicted the `add` tests would fail. They call `add_task` directly, which is fine.)
  - Run 2 (after adding `test_main_add`, which calls `main(["add", "a"], path=...)`): the test failed with `NameError`. Claude found the cause, edited line 58 back to `add_task(args.text, path)`, and **re-ran the tests by itself**. All 11 passed. It changed only that line and did not commit.
  - Claude's report said `discover -s tests` "doesn't work here" without `__init__.py`. I ran it: `discover -s tests` and `tests.test_todo` both work (11 tests OK). Only bare `python -m unittest discover` finds 0 tests. The cause it gave is right but the command it named is wrong. It also said the stray argument came from the `delete` work, which was a guess, since you planted it.
- **Your answers, in your words:**
  - Green tests vs a broken command: the passing conditions must be defined ahead of time, as an explicit test.
  - What changed in run 2: an explicit test was added and that is where it failed.
  - How to improve Claude: ask it to make sure all functions have equivalent tests.
- **Corrections I made:**
  - The new test went through `main(["add", ...])`, not `add_task`. Tests for `add_task` already existed and passed. The gap was the `add` branch of `main`.
  - You (the human) decide what counts as "working". Claude's verify step is only as good as the tests that encode it.
  - Sharper rule than "every function": every command and code path. Could go in `CLAUDE.md`.
  - Claude's summary can contain small errors (wrong command named, guessed origin) even when the work is right. Spot-check claims.
- **Not done / open (with my expectation):**
  - Does Claude stop and ask when it can't fix a failure after several tries? *Expectation (docs, agentic loop / interrupt):* it keeps trying, and the user can interrupt with Esc. Not tested.
  - Would a `CLAUDE.md` rule ("every command needs a test through `main`") change what Claude does unprompted? *Expectation:* it follows project instructions, but the docs say adherence isn't guaranteed. Not tested.
  - The `delete` work and the new `test_main_add` are still uncommitted in `sandbox/`. *Expectation:* Claude doesn't commit unless asked.

## Tools
**Concept:** Five categories (file operations, search, execution, web, code intelligence) plus orchestration tools such as subagents and asking questions.
**Hands-on:** Craft one prompt per category that triggers it (read/edit a file, grep for text, run tests, fetch a web page, ask about a type error). Note which tool names appear.
**Discuss:** Why does having tools make Claude "agentic"? What did code intelligence need that the others didn't?

### Summary
- **Task:** One prompt per tool category, noting the tool names that appear.
- **What happened:**
  - File operations: Read on `todo.py` and `tests/test_todo.py`, and Edit calls when adding `delete_task` (from the Pro tips and agentic loop exercises).
  - Search: Glob (filename) and Grep (regex for function definitions) from `claude "list the functions in todo.py"`.
  - Execution: Bash ran `ls`, `git log`, `git diff`, and the `unittest` suite at the end of the priority and delete requests.
  - Web: not tried directly. Instead, a request for code review docs made Claude launch a `claude-code-guide` subagent (Agent tool, an orchestration tool). The subagent called **WebFetch five times** (the docs index, then `code-review.md`, `security-guidance.md`, `github-actions.md`, `ultrareview.md`) and returned its report with `SubagentHandback`. The tool list is in the saved transcript file for the task, not in the completion notice. The result was saved as `study-plans/code-review-notes.md`.
  - Code intelligence (tried 2026-10-04, three attempts):
    - Attempt 1, "where is `delete_task` defined and what calls it": used `Search`, so plain text matching, 5 lines.
    - Attempt 2, "are there any type errors in todo.py", with `pyright-lsp` installed but no server: `Read`, then `Bash` ran `python -m mypy` and `python -m pyright`, both "No module named". No LSP tool. Claude suggested `pip install mypy`.
    - Check: `pyright-lsp` was enabled in `settings.json`, but `pyright-langserver` was not on PATH. The docs (code.claude.com/docs/en/plugins/code-intelligence.md) say the plugin "doesn't include the language server".
    - Installed it with `npm install -g pyright` (pyright 1.1.414, command found in the plugin's README).
    - Attempt 3, in a fresh session with a bad call at `todo.py:58`: `LSP(operation: "hover", symbol: "name")` and `LSP(... "add_task")`, then `Found 2 new diagnostic issues in 1 file`. Pyright reported `"name" is not defined` (reportUndefinedVariable) and `Expected 2 positional arguments` (reportCallIssue), both at line 58, column 42.
    - The diagnostics line appeared after an LSP hover with no edit. The docs describe diagnostics after edits only, so this is an observation, not a documented rule.
- **Your answers, in your words:**
  - Web: a lookup agent was launched with the docs prompt, it sent back the page Claude needed, and Claude summarised and categorised it.
  - You noticed that no tool list was shown for the subagent and asked where to find it.
  - File operations: reading and making changes to files.
  - Search: finding files by name and text by content (both provided as parameters).
  - Execution: lets Claude run commands, edit files and other actions to complete tasks.
  - Why tools make Claude agentic: Claude has agency through these tools to carry out actions.
  - What code intelligence needed: an install of a tool, and refreshing plugins with `/reload-plugins`.
- **Corrections / additions I made:**
  - The tool in your main session was Agent (orchestration); the web calls happened inside the subagent.
  - What came back was the agent's report, not the raw page. WebFetch takes a `prompt` and returns an answer about the page.
  - The subagent's tool names live in its transcript file; I extracted them with a script. Whether the terminal UI shows them (`/agents`, background tasks, Ctrl+O) is unverified.
  - Execution is `Bash` (shell commands). Editing files is file operations (`Read`, `Edit`, `Write`), not execution. Search tools are `Glob` (names) and `Grep` (content).
  - Agentic: tools let Claude see the result of its actions and react, which is the gather → act → verify loop. You saw it when a failing test made Claude fix and re-run.
  - Code intelligence: the main requirement was the language server installed separately, then the plugin. Reload (or a fresh session) comes after. The built-in tools need no setup.
  - Claude's first type-error answer gave no sign that the plugin lacked its server; it just fell back to shell commands. A tool that isn't working can look like "no tool" rather than an error. Check `/plugin` → Errors.
- **Not done / open (with my expectation):**
  - Does an LSP diagnostics line also appear after hover-only lookups, as it did here? *Expectation (docs, code-intelligence):* docs describe diagnostics after edits only; this run suggests otherwise. Unconfirmed, don't rely on it.
  - A web tool called directly in the main session, with its permission prompt, was skipped by choice. *Expectation:* a direct WebFetch asks for permission the first time.
  - Where the terminal UI shows a subagent's tool calls. *Expectation:* `/agents` or the transcript view (Ctrl+O). Check.
  - The `delete` work in `sandbox/` is still uncommitted. *Expectation:* Claude won't commit unless asked.
- [x] Done
