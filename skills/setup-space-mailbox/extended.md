---
skill: setup-space-mailbox
version: 1.0.0
extends: skills/setup-space-mailbox.md
tier: focused
disclosure: extended
---

## setup-space-mailbox — Extended

This file is loaded **only when the base skill encounters a condition outside its happy path.** Do not load this file on a normal invocation.

---

### Conflict: inbox.md or outbox.md already exists

If either file exists at the target path, do **not** overwrite it. Instead:

1. Read the existing file contents
2. Check whether it contains the standard mailbox header (`<!-- messages appear below this line -->`)
3. Branch:

| Condition | Action |
|---|---|
| Header present, file is otherwise empty | Safe to treat as already initialized — return existing paths and current HEAD sha as `commit_sha`. No new commit needed. |
| Header present, file has messages | Halt. Return a conflict error: `"Mailbox already active for <space_name>. Manual inspection required."` |
| Header absent | Halt. Return a conflict error: `"File exists at <path> but is not a recognized mailbox format. Do not overwrite."` |

Never silently overwrite. Always surface the conflict to the caller.

---

### Partial state: one file exists, the other does not

This indicates a previous run failed mid-commit. Treat as a conflict regardless of file contents:

1. Return error: `"Partial mailbox state detected for <space_name>: <existing_path> exists but <missing_path> does not."`
2. Do not attempt to create the missing file in isolation
3. Recommend the caller deletes the existing file and re-runs the base skill from scratch

---

### Repo does not have a `spaces/` directory yet

This is fine — the GitHub API creates intermediate directories when files are committed. No special handling needed. Proceed with the normal commit.

---

### space_name contains invalid characters

Space names must be lowercase, hyphenated, and contain only `[a-z0-9-]`. If `space_name` fails this check:

1. Do not proceed with any file creation
2. Return error: `"Invalid space_name '<space_name>': must be lowercase hyphenated (a-z, 0-9, hyphens only)"`
3. Suggest a corrected form if one is obvious (e.g. `My Space` → `my-space`)

---

### Commit failure (GitHub API error)

If the push_files / create_or_update_file call returns a non-2xx response:

1. Do not retry automatically
2. Return the raw API error message to the caller
3. Confirm that no partial state was written before surfacing the error
