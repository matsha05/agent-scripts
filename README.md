# Agent Scripts

Shared agent instructions for Matt Shaw's projects. This folder is the canonical source for universal rules, skills, and workflows that apply across all repositories.

## How It Works

### Pointer-Style AGENTS
Every project's `.agent/AGENTS.md` starts with:
```
READ ~/Desktop/dev/agent-scripts/AGENTS.md BEFORE ANYTHING (skip if missing).
```

Then lists only **project-specific** rules below that line.

This keeps:
- **Universal rules** in one place (this repo)
- **Project rules** in each repo's `.agent/AGENTS.md`

### Structure

```
agent-scripts/
├── AGENTS.md              # Universal rules (persona, work-style, git, quality, etc.)
├── README.md              # This file
└── skills/
    ├── frontend-design/
    │   └── SKILL.md       # Anti-AI-slop UI guidance
    └── oracle/
        └── SKILL.md       # Oracle CLI best practices
```

## Syncing With Other Repos

When you update `agent-scripts/AGENTS.md`:
1. Changes automatically apply to all projects that have the pointer
2. No need to copy/paste - agents will read from this file

When you update a **skill**:
1. Edit it here in `agent-scripts/skills/<skill>/SKILL.md`
2. All projects referencing it will get the update

### Sync Sweep Checklist
When adding a new project or syncing:
- [ ] Ensure `.agent/AGENTS.md` exists with the pointer line at top
- [ ] Add project-specific rules below the pointer
- [ ] Reference any needed skills in project instructions

## Skills

Skills are self-contained expert prompts for specific domains. They live in `skills/<name>/SKILL.md`.

| Skill | Purpose |
|-------|---------|
| `frontend-design` | Create distinctive, production-grade UIs that avoid generic AI aesthetics |
| `oracle` | Best practices for the Oracle CLI (bundling prompts + files for GPT-5.2 Pro) |

### Adding a New Skill
1. Create `skills/<skill-name>/SKILL.md`
2. Use YAML frontmatter with `name` and `description`
3. Document when/how to use the skill
4. Commit and push

## Consuming Repos

Current repos using this:
- `recruiter-in-your-pocket` - AI-powered career studio

## Usage

For AI agents working in Matt's repos:
1. Read `AGENTS.md` first (persona, work-style, principles)
2. Check `skills/` for relevant domain expertise
3. Then read the project's local `.agent/AGENTS.md` for project-specific context
