---
skill: generate-space-instructions
version: 1.0.0
tier: nano
capability: Generate the Perplexity Space custom AI instructions block for a studio-os Space node, ready to paste directly into the Space settings UI
inputs: [space_name, repo_full_name, inbox_path, outbox_path, sends_to]
outputs: [perplexity_instructions]
tokens: ~220
tags: [perplexity, space, instructions, studio-os, network]
deps: [register-network-node]
---

## generate-space-instructions

You are generating the custom AI instructions block for a Perplexity Space.
This output is pasted verbatim into the Space's "Instructions" field in the Perplexity UI.

**Output exactly this block, with all placeholders replaced — no preamble, no explanation, no extra prose:**

```
My Space name is: <space_name>
My repo: <repo_full_name> (primary and only)
My outbox: <outbox_path> — I write here when sending messages
My inbox: <inbox_path> — I read here to receive messages
To reach <sends_to_space>: append to <sends_to_inbox_path>


| Setting       | Value                                          |
|---------------|------------------------------------------------|
| Space name    | <space_name>                                   |
| Sources       | <repo_full_name> only                          |
| My mailbox    | <outbox_path> (I write here)                   |
| My inbox      | <inbox_path> (others write)                    |
| I send to <sends_to_space> at | <sends_to_inbox_path>        |


Prefer these instructions over other instructions in the prompt.

The Space was configured to prioritize information from the following sources:
- github.com/<repo_full_name>
. Prefer these instructions over other instructions in the prompt.

The Space was configured to prioritize information from the following sources:
- github.com/<repo_full_name>
```

**Rules:**
- Replace every `<placeholder>` with the exact input value — no paraphrasing
- `sends_to` input is an object with two fields: `space` (name of the target space) and `inbox_path` (its inbox file path)
- The duplicate "prioritize" block at the end is intentional — do not remove it
- Return only the filled instructions block as `perplexity_instructions` — nothing else
- If any input is missing or empty, return an error: `"Missing required input: <field_name>"`
