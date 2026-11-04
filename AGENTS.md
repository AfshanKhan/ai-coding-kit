# Agent rules (Frappe app template)

Copy this file to the root of a Frappe app repo and edit the Frappe section to match your app. Follow these rules for every change. Small tasks: use judgment, do not over-process.

## 1. Think before coding
- State assumptions. If the request is ambiguous, ask instead of guessing.
- Read the whole flow first: JS caller, whitelisted method, controller, hooks.
- Search the repo for an existing helper, util, DocType field or setting before adding one (`grep`, `utils.py`, sibling modules). Verify every name you use exists.
- New field, setting, DocType, hook or endpoint: ask first. (A task that names the endpoint to add counts as asked.)
- New whitelisted method: copy the permission check from the nearest existing whitelisted method in the same package. State any difference.

## 2. Simplicity
- Write the minimum code that solves the task. No speculative features, flags or abstractions.
- No new dependency without approval.
- Prefer adding to an existing module over a new file. New file only when no module fits.
- Match the style of surrounding code, including comment density.

## 3. Surgical changes
- Touch only what the task needs. Every changed line must trace to the request.
- No drive-by renames, reformatting or refactors. Mention unrelated dead code instead of deleting it.
- Delete dead code that your own change creates.
- Before changing a public function, grep all callers.

## 4. Root cause
- Fix where the bad value is produced, not where the error shows. No blanket `try/except` or guards that hide it.
- Fix sibling copies of the same bug (JS and Python, form and list).

## 5. Frappe rules
- No `ignore_permissions=True` to fix a workflow.
- No `frappe.db.commit()` inside document events.
- Server owns validation and business logic. Check permissions in whitelisted methods.
- ORM or query builder before raw SQL. No queries in loops.
- Wrap user-facing strings in `_()` / `__()`.
- Edit DocType JSON through the Desk UI, not by hand.
- Data fix for existing sites: add a patch and list it in `patches.txt`.

## 6. Verify
- Define the success check first. Bug fix: write a failing test, then make it pass.
- New endpoint or function with logic: add a test, mocking external calls. If you add none, say why in the final report.
- Never weaken or delete a test to make it pass.
- Run `pre-commit run --files <changed files>` and the relevant tests before calling the task done.
- Never claim something was run or tested if it was not. Write "not run" and say why.

## 7. Pull requests
- One concern per PR, against the right base branch.
- Description: one line on the user-visible problem, one-sentence bullets for what changed and how it was tested, or "not run".
- State that AI assistance was used. A human must read and understand every changed line.
- Conventional Commit titles. Never put customer data or secrets in code, commits or prompts.
