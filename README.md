# AI coding kit: rules and workflow for Claude Code, Codex and similar tools

A starter kit for teams that let developers write code with AI tools. Written for Frappe apps, but most of it applies to any codebase.

| File | Use |
|---|---|
| `AGENTS.md` | Rules the AI tool follows. Copy to your repo root and edit the Frappe section. |
| `CLAUDE.md` | One line, `@AGENTS.md`, so Claude Code reads the same rules. |
| `.github/pull_request_template.md` | PR template with an AI-disclosure field. Copy to your repo's `.github/`. |
| This README | How developers should set up and work. |

You own every line you merge, whoever or whatever wrote it. These tools are fast juniors: useful, confident, sometimes wrong. This guide shows how to set up and get the benefit without lowering code quality.

## 1. Setup (5 minutes)

1. Install your tool: Claude Code, Codex, or another. Use the org account, not a personal one.
2. Clone the repo. Rules load automatically:
   - `AGENTS.md` holds the rules (Codex and most tools read it directly).
   - `CLAUDE.md` contains `@AGENTS.md`, so Claude Code reads the same file.
3. Install the lint hooks: `pre-commit install`.
4. Never put customer data, secrets, API keys or prod dumps into a prompt, a file the tool can read, or a commit.
5. Run the tool against a dev bench only. No prod credentials in your shell.

## 2. How to work

1. **Ask for a plan first** for anything non-trivial. Read it. Fix it before any code is written.
2. **Give a checkable goal**: "bug X no longer reproduces, with a failing test first", not "fix X".
3. **Small steps.** One concern per change. If the diff is too big to read, split it.
4. **Read the whole diff yourself.** If you cannot explain a line, do not merge it.
5. **Run the checks**: `pre-commit run --files <files>` and the relevant `bench run-tests`. The tool saying "tests pass" is not evidence. Run them.
6. **Open the PR** with the template. Say AI was used, what you verified by hand, and what you did not run.

## 3. What the rules enforce (see `AGENTS.md`)

- Search for an existing helper before writing a new one.
- Smallest change that works. No speculative features, no drive-by refactors.
- Fix the root cause and sibling copies, not just the symptom.
- Copy the permission pattern of neighbouring endpoints. No `ignore_permissions=True` shortcuts.
- Add a test for new logic. Never weaken a test to make it pass.
- Never claim something was run when it was not.

## 4. When not to use AI

- Auth, permissions, payments, data migrations or deletes: use it for drafts only and get a second reviewer.
- Code you cannot review. If you do not understand the area, learn it first.
- Anything involving customer data.

## 5. Optional add-ons

These change how the tool *talks* or *codes*. None replaces review. Review each plugin's source before installing: plugins can run hooks on your machine. The numbers below are the authors' own claims, not measured by us.

| Add-on | What it does | Our take |
|---|---|---|
| [caveman](https://github.com/JuliusBrussee/caveman) | Terse replies, fewer output tokens. Install: `claude plugin marketplace add JuliusBrussee/caveman` then `claude plugin install caveman@caveman`. | Safe personal preference. Saves chat tokens, does not improve code. It keeps commits and PR text in normal prose. |
| [ponytail](https://github.com/DietrichGebert/ponytail) | Pushes toward minimal code, reuse and stdlib. Needs Node on PATH. | Overlaps with our `AGENTS.md` simplicity rules. Optional. |
| [claude-token-efficient](https://github.com/drona23/claude-token-efficient) | A `CLAUDE.md` of terse-output rules. Author reports roughly 4 to 17 percent savings in his own tests. | It adds input tokens on every message, so it may cost more for small tasks. Skip unless token cost is a real problem. |

Do not stack all of them without a reason. They overlap, and each adds instructions the tool must follow.

Install commands change. Check each project's own README before installing.

## 6. Trial results (one repo, small sample)

We ran the same task with and without the rules, headless, two runs each, on one Frappe app:

- Without rules: tests added 0/2, `pre-commit` run 0/2, existing helper reused 1/2.
- With the tightened rules: tests 2/2, `pre-commit` 2/2, helper reused 2/2, permission pattern copied from neighbours 2/2.
- Honesty about untested work was good in every run.
- Rules improved process, not design judgement. Review still matters.

## 7. Changing the rules

If you keep correcting the tool for the same mistake, add one line to `AGENTS.md` in a PR with the reason. Keep the file short; every rule must be checkable.

## 8. Credits and sources

The rules are adapted from public projects. Read them for more:

- [frappe/frappe `AGENTS.md`](https://github.com/frappe/frappe/blob/develop/AGENTS.md) and `code_review.md`: reuse, root cause, permissions, tests.
- [vLLM `AGENTS.md`](https://github.com/vllm-project/vllm/blob/main/AGENTS.md): disclosure, human in the loop, honest test reporting.
- [Sentry `AGENTS.md`](https://github.com/getsentry/sentry/blob/master/AGENTS.md): one source of truth, layered files, skills.
- [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills): think first, simplicity, surgical changes, goal-driven. A community file inspired by Karpathy's posts, not by him.
- [Open-source AI contribution policies](https://github.com/melissawm/open-source-ai-contribution-policies): how many projects handle disclosure and responsibility.

## License

MIT. See `LICENSE`.
