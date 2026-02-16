# claude-code-skills

Reusable skills for **Claude Code** and **OpenAI Codex** (CLI agents).

Both tools use the same open [SKILL.md](https://docs.anthropic.com/en/docs/claude-code/skills) standard, so every skill in this repo works with either agent out of the box.

## Installation

### Claude Code

```bash
# Copy a single skill
cp -r skills/<skill-name> ~/.claude/skills/

# Or clone the whole repo and symlink
git clone https://github.com/FDongFDong/claude-code-skills.git
ln -s "$(pwd)/claude-code-skills/skills/<skill-name>" ~/.claude/skills/<skill-name>
```

### OpenAI Codex

```bash
# Copy into your Codex skills directory
cp -r skills/<skill-name> ~/.codex/skills/

# Or place inside your project
cp -r skills/<skill-name> .codex/skills/
```

## Included Skills

| Skill | Description | Trigger |
|-------|-------------|---------|
| [nextjs-project-generator](skills/nextjs-project-generator/) | Next.js + React 19 + TypeScript + Tailwind v4 + shadcn/ui + Prisma + NextAuth + TanStack Query project scaffolding | `"next.js 프로젝트 만들어줘"` |

## Skill Structure

Each skill follows the SKILL.md open standard:

```
skills/<skill-name>/
├── SKILL.md              # Main skill file (frontmatter + instructions)
└── references/           # Optional supporting documents
    ├── setup-guide.md
    └── recommended-libs.md
```

### SKILL.md Frontmatter

```yaml
---
name: skill-name
description: What the skill does and when to trigger it
trigger: Example prompt that activates the skill
version: 1.0.0
---
```

## Contributing

1. Create a new directory under `skills/`
2. Add a `SKILL.md` with proper frontmatter
3. Include any reference files in `references/`
4. Submit a PR

## License

MIT
