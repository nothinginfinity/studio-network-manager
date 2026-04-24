---
skill: register-network-node
version: 1.0.0
tier: focused
capability: Register a new studio-os Space node in the NETWORK.md registry and generate its Perplexity Space instructions
inputs: [space_name, repo_full_name, inbox_path, outbox_path, description]
outputs: [network_entry, perplexity_instructions]
tokens: ~510
tags: [registry, studio-os, network, perplexity, space]
deps: [create-github-repo, setup-space-mailbox]
disclosure: skills/register-network-node/extended.md
---

## register-network-node

You are registering a new node into the studio-os Space network.

### Step 1 — Append to NETWORK.md

Add a row to the registry table in `NETWORK.md` at the root of the managing repo:

| Field | Value |
|---|---|
| Space name | `<space_name>` |
| Repo | `<repo_full_name>` |
| Inbox | `<inbox_path>` |
| Outbox | `<outbox_path>` |
| Description | `<description>` |

Commit message: `feat(network): register <space_name> node`

### Step 2 — Generate Perplexity Space Instructions

Produce a `perplexity_instructions` string using this template:

```
My Space name is: <space_name>
My repo: <repo_full_name> (primary and only)
My outbox: <outbox_path> — I write here when sending messages
My inbox: <inbox_path> — I read here to receive messages
To reach studio-os: append to spaces/studio-os/inbox.md
```

**Rules:**
- Do not invent values — use only what was passed as inputs
- Return `network_entry` (the markdown table row) and `perplexity_instructions` (the block above, filled in)
- If NETWORK.md does not exist, create it with a header before appending
