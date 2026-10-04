# Notes: setting up code review for Claude Code

Researched 2026-10-04 by the claude-code-guide agent from the official docs (code.claude.com/docs). I haven't re-checked the pages myself, so treat details such as prices, version numbers and plan limits as "to verify before relying on them". Not part of the PDF; kept as a side reference.

## Options at a glance

| Option | Where it runs | Setup | Cost / availability |
|---|---|---|---|
| `/code-review` (local) | In your session, as a background subagent | None, built in | Normal usage |
| `/code-review ultra`, `claude ultrareview` | Anthropic cloud sandbox | Log in with a claude.ai account | 3 free runs on Pro/Max, then about $5–25 per review. Not available with API-key-only, Bedrock, Vertex, Foundry or Zero Data Retention |
| Managed Code Review | Anthropic infrastructure, on every PR | Org admin: `claude.ai/admin-settings/claude-code` → Code Review → Setup → install the Claude GitHub App → pick repos → pick trigger | Team or Enterprise plan. About $15–25 per review, billed on usage credits, with a monthly spend cap |
| GitHub Actions | Your own GitHub runners | `/install-github-app` (needs authenticated `gh`) or copy the workflow by hand, plus an API key or OAuth token secret | Your API usage |

Docs: `/docs/en/code-review.md`, `/docs/en/github-actions.md`, `/docs/en/ultrareview.md`, `/docs/en/memory.md`.

## Local `/code-review`
- Syntax: `/code-review [target] [low|medium|high|max] [--fix] [--comment] [--post] [--max-findings n|all|default]`.
- Target: file path, PR number, branch, or range such as `main...my-feature`. The default is the current branch's commits ahead of upstream plus uncommitted changes.
- Low/medium: fewer, high-confidence findings. High/max: broader coverage, may include uncertain ones.
- `--fix` applies findings to the working tree. Those edits are outside session checkpoints, so `/rewind` can't undo them.
- `--comment` posts inline comments on a GitHub PR or GitLab MR.
- Follows `CLAUDE.md`. Ignores `REVIEW.md`.
- Examples: `/code-review high --fix`, `/code-review 123 --comment`, `/code-review main..HEAD --max-findings 5`.

## Ultrareview (cloud)
- `/code-review ultra [branch|PR#] [--fix|--comment|--post]`.
- Script/CI form: `claude ultrareview [PR#] [--json] [--post] [--timeout minutes]`. Exit codes: 0 success, 1 failed, 2 partial, 130 interrupted.
- Takes about 5–10 minutes, multi-agent with a verification step.

## Managed Code Review (Team/Enterprise)
- Review behaviour per repo: once after PR creation, after every push (most expensive), or manual only.
- Manual triggers (top-level PR comment, needs write access): `@claude review`, `@claude review once`, `@claude review always`.
- Findings are labelled Important, Nit or Pre-existing, posted inline, with a summary in the check run. The check run is neutral, so it never blocks merging.
- Analytics: `claude.ai/analytics/code-review`.

## Customizing PR reviews
- `CLAUDE.md`: rule violations show up as nit-level findings. Subdirectory rules apply under their path.
- `REVIEW.md` at the repo root (read only by the PR reviewers): redefine what "Important" means, cap nits, skip paths such as generated files and lockfiles, add repo-specific checks, set the verification bar, shape the summary. Keep it short, because length dilutes the priority rules.

## GitHub Actions
- Quick: run `/install-github-app` in the repo. It installs the app, stores the secret, and opens a PR with `.github/workflows/claude.yml`.
- Manual: install the app (github.com/apps/claude), add `ANTHROPIC_API_KEY` or `CLAUDE_CODE_OAUTH_TOKEN` (from `claude setup-token`) as a repo secret, then copy the example workflow from `anthropics/claude-code-action`.
- Automated review workflow, sketched from the docs:
  ```yaml
  on:
    pull_request:
      types: [opened, synchronize, ready_for_review, reopened]
  jobs:
    review:
      runs-on: ubuntu-latest
      permissions: { contents: read, pull-requests: read, issues: read, id-token: write }
      steps:
        - uses: actions/checkout@v6
          with: { fetch-depth: 1 }
        - uses: anthropics/claude-code-action@v1
          with:
            anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
            plugin_marketplaces: "https://github.com/anthropics/claude-code.git"
            plugins: "code-review@claude-code-plugins"
            prompt: "/code-review:code-review --comment ${{ github.repository }}/pull/${{ github.event.pull_request.number }}"
            claude_args: '--allowedTools "mcp__github_inline_comment__create_inline_comment"'
  ```
- Interactive mode: a workflow on `issue_comment` and `pull_request_review_comment` that responds to `@claude` (omit `prompt`).
- Cost controls: specific requests, `--max-turns`, workflow timeouts, concurrency limits.
- Troubleshooting: if CI doesn't run on Claude's commits, GitHub doesn't trigger workflows from `GITHUB_TOKEN`, so use a custom app token.

## Security review
- `/security-review`: on-demand security pass on the current branch. Only briefly covered in the docs the agent read.
- `security-guidance` plugin (`/plugin install security-guidance@claude-plugins-official`): automatic in-session checks (per-edit patterns, end-of-turn diff review, commit/push review). Switchable with env vars such as `ENABLE_STOP_REVIEW=0`.

## Open items (with my expectation)
- Exact thresholds behind the effort levels. *Expectation:* not published; test by comparing `low` and `max` on the same diff in `sandbox/`.
- Differences between Team and Enterprise for managed Code Review. *Expectation:* the docs only say "Team or Enterprise", so check the admin settings page if it ever matters.
- Whether `/security-review` is a built-in command or a skill in this setup. *Expectation:* it is available (it's listed in this session). Try it on `sandbox/` changes.
- GitHub Actions and managed setup both push to GitHub and spend credits. *Expectation:* only try them in `sandbox/`, after confirming (see CLAUDE.md risky-feature rule).
