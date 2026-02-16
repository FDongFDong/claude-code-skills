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
- **verify-implementation** - 프로젝트의 모든 verify 스킬을 순차 실행하여 통합 검증 보고서를 생성합니다
- **manage-skills** - 세션 변경사항을 분석하여 검증 스킬 누락을 탐지하고, 새 스킬을 생성/업데이트합니다
