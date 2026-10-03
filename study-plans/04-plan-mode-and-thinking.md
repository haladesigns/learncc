# 04: Plan Mode and Thinking

PDF pages 22–24. Progress: 0/7

## Plan Mode
**Concept:** A read-only permission mode. Claude reads and reasons but doesn't change anything. Switch with Shift+Tab to cycle modes.
**Hands-on:** Press Shift+Tab until you see plan mode. Ask for a change and confirm nothing is edited.
- [ ] Done

### When to use Plan Mode
**Concept:** Multi-step or multi-file work, code exploration, and interactive direction-setting. Also `claude --permission-mode plan`, and headless with `-p`. Ctrl+Alt+G edits the plan in your editor.
**Hands-on:** Run `claude --permission-mode plan -p "Analyze todo.py and suggest improvements"`. Then start an interactive plan-mode session and open the plan with Ctrl+Alt+G.
**Discuss:** When is plan mode overhead not worth it? ("If you could describe the diff in one sentence, skip the plan.")
- [ ] Done

### Explore first, then plan, then code
**Concept:** The 4-step flow: Explore → Plan → Implement (normal mode) → Commit.
**Hands-on:** Add a "due dates" feature to the todo app using the full flow, staying in the right mode at each step, ending with a commit.
- [ ] Done

### Configure Plan Mode as default
**Concept:** `permissions.defaultMode: "plan"` in `.claude/settings.json`.
**Hands-on:** Set it, start a session and verify the mode. Then remove it.
**Discuss:** Is it a good default for you? Why or why not?
- [ ] Done

## Extended thinking
**Concept:** Controls how much internal reasoning Claude does before answering. Some models always think and can't disable it. Manual `budget_tokens` is unsupported or deprecated on newer models.
**Hands-on:** Give a hard task (find a subtle bug you plant) and compare it with a trivial one. Note the difference in visible thinking.
**Discuss:** Which models in the notes force thinking on, and which reject `budget_tokens`?
- [ ] Done

## Adaptive thinking
**Concept:** Claude decides when and how much to think per request. It's recommended over fixed budgets, especially for long agentic tasks, and no beta header is needed.
**Hands-on:** Run a simple question and a complex one back to back and compare latency and depth.
- [ ] Done

### Adaptive thinking with the effort parameter
**Concept:** Soft guidance: max, xhigh, high (default), medium, low. Availability varies by model. API form: `thinking={"type": "adaptive"}, output_config={"effort": "medium"}`.
**Hands-on:** Run the notes' Python snippet (needs an API key; if you don't have one, we walk through it instead) with `low` vs `high` on the same question. Or use the in-app effort setting if available.
**Discuss:** What does "soft guidance" mean? When would you pick low vs max?
- [ ] Done
