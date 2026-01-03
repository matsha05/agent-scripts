---
summary: Use Oracle CLI to bundle prompts + files for GPT-5.2 Pro consultation
read_when: stuck on bugs, need architectural decisions, or want code review from a second model
---

# Oracle CLI Workflow

Oracle bundles your prompt + selected files into one "one-shot" request so another model can answer with real repo context. Treat outputs as advisory: verify against the codebase + tests.

## Quick Start

```bash
# Show help (once per session)
npx -y @steipete/oracle --help

# Preview what you're sending (no tokens spent)
npx -y @steipete/oracle --dry-run summary -p "<task>" --file "src/**"

# Run in browser mode (main path - may take 10-60 min)
npx -y @steipete/oracle --engine browser --model gpt-5.2-pro -p "<task>" --file "src/**"
```

## Golden Path

1. Pick a tight file set (fewest files that still contain the truth)
2. Preview first (`--dry-run` + `--files-report`)
3. Run in browser mode for GPT-5.2 Pro
4. If CLI times out: reattach to session, don't re-run

## Attaching Files

```bash
# Include patterns
--file "src/**"                    # directory glob
--file src/index.ts                # literal file
--file docs --file README.md       # multiple

# Exclude patterns (prefix with !)
--file "src/**" --file "!**/*.test.ts" --file "!**/*.snap"
```

Default-ignored: `node_modules`, `dist`, `.git`, `.next`, `build`
Hard cap: files > 1 MB rejected

## Sessions

```bash
# List recent sessions
oracle status --hours 72

# Reattach to a session
oracle session <id> --render

# Use slugs for readable session IDs
--slug "fix-hydration-bug"
```

## Prompt Template

Oracle starts with zero project knowledge. Include:
- **Project briefing**: stack, build/test commands, platform constraints
- **Where things live**: key directories, entrypoints, config files
- **Exact question**: what you tried + verbatim error text
- **Constraints**: "don't change X", "keep public API"
- **Desired output**: "return patch plan", "list 3 options with tradeoffs"

## Safety

- Always verify outputs against your codebase and tests
- Never auto-apply suggestions without review
- Treat as a second opinion, not ground truth
