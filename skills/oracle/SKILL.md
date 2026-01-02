---
name: oracle
description: Use the Oracle CLI to bundle prompts and files for GPT-5.2 Pro. Invoke when stuck on bugs, need architectural decisions, or want code review from a second model.
---

# Oracle (CLI) - Best Use

Oracle bundles your prompt + selected files into one "one-shot" request so another model can answer with real repo context (API or browser automation). Treat outputs as advisory: verify against the codebase + tests.

## Main Use Case (Browser + GPT-5.2 Pro)

Default workflow: `--engine browser` with GPT-5.2 Pro. This is the "human in the loop" path that can take 10 minutes to 1 hour. Expect a stored session you can reattach to.

Recommended defaults:
- Engine: browser (`--engine browser`)
- Model: GPT-5.2 Pro
- Attachments: directories/globs + excludes; avoid secrets

## Golden Path (Fast + Reliable)

1. Pick a tight file set (fewest files that still contain the truth)
2. Preview what you're about to send (`--dry-run` + `--files-report`)
3. Run in browser mode; use API only when explicitly wanted
4. If the run detaches/timeouts: reattach to the stored session (don't re-run)

## Commands (Preferred)

Show help (once per session):
```bash
npx -y @steipete/oracle --help
```

Preview (no tokens):
```bash
npx -y @steipete/oracle --dry-run summary -p "<task>" --file "src/**" --file "!**/*.test.*"
```

Browser run (main path):
```bash
npx -y @steipete/oracle --engine browser --model gpt-5.2-pro -p "<task>" --file "src/**"
```

Manual paste fallback:
```bash
npx -y @steipete/oracle --render --copy -p "<task>" --file "src/**"
```

## Attaching Files

`--file` accepts files, directories, and globs. You can pass it multiple times.

Include:
- `--file "src/**"` (directory glob)
- `--file src/index.ts` (literal file)
- `--file docs --file README.md`

Exclude (prefix with `!`):
- `--file "src/**" --file "!src/**/*.test.ts" --file "!**/*.snap"`

Default-ignored dirs: `node_modules`, `dist`, `.git`, `.next`, `build`
Hard cap: files > 1 MB are rejected

## Sessions

- Stored under `~/.oracle/sessions`
- Runs may detach or take a long time. If CLI times out: don't re-run; reattach
- List: `oracle status --hours 72`
- Attach: `oracle session <id> --render`
- Use `--slug "<3-5 words>"` to keep session IDs readable

## Prompt Template (High Signal)

Oracle starts with zero project knowledge. Include:
- Project briefing (stack + build/test commands + platform constraints)
- "Where things live" (key directories, entrypoints, config files)
- Exact question + what you tried + the error text (verbatim)
- Constraints ("don't change X", "must keep public API")
- Desired output ("return patch plan + tests", "list risky assumptions")

## Safety

- Always verify Oracle outputs against your codebase and tests
- Never auto-apply suggested changes without review
- Treat as a second opinion, not ground truth
