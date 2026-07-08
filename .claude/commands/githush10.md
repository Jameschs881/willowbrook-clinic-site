---
description: Push code, deploy GitHub Pages via Actions, refresh README, update repo About, and run a secrets scan
---

Run the full publish workflow for this project, in this order. Do not skip steps. Ask the user before any destructive or irreversible action (force-push, overwriting unrelated history, deleting files). If a step fails, stop and report — do not silently continue to the next step.

## 1. Security scan first — before anything is pushed

Before staging or committing anything new, scan the working tree (including any newly added/modified files not yet committed) for sensitive data that should never reach a public repo:

- API keys, tokens, secrets (AWS `AKIA...`, generic `sk-...`, `ghp_...`, `xox...`, JWTs, private keys `-----BEGIN ... PRIVATE KEY-----`)
- `.env` files, credentials, passwords, connection strings with embedded credentials
- Personal data that doesn't belong in a public demo (real emails/phone numbers/addresses instead of placeholder/example ones)
- Files that look like local machine artifacts (absolute paths, `.vscode` personal settings, OS junk files like `.DS_Store`, `Thumbs.db`)

Use `git status`, `git diff`, and grep across changed files for common secret patterns. If anything suspicious is found:
- Do NOT commit or push it.
- Report exactly what was found and where.
- Ask the user how to proceed (redact, move to `.env` + add to `.gitignore`, or confirm it's a safe placeholder).

Only proceed to the next steps once the scan is clean or the user has explicitly confirmed the flagged content is safe to publish.

## 2. Push / update code to GitHub

- Run `git status` to see what's changed.
- Stage and commit with a clear, descriptive commit message summarizing the actual changes (not a generic "update").
- Push to the current branch's remote (`origin`). If no remote/repo exists yet, create one with `gh repo create` (ask the user for name/visibility if not already established in this conversation).

## 3. Create/update GitHub Pages via GitHub Actions

- Check for an existing Pages workflow at `.github/workflows/*.yml`. If one already deploys to Pages, verify it still matches the site's structure (e.g. static root, correct build steps if a framework was added) and update it if the project has changed (e.g. added a build step, changed output directory).
- If none exists, create one using `actions/checkout`, `actions/configure-pages`, `actions/upload-pages-artifact`, and `actions/deploy-pages`, triggered on push to the main branch.
- Ensure the repo's Pages source is set to "GitHub Actions" (via `gh api -X POST repos/{owner}/{repo}/pages -f build_type=workflow`, or PATCH if it already exists with a different source).
- After pushing, watch the workflow run (`gh run watch`) and confirm it succeeds. Report the live Pages URL.

## 4. Create/update a professional README

- Read the current `README.md` if one exists; otherwise create one.
- It should reflect what the project *actually* is (check the code — don't assume). Include: a short description, live site link, key features, project structure, local dev/run instructions, and deployment notes (mention the GitHub Actions Pages workflow).
- Keep it accurate and free of placeholder text that doesn't apply to this project.
- Commit and push the README update as part of (or immediately after) step 2's push, so it's live before step 5.

## 5. Update the GitHub repo "About" section with the live site link

- Use `gh repo edit` to set the repo description and homepage URL to the live GitHub Pages URL (e.g. `gh repo edit OWNER/REPO --homepage "https://OWNER.github.io/REPO/" --description "..."`).
- Pull the description from the README's opening summary so it stays consistent.

## Finish

Report a short summary: what was committed/pushed, the live Pages URL, whether the About section was updated, and the outcome of the security scan (clean, or what was flagged and how it was resolved).
