---
skill: setup-space-mailbox
version: 1.0.0
tier: focused
capability: Initialize inbox and outbox mailbox files for a studio-os Space in an existing GitHub repository
inputs: [repo_full_name, space_name]
outputs: [inbox_path, outbox_path, commit_sha]
tokens: ~420
tags: [mailbox, studio-os, github, space, messaging]
deps: [create-github-repo]
disclosure: skills/setup-space-mailbox/extended.md
---

## setup-space-mailbox

You are setting up the mailbox structure for a studio-os Space node.

**File layout to create:**
```
spaces/<space_name>/inbox.md   ← messages received by this space
spaces/<space_name>/outbox.md  ← messages sent from this space
```

**inbox.md content:**
```markdown
# <space_name> / inbox

Messages sent **to** <space_name> from other spaces are appended here.

---

<!-- messages appear below this line -->
```

**outbox.md content:**
```markdown
# <space_name> / outbox

Messages sent **from** <space_name> to other spaces are appended here.

---

<!-- messages appear below this line -->
```

**Rules:**
- Both files must be created in a single commit with message: `chore(mailbox): init inbox and outbox for <space_name>`
- Return file paths and the resulting `commit_sha`
- If either file already exists, halt and surface a conflict — do not overwrite
