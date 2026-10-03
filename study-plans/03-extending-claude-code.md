# 03: Extending Claude Code

PDF pages 7–21. This is the biggest section, so we'll likely split it over several sittings. Progress: 0/21

## Extend Claude Code
**Concept:** Extensions plug into different parts of the agentic loop. CLAUDE.md = persistent context; Skills = reusable knowledge/workflows; MCP = external services; Subagents = isolated loops; Agent teams = coordinated sessions; Hooks = deterministic scripts outside the loop; Plugins = packaging.
**Hands-on:** Without notes, draw/write a one-line summary of each of the 7 extensions. We compare against the PDF.
- [ ] Done

## Match features to your goal
**Concept:** The table of "what / when / example" for each feature.
**Hands-on:** I give you 6 scenarios (e.g. "always use pytest", "query a database", "run lint after every edit"). You pick the right feature for each and justify it.
- [ ] Done

## CLAUDE.md
**Concept:** `/init` generates a starter. Files are discovered by walking up the tree, and subdirectory files are also found. Use it for every-session rules; use skills for occasional knowledge.
**Hands-on:** Run `/init` in the sandbox. Create `sandbox/sub/CLAUDE.md` with a different rule. Launch from `sandbox/sub/` and check with `/memory` which files loaded.
- [ ] Done

### Load from additional directories
**Concept:** `--add-dir` gives access but doesn't load CLAUDE.md unless `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1`.
**Hands-on:** Make `../shared-config/CLAUDE.md` with a distinctive rule. Launch with and without the env var and check whether Claude follows the rule.
- [ ] Done

### Organize rules with .claude/rules/
**Concept:** Modular rule files. Without `paths` frontmatter they load at launch. With `paths` globs they load only when matching files are read. Symlinks are supported, and user-level rules live in `~/.claude/rules/`.
**Hands-on:** Create `.claude/rules/testing.md` (unconditional) and `.claude/rules/py.md` with `paths: ["**/*.py"]`. Use `/memory` to see when each loads. Try a brace-expansion glob.
**Discuss:** Rules vs skills: when each?
- [ ] Done

### Write effective instructions
**Concept:** Under ~200 lines, structured with headers and bullets, no contradictions. Include/exclude table. Prune regularly. Emphasis ("IMPORTANT") can help.
**Hands-on:** Write a bloated CLAUDE.md with contradictions and a rule Claude already follows. Observe misbehaviour. Then prune it with the include/exclude table and re-test.
- [ ] Done

### Import additional files
**Concept:** `@path` imports, relative to the importing file, recursive up to 5 hops. `CLAUDE.local.md` is for private, git-ignored prefs. A home-directory import shares prefs across worktrees.
**Hands-on:** Import `@README.md` and `@~/.claude/my-project-instructions.md`. Accept the approval dialog and verify with `/memory`.
- [ ] Done

### View and edit with /memory
**Concept:** Lists loaded CLAUDE.md and rules files, toggles auto memory, and opens the memory folder.
**Hands-on:** Open `/memory`, open a file, toggle auto memory off and back on.
- [ ] Done

### Manage CLAUDE.md for large teams
**Concept:** A managed-policy CLAUDE.md can't be excluded. `claudeMdExcludes` skips files by glob (any settings layer; arrays merge).
**Hands-on (read-only for the managed file):** Locate the managed path for Windows (`C:\Program Files\ClaudeCode\CLAUDE.md`) without creating it. Add a `claudeMdExcludes` entry in `.claude/settings.local.json` and confirm a file disappears from `/memory`.
- [ ] Done

### Claude.md Locations
**Concept:** `~/.claude/CLAUDE.md` (all sessions), project root (shared, or `.local` ignored), parent directories (monorepos).
**Hands-on:** Put a distinct rule in each of the three locations and confirm all load, then see which wins when they conflict.
- [ ] Done

## Auto memory
**Concept:** Claude writes its own notes. It's per working tree, and the first 200 lines of `MEMORY.md` load each session. CLAUDE.md = you write it; auto memory = Claude writes it. It can be turned off via `/memory`, `autoMemoryEnabled`, or `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1`.
**Hands-on:** Correct Claude on a preference twice ("always use argparse subcommands"). In a new session, see if it applies it. Then inspect `~/.claude/projects/<project>/memory/`.
- [ ] Done

### Storage Location
**Concept:** `~/.claude/projects/<project>/memory/`, derived from the git repo (shared across worktrees), machine-local, with `MEMORY.md` as the index plus topic files.
**Hands-on:** Find the sandbox's memory directory and read `MEMORY.md`. Note what's an index and what's a topic file.
- [ ] Done

### How it works
**Concept:** The first 200 lines of `MEMORY.md` load at session start. Claude decides what's worth saving. Subagents can have their own `memory:` field.
**Hands-on:** Create a subagent with `memory: user`, run it twice, and see what it stored.
- [ ] Done

## Configure permissions
**Concept:** `/permissions` allowlists safe commands. `/sandbox` gives OS-level isolation so Claude can work more freely inside boundaries.
**Hands-on:** Allowlist `python -m pytest`, run tests, and confirm no prompt. Try `/sandbox`.
- [ ] Done

## CLI Tools
**Concept:** CLI tools (`gh`, `aws`, `gcloud`) are the most context-efficient way to talk to external services. Claude can learn unfamiliar CLIs via `--help`.
**Hands-on:** Have Claude use `git` and `gh` (if installed) to list recent commits/issues. Ask it to learn a CLI via `--help` and use it.
- [ ] Done

## Skills
**Concept:** Markdown files in `.claude/skills/<name>/SKILL.md`. Reference vs action skills. Frontmatter (`name`, `description`, `disable-model-invocation`). `$ARGUMENTS`. Descriptions are always loaded, full content only on use.
**Hands-on:** Create an `api-conventions`-style reference skill and a `/fix-issue`-style action skill with `disable-model-invocation: true` for the todo app. Invoke the second as `/fix-todo 1`. Check `/context` to compare costs.
- [ ] Done

## MCP
**Concept:** `claude mcp add` connects external tools. MCP servers load all tool definitions every request.
**Hands-on:** Add a simple MCP server (we'll pick one together, e.g. filesystem), run `/mcp`, use a tool, then check `/context` cost and remove it.
- [ ] Done

## Subagents
**Concept:** Isolated context; get the system prompt, listed skills, CLAUDE.md and git status plus whatever the lead passes. Frontmatter: `tools`, `model`, `skills`, `context: fork`. Use for isolation and when context fills up.
**Hands-on:** Ask "use a subagent to review todo.py for bugs" and observe that only a summary returns. Compare `/context`.
- [ ] Done

### Use specialized subagents
**Concept:** `/agents` to view and create. Automatic vs explicit delegation. Project agents in `.claude/agents/`. Limit tools. Write descriptive `description`s.
**Hands-on:** Create `security-reviewer` (tools: Read, Grep, Glob). Trigger it automatically and explicitly. Try to make it edit a file and see the limit.
- [ ] Done

## Agent teams
**Concept:** Experimental and off by default (`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS`). For teammates that share findings, challenge each other and coordinate. It has known limitations.
**Hands-on (optional):** Enable the flag in the sandbox and run a 2-teammate "competing hypotheses" debug. Otherwise a discussion: when subagents vs teams?
- [ ] Done

## Hooks
**Concept:** Deterministic scripts on lifecycle events. They cost zero context unless output is returned. CLAUDE.md is advisory; hooks are guaranteed.
**Hands-on:** Ask Claude to write a hook that runs `python -m pytest` (or a linter) after every edit, and another that blocks edits to a `migrations/` folder. Verify both fire. Explore `/hooks`.
- [ ] Done

### Set up hooks
**Concept:** `/hooks` for interactive config, or edit `.claude/settings.json`. Claude can write hooks for you.
**Hands-on:** Locate your hooks in `settings.json` and edit one by hand (change its matcher).
- [ ] Done

## Plugins
**Concept:** Bundles of skills, hooks, subagents and MCP servers, namespaced (`/my-plugin:review`), distributed through marketplaces. Code-intelligence plugins improve typed-language work.
**Hands-on:** Run `/plugin`, browse the marketplace and read one plugin's contents. (Install only if useful.)
- [ ] Done

## Understand how features layer
**Concept:** CLAUDE.md = additive. Skills/subagents override by name (priority orders). MCP overrides by name (local > project > user). Hooks merge.
**Hands-on:** Create a same-named skill at user and project level and see which wins. Add hooks at two levels and see both fire.
- [ ] Done

## Understand context costs
**Concept:** Every feature uses context, and too much adds noise as well as cost.
**Hands-on:** Run `/context` on a fresh session, then add a skill, an MCP server and a big CLAUDE.md in turn and watch the numbers.
- [ ] Done

## Context cost by feature
**Concept:** The table: CLAUDE.md (every request), Skills (descriptions only until used), MCP (all schemas), Subagents (isolated), Hooks (zero).
**Hands-on:** Fill in a blank copy of the table from memory, then check it.
- [ ] Done

## Understand how features load
**Concept:** When each feature loads and what goes in. Session start vs on trigger vs on demand.
**Hands-on:** Sequence exercise: place each of the 5 features on a session timeline (start → prompt → tool use → subagent spawn).
- [ ] Done

## Combine features
**Concept:** Skill+MCP, Skill+Subagent, CLAUDE.md+Skills, Hook+MCP patterns.
**Hands-on capstone:** Build one mini setup in the sandbox that combines at least three features (e.g. CLAUDE.md rule + `/review` skill spawning a subagent + lint hook).
- [ ] Done
