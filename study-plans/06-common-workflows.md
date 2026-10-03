# 06: Common Workflows

PDF pages 27–29. Progress: 0/4

## Working with Tests
**Concept:** Be specific about the behaviour to verify. Claude matches your existing test style, and can look for edge cases you missed.
**Hands-on:** Ask "write tests for `done` covering an invalid ID and an already-done task", then ask "what edge cases have I missed?". Run the tests.
**Discuss:** How did Claude know which framework and style to use?
- [ ] Done

## Create pull requests
**Concept:** Ask directly, or use `/commit-push-pr`. PRs made via `gh pr create` link to the session, so you can resume with `claude --from-pr <n>`.
**Hands-on:** Push the sandbox to a throwaway GitHub repo (only with your OK; private repo recommended), create a PR via Claude, then resume with `--from-pr`. If you'd rather not use GitHub, we go through it as a walkthrough and use `claude commit` locally.
- [ ] Done

## Reference files and directories
**Concept:** `@file` includes full content, `@dir` includes a listing only, and `@server:resource` pulls MCP resources. It also adds CLAUDE.md files from that file's directory and parents. You can reference several in one message.
**Hands-on:** `Explain @todo.py`, `What's the structure of @tests`, and one with two files. Check `/context` for what each added. Try an MCP resource if you have a server.
- [ ] Done

## Pipe in, pipe out
**Concept:** `cat x | claude -p '...' > out.txt`. Output formats: `text` (default), `json` (messages + cost/duration), `stream-json` (real-time lines).
**Hands-on:** Pipe a made-up `build-error.txt` into `claude -p`. Then do the same with `--output-format json` and inspect the cost field. Try `stream-json` too.
**Discuss:** When would each format be right?
- [ ] Done
