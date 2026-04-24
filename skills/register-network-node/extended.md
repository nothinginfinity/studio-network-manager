---
skill: register-network-node
version: 1.0.0
extends: skills/register-network-node.md
tier: focused
disclosure: extended
---

## register-network-node — Extended

This file is loaded **only when the base skill encounters a condition outside its happy path.** Do not load this file on a normal invocation.

---

### NETWORK.md does not exist yet

Create it from scratch before appending the new node entry.

**Full file template:**

```markdown
# Studio-OS Network Registry

This file is the canonical registry of all active studio-os Space nodes.
It is maintained by the studio-network-manager PWA.
Do not edit manually — changes should be made via the register-network-node skill.

---

## Active Nodes

| Space Name | Repo | Inbox | Outbox | Description |
|---|---|---|---|---|
```

After creating the file with this header, append the new node row and commit both as a single operation with message:
`feat(network): init NETWORK.md and register <space_name> node`

---

### space_name already exists in NETWORK.md

Search NETWORK.md for an existing row where the Space Name cell matches `space_name` exactly.

| Condition | Action |
|---|---|
| Row found, repo matches `repo_full_name` | Treat as idempotent re-registration. Return existing `network_entry` and freshly generated `perplexity_instructions`. No new commit. |
| Row found, repo differs | Halt. Return conflict error: `"Space '<space_name>' is already registered to a different repo: <existing_repo>. Manual resolution required."` |

---

### NETWORK.md exists but has no table yet

If the file exists but does not contain an Active Nodes table header (`| Space Name |`):

1. Append the full table header block to the end of the existing file
2. Then append the new node row
3. Commit with message: `feat(network): add nodes table and register <space_name>`

Do not alter any existing content above the appended block.

---

### Malformed NETWORK.md (unparseable table)

If NETWORK.md exists, contains a table header, but the table rows cannot be parsed reliably:

1. Halt. Do not attempt to append.
2. Return error: `"NETWORK.md table is malformed and cannot be safely modified. Manual inspection required."`
3. Include the raw file content in the error response so the caller can inspect it.

---

### Generating perplexity_instructions for non-standard inbox/outbox paths

The base skill assumes paths follow the `spaces/<space_name>/inbox.md` convention. If `inbox_path` or `outbox_path` deviate from this pattern (e.g. a legacy node with a custom layout):

1. Use the provided paths verbatim — do not normalize them
2. Add a comment line to the generated instructions block:
   `# Note: non-standard mailbox paths — verify before pasting into Perplexity Space`

---

### Commit failure on NETWORK.md update

NETWORK.md updates are read-modify-write operations and are vulnerable to race conditions if multiple nodes are registered simultaneously.

If the GitHub API returns a `409 Conflict` (SHA mismatch):

1. Re-fetch the current NETWORK.md and its SHA
2. Re-apply the append
3. Retry the commit exactly once
4. If the second attempt also fails, halt and return: `"NETWORK.md update failed after retry — possible concurrent write. Try again."`
