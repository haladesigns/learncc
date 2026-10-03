# 01: Getting Started

PDF pages 4–5. Progress: 0/5

## Start session
**Concept:** Claude Code runs in your terminal, scoped to the directory you launch it from.
**Hands-on:** `cd sandbox && claude`. Ask "what does this project do?" Then exit and relaunch from `learncc/` instead. Ask the same question.
**Discuss:** What differed between the two launches, and why does the launch directory matter?
- [ ] Done

## Essential commands
**Concept:** The core launch commands and in-session commands you'll use daily.
**Hands-on:** Run each in turn and note what it does:
`claude "list the functions in todo.py"`, `claude -p "explain todo.py"`, `claude -c`, `claude -r`, then in-session `/help`, `/clear`, and exit with `exit` or Ctrl+C. (Try `claude commit` after making a trivial edit.)
**Discuss:** When would you pick `-p` over interactive mode? What is the difference between `-c` and `-r`? What does `/clear` actually discard?
- [ ] Done

## Pro tips
**Concept:** The three habits from the notes: be specific, use step-by-step instructions, let Claude explore first. Plus keyboard shortcuts.
**Hands-on:**
1. Give a vague prompt ("fix the bug"), then a specific one on a bug you plant in `todo.py`. Compare results.
2. Give a 3-step list prompt (e.g. add a `priority` field → update `list` → update tests).
3. Ask Claude to "analyze the structure of todo.py" before asking for a change.
4. Try `?`, Tab completion, the up-arrow history, and typing `/` to list commands.
**Discuss:** What made the specific prompt better? When is a vague prompt actually fine?
- [ ] Done

## The agentic loop
**Concept:** Gather context → take action → verify, repeated and adaptive. Claude Code is the harness that provides tools, context management and execution.
**Hands-on:** Ask for a small feature ("add a `delete` command and a test"). Watch the output and label each step you see as *gather*, *act* or *verify*. Then ask a pure question and note that it only gathers.
**Discuss:** Which request needed which phases? What decides what happens next?
- [ ] Done

## Tools
**Concept:** Five categories (file operations, search, execution, web, code intelligence) plus orchestration tools such as subagents and asking questions.
**Hands-on:** Craft one prompt per category that triggers it (read/edit a file, grep for text, run tests, fetch a web page, ask about a type error). Note which tool names appear.
**Discuss:** Why does having tools make Claude "agentic"? What did code intelligence need that the others didn't?
- [ ] Done
