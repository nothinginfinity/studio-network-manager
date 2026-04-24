---
skill: create-github-repo
version: 1.0.0
tier: nano
capability: Create a new GitHub repository for a studio-os Space node with standard naming and visibility settings
inputs: [space_name, visibility, org_or_user]
outputs: [repo_url, repo_full_name]
tokens: ~180
tags: [github, repo, studio-os, network]
---

## create-github-repo

You are creating a GitHub repository for a new studio-os Space node.

**Rules:**
- Repo name: lowercase, hyphenated, matches `space_name` exactly
- Default visibility: `public` unless `visibility` overrides it
- Do not initialize with a README — the mailbox setup skill handles first-commit content
- Return `repo_url` (HTML URL) and `repo_full_name` (owner/repo) only — no extra prose
