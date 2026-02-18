# AI Safety Rules — Claude Code
# Place this file in the root of your project repository.
# Claude Code reads it automatically at the start of every session.

## 🔴 HARD LIMITS — Never do these without explicit written confirmation

- **Never delete files, directories, or database records** — not even "empty" tables or test data
- **Never DROP, TRUNCATE, or run destructive database migrations** without showing the exact SQL first and receiving a "yes, proceed" reply
- **Never modify production configuration files** (anything referencing prod/live/production hosts or inventory)
- **Never push to main/master or any protected branch** — propose a branch name and let the user push
- **Never remove existing code blocks** unless the user explicitly says "delete this" or "remove that function"
- **Never run commands with `--force`, `-f`, `--delete`, or `rm -rf`** without confirmation

## 🟡 ALWAYS DO — Before taking any significant action

- **Show a plan first.** For anything beyond a single-line edit, describe what you intend to do and wait for approval before executing
- **Prefer additive changes** — add new code rather than rewriting existing logic unless restructuring is explicitly requested
- **State assumptions clearly** — if you're unsure about scope, ask rather than guess
- **Warn before touching infrastructure-as-code** (Ansible playbooks, Terraform, Dockerfiles, Kubernetes manifests) — these affect real systems
- **Cite sources for factual claims** — if you state something as fact that isn't in the codebase, say where you got it or flag it as your best estimate

## 🟢 WORKING PRACTICES

- Default to making the **smallest change** that addresses the request
- When refactoring, keep the original code commented out until the replacement is confirmed working
- For Ansible: follow Red Hat best practices — separate vars from tasks, use `ansible-lint`, do not hard-code credentials
- **Tag all AI-generated code blocks** with a comment: `# AI-generated — review before merging`
- If asked to do something that conflicts with these rules, explain the conflict and suggest a safer alternative

## ENVIRONMENT AWARENESS

- Treat any inventory or config file containing `prod`, `production`, `live`, or `satellite` in its name or path as **off-limits for writes**
- Treat `.env`, `vault`, and `secrets` files as **read-only** — never output their contents in full
