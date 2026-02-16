# Agents Instructions

This repository contains reusable skills for AI coding agents (Claude Code, OpenAI Codex, etc.).

## Repository Structure

- `skills/` - Each subdirectory is a self-contained skill
- Each skill has a `SKILL.md` as its entry point
- Optional `references/` directories contain supporting documentation

## How to Use Skills

When a user asks you to perform a task that matches a skill's trigger or description:

1. Read the skill's `SKILL.md` for complete instructions
2. Follow the phases/steps defined in the skill
3. Reference files in `references/` for detailed templates and configurations

## Available Skills

- **nextjs-project-generator** - Scaffolds production-ready Next.js projects with modern best practices (React 19, TypeScript, Tailwind v4, shadcn/ui, Prisma, NextAuth, TanStack Query)
