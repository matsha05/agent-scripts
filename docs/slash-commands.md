---
summary: How to use and create slash commands for agent workflows
read_when: setting up shortcuts, creating reusable prompts, or customizing agent behavior
---

# Slash Commands

Slash commands are reusable prompt templates that agents can invoke with a simple `/command-name` syntax.

## Locations

| Location | Scope |
|----------|-------|
| `~/.codex/prompts/` | Global (all projects) |
| `docs/slash-commands/` | Repo-specific |
| `.agent/workflows/` | Repo-specific (alternative) |

## Creating a Slash Command

Create a markdown file with frontmatter:

```markdown
---
description: Short description of what this command does
---

The actual prompt content goes here.
Include any context, instructions, or templates the agent needs.
```

## Common Commands

| Command | Purpose |
|---------|---------|
| `/handoff` | Prepare context for handing off to another agent |
| `/pickup` | Resume work from a previous handoff |
| `/review` | Request code review |
| `/debug` | Systematic debugging workflow |

## Best Practices

1. Keep commands focused on a single task
2. Include clear success criteria
3. Reference docs or skills when relevant
4. Use `// turbo` annotation for auto-runnable steps
