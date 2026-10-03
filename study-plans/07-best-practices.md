# 07: Best Practices

PDF pages 29–34. Progress: 0/16

## Give cc a way to verify its work
**Concept:** Provide verification criteria (tests), verify UI visually (screenshots), and fix root causes rather than suppressing symptoms.
**Hands-on:** Prompt "implement validateEmail" with no criteria, then redo it with test cases and "run the tests". Plant a failing build error and ask for a root-cause fix (not suppression).
- [ ] Done

## Provide specific context in your prompts
**Concept:** Scope the task, point to sources, reference existing patterns, describe symptoms. Vague prompts are OK for exploration.
**Hands-on:** Rewrite 3 vague prompts of your own using the before/after table. Test one of each.
- [ ] Done

## Provide rich content
**Concept:** `@` references, pasted images, URLs (allowlist domains with `/permissions`), piped data. Let Claude fetch what it needs.
**Hands-on:** Paste a screenshot of an error, give a docs URL, and pipe in a log in the same session.
- [ ] Done

## Let Claude interview you
**Concept:** For larger features, have Claude interview you (via the AskUserQuestion tool) and write `SPEC.md`. Then execute in a fresh session.
**Hands-on:** Use the notes' interview prompt for a new todo-app feature (e.g. tags). Produce `SPEC.md`, then start a clean session to implement it.
- [ ] Done

## Manage your Session
### Course-correct early and often
**Concept:** Esc stops mid-action (context kept). Esc Esc / `/rewind` restores. If you've corrected the same issue more than twice, `/clear` and write a better prompt.
**Hands-on:** Deliberately let Claude go wrong, correct it twice, then `/clear` and reprompt with what you learned. Compare.
- [ ] Done

## Manage context aggressively!
**Concept:** Auto-compaction at limits. Partial compaction through "Summarize from here". Customise compaction in CLAUDE.md.
**Hands-on:** Run `/compact`, then use `/rewind` → "Summarize from here". Add a "when compacting, always preserve…" line to CLAUDE.md.
- [ ] Done

### Use subagents for investigation
**Concept:** Delegate research so the exploration stays out of the main context.
**Hands-on:** Same investigation with and without a subagent, then compare `/context`.
- [ ] Done

### Rewind with checkpoints
**Concept:** Every action creates a checkpoint. The rewind menu offers conversation only / code only / both / summarize. Checkpoints persist across sessions. Rewinding makes risky experiments cheap.
**Hands-on:** Try something risky, rewind, and reopen the terminal and rewind again later.
- [ ] Done

### Resume conversations
**Concept:** `--continue`, `--resume`, `/rename`. Treat sessions like branches.
**Hands-on:** Review (quick recap from file 05) and teach it back to me.
- [ ] Done

## Automate and scale
### Run non-interactive mode
**Concept:** `claude -p` for CI, pre-commit hooks and scripts, with `--output-format json` or `stream-json`.
**Hands-on:** Write a small script that calls `claude -p "List all functions in todo.py" --output-format json` and parses the result.
- [ ] Done

### Run multiple Claude sessions
**Concept:** Desktop app, Claude Code on the web, agent teams. Writer/Reviewer pattern: a fresh context reviews better.
**Hands-on:** Session A implements a feature; session B (fresh) reviews it; paste B's feedback back to A.
- [ ] Done

### Fan out across files
**Concept:** Generate a task list, loop `claude -p` per item with `--allowedTools`, test on 2–3 files, then scale. Use `--verbose` only in development.
**Hands-on:** Create 5 small Python files, loop to add docstrings with `--allowedTools "Edit"`. Test on 2 first. (Do it in the sandbox only.)
- [ ] Done

## Safe autonomous mode
**Concept:** `--dangerously-skip-permissions` bypasses all checks (risk: data loss, prompt injection). Safer alternatives: a container with no internet, or `/sandbox`.
**Hands-on:** Enable `/sandbox` and run an unattended task. Discuss, but don't run, the flag outside a disposable environment.
- [ ] Done

## Avoid common failure patterns
**Concept:** Kitchen-sink session (`/clear` between tasks), repeated correcting (clear and reprompt), over-specified CLAUDE.md (prune or convert to hooks), trust-then-verify gap (always verify), infinite exploration (scope it or use subagents).
**Hands-on capstone:** I describe 5 scenarios. You diagnose which failure pattern it is and name the fix. Then audit your own sandbox setup against the list.
- [ ] Done
