# claude-code-skills

**Claude Code**와 **OpenAI Codex** (CLI 에이전트)에서 사용할 수 있는 스킬 모음입니다.

개인적으로 사용하려고 만든 스킬들을 정리한 저장소입니다. 두 도구 모두 동일한 [SKILL.md](https://docs.anthropic.com/en/docs/claude-code/skills) 오픈 표준을 사용하기 때문에, 이 저장소의 모든 스킬은 별도 수정 없이 양쪽에서 동작합니다.

## 설치 방법

### Claude Code

```bash
# 스킬 하나만 복사
cp -r skills/<skill-name> ~/.claude/skills/

# 또는 전체 저장소를 클론하고 심볼릭 링크
git clone https://github.com/FDongFDong/claude-code-skills.git
ln -s "$(pwd)/claude-code-skills/skills/<skill-name>" ~/.claude/skills/<skill-name>
```

### OpenAI Codex

```bash
# Codex 스킬 디렉토리에 복사
cp -r skills/<skill-name> ~/.codex/skills/

# 또는 프로젝트 내부에 배치
cp -r skills/<skill-name> .codex/skills/
```

## 포함된 스킬

| 스킬 | 설명 | 트리거 |
|------|------|--------|
| [nextjs-project-generator](skills/nextjs-project-generator/) | Next.js + React 19 + TypeScript + Tailwind v4 + shadcn/ui + Prisma + NextAuth + TanStack Query 프로젝트 자동 생성 | `"next.js 프로젝트 만들어줘"` |

## 스킬 구조

각 스킬은 SKILL.md 오픈 표준을 따릅니다:

```
skills/<skill-name>/
├── SKILL.md              # 스킬 메인 파일 (frontmatter + 실행 지침)
└── references/           # 참조 문서 (선택)
    ├── setup-guide.md
    └── recommended-libs.md
```

### SKILL.md Frontmatter

```yaml
---
name: skill-name
description: 스킬 설명과 트리거 조건
trigger: 스킬을 활성화하는 예시 프롬프트
version: 1.0.0
---
```

## 라이선스

MIT
