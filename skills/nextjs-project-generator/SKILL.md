---
name: nextjs-project-generator
description: Next.js + React 19 + TypeScript + Tailwind v4 + shadcn/ui + Prisma + NextAuth + TanStack Query 프로젝트 자동 생성. MCP로 최신 버전 자동 확인. "next.js 프로젝트 만들어줘", "next 프로젝트 생성", "웹앱 만들어줘" 시 트리거.
trigger: "next.js 프로젝트 만들어줘"
version: 2.0.0
---

# Next.js Project Generator

## 목적

최신 베스트프랙티스(App Router, Server Components, NextAuth v5, Prisma)가 적용된 **프로덕션급 Next.js 프로젝트**를 자동으로 생성합니다.

## 패키지 매니저 안내

이 가이드는 **pnpm**을 기본 패키지 매니저로 사용합니다.

### pnpm 설치
```bash
npm install -g pnpm
# 또는
corepack enable
```

### npm 사용자를 위한 변환 가이드
| pnpm 명령어 | npm 명령어 |
|------------|-----------|
| `pnpm add [package]` | `npm install [package]` |
| `pnpm add -D [package]` | `npm install -D [package]` |
| `pnpm dlx [package]` | `npx [package]` |
| `pnpm [script]` | `npm run [script]` |

## 핵심 기술 스택

### 필수 스택
- **Next.js** - React 풀스택 프레임워크 (App Router)
- **React 19** - Server Components, Actions, use() 등 최신 기능
- **TypeScript** - Strict mode
- **Tailwind CSS v4** - 최신 CSS 기능 (`@tailwindcss/postcss`)

### 선택 스택 (AskUserQuestion으로 사용자 확인)
- **shadcn/ui** - 고품질 UI 컴포넌트 라이브러리
- **Prisma + NextAuth.js v5** - ORM + 인증 시스템
- **TanStack Query + Zustand** - 서버/클라이언트 상태 관리
- **React Hook Form + Zod** - 고성능 폼 + 타입 안전한 검증
- **next-themes** - 다크모드
- **framer-motion** - 애니메이션
- **next-intl** - 다국어 (i18n)
- **nuqs** - URL 상태 관리

## 사용 시점

### 자동 활성화 트리거
1. **사용자 프롬프트**: "next.js 프로젝트 만들어줘", "next 프로젝트 생성", "웹앱 만들어줘", "풀스택 프로젝트 생성"
2. **파일 편집**: `next.config.ts` 편집 시, `package.json`에서 next 의존성 감지 시

---

## Claude 실행 워크플로우

### Phase 0: MCP 최신 버전 조회 (MANDATORY)

스킬 실행 시 **반드시** Context7 + Perplexity로 핵심 라이브러리 최신 안정 버전을 조회합니다.

> **Next.js 16 참고**: Next.js 16부터 React Compiler와 Turbopack이 기본 활성화됩니다. `create-next-app`이 이를 자동으로 설정하므로 별도 구성이 불필요합니다. 조회 시 최신 기본값 변경사항을 확인하세요.

**조회 대상 라이브러리**:

| 라이브러리 | Context7 ID | 용도 |
|-----------|------------|------|
| next | /vercel/next.js | 프레임워크 |
| react | /reactjs/react.dev | UI 라이브러리 |
| tailwindcss | /tailwindlabs/tailwindcss.com | CSS |
| @prisma/client | /prisma/docs | ORM |
| next-auth | /nextauthjs/next-auth | 인증 |
| @tanstack/react-query | /tanstack/query | 서버 상태 |
| zustand | /pmndrs/zustand | 클라이언트 상태 |
| react-hook-form | /react-hook-form/react-hook-form | 폼 |
| zod | /colinhacks/zod | 검증 |
| shadcn/ui | /shadcn-ui/ui | UI 컴포넌트 |

**조회 후 테이블로 정리하여 사용자에게 확인**. 사용자 확인 후 다음 Phase로 진행합니다.

---

### Phase 1: 프로젝트 스캐폴딩

#### 1.1 프로젝트 이름 확인
AskUserQuestion으로 프로젝트 이름을 확인합니다.

#### 1.2 create-next-app 실행
```bash
pnpm create next-app@latest [프로젝트명] --yes --use-pnpm
```

> **`--yes` 플래그**: TypeScript, Tailwind CSS, ESLint, App Router, Turbopack, `src/` 디렉토리, `@/*` import alias가 모두 기본 활성화됩니다. 개별 플래그를 나열할 필요 없습니다.
> 최신 create-next-app은 React Compiler, Biome 린터 옵션도 포함될 수 있으므로 Phase 0에서 확인하세요.

#### 1.3 프로젝트 디렉토리 이동
```bash
cd [프로젝트명]
```

---

### Phase 2: 라이브러리 선택 (AskUserQuestion)

스캐폴딩 후, **AskUserQuestion**으로 필요한 라이브러리를 사용자에게 확인합니다.

#### 2.1 핵심 라이브러리 선택 (multiSelect: true)

```
AskUserQuestion({
  questions: [
    {
      question: "어떤 핵심 라이브러리를 설치할까요? (기본적으로 모두 권장됩니다)",
      header: "핵심 스택",
      multiSelect: true,
      options: [
        { label: "shadcn/ui (권장)", description: "접근성 준수 UI 컴포넌트 (button, card, input, form, dialog, sonner, table)" },
        { label: "Prisma + NextAuth", description: "DB ORM + 인증 시스템 (GitHub/Google OAuth 등)" },
        { label: "TanStack Query + Zustand", description: "서버 상태 관리 + 클라이언트 상태 관리" },
        { label: "RHF + Zod", description: "고성능 폼 + 타입 안전한 스키마 검증" }
      ]
    }
  ]
})
```

#### 2.2 선택 라이브러리 확인 (multiSelect: true)

```
AskUserQuestion({
  questions: [
    {
      question: "추가로 설치할 라이브러리가 있나요?",
      header: "추가 스택",
      multiSelect: true,
      options: [
        { label: "next-themes", description: "다크모드/라이트모드 테마 전환" },
        { label: "framer-motion", description: "부드러운 페이지 전환 및 UI 애니메이션" },
        { label: "next-intl", description: "다국어(i18n) 지원" },
        { label: "nuqs", description: "검색/필터 URL 상태 동기화" }
      ]
    }
  ]
})
```

---

### Phase 3: 선택에 따른 설치

사용자가 선택한 라이브러리에 대해:
1. **Context7 MCP로 최신 패턴 확인** (MANDATORY)
2. **선택된 것만 합쳐서 한 번에 설치**

#### 3.1 설치 명령어 매핑

| 선택 항목 | 설치 명령어 |
|----------|------------|
| shadcn/ui | `pnpm dlx shadcn@latest init` → globals.css 보존 확인 → `pnpm dlx shadcn@latest add button card input form dialog sonner table` |
| Prisma + NextAuth | `pnpm add @prisma/client next-auth @auth/prisma-adapter` → `pnpm add -D prisma` → `pnpm dlx prisma init` |
| TanStack Query + Zustand | `pnpm add @tanstack/react-query zustand` → `pnpm add -D @tanstack/react-query-devtools` |
| RHF + Zod | `pnpm add react-hook-form zod @hookform/resolvers` |
| next-themes | `pnpm add next-themes` |
| framer-motion | `pnpm add framer-motion` |
| next-intl | `pnpm add next-intl` |
| nuqs | `pnpm add nuqs` |

> **중요**: `next-auth`는 v5가 정식 출시되었으므로 `next-auth@latest`를 사용합니다 (`@beta` 아님).
>
> **shadcn/ui 초기화 주의**: `pnpm dlx shadcn@latest init` 실행 시 `globals.css`를 덮어쓸 수 있습니다. 초기화 후 Tailwind v4의 `@import 'tailwindcss'`와 `@theme` 설정이 보존되었는지 확인하세요. shadcn이 생성한 CSS 변수(`--background`, `--foreground` 등)는 유지하되, `@import 'tailwindcss'` 구문이 누락되지 않도록 합니다.

**공통 유틸리티** (항상 설치):
```bash
pnpm add lucide-react clsx tailwind-merge
```

---

### Phase 4: 조건부 파일 생성 (CRITICAL)

> **핵심 규칙**: 사용자가 선택하지 않은 라이브러리의 파일은 **절대 생성하지 않습니다**.
> 선택하지 않은 패키지의 import가 있으면 빌드 에러가 발생합니다.

#### 4.1 조건부 파일 생성 매트릭스

| 파일 | 생성 조건 | 참조 |
|------|----------|------|
| `src/lib/utils.ts` | **항상** | setup-guide.md |
| `src/app/layout.tsx` | **항상** (내용은 선택에 따라 변동) | 아래 4.2 참조 |
| `src/app/page.tsx` | **항상** | create-next-app 기본 |
| `src/app/globals.css` | **항상** | setup-guide.md (Tailwind v4) |
| `src/lib/providers.tsx` | TanStack Query **또는** NextAuth **또는** next-themes 선택 시 | 아래 4.3 참조 |
| `src/auth.config.ts` | Prisma + NextAuth 선택 시 | setup-guide.md |
| `src/auth.ts` | Prisma + NextAuth 선택 시 | setup-guide.md |
| `src/lib/prisma.ts` | Prisma + NextAuth 선택 시 | setup-guide.md |
| `prisma/schema.prisma` | Prisma + NextAuth 선택 시 | setup-guide.md |
| `src/app/api/auth/[...nextauth]/route.ts` | Prisma + NextAuth 선택 시 | setup-guide.md |
| `middleware.ts` | Prisma + NextAuth 선택 시 | setup-guide.md |
| `.env.local` | Prisma + NextAuth 선택 시 | setup-guide.md |
| `src/types/next-auth.d.ts` | Prisma + NextAuth 선택 시 | setup-guide.md |
| `src/lib/stores/` | Zustand 선택 시 | 디렉토리만 생성 |
| `src/lib/validations/` | RHF + Zod 선택 시 | 디렉토리만 생성 |
| `src/i18n/request.ts` | next-intl 선택 시 | setup-guide.md |
| `src/i18n/routing.ts` | next-intl 선택 시 | setup-guide.md |
| `messages/en.json`, `messages/ko.json` | next-intl 선택 시 | setup-guide.md |
| `src/app/[locale]/layout.tsx` | next-intl 선택 시 | setup-guide.md |
| `src/app/[locale]/page.tsx` | next-intl 선택 시 | setup-guide.md |

#### 4.2 layout.tsx 조건부 구성

layout.tsx의 내용은 선택에 따라 달라집니다:

**Providers가 필요한 경우** (TanStack Query, NextAuth, next-themes 중 하나라도 선택):
```typescript
import { Providers } from '@/lib/providers'
// ... Providers로 children 래핑
```

**Providers가 불필요한 경우** (위 3개 모두 미선택):
```typescript
// Providers import 없이 children 직접 렌더링
```

**next-intl 선택 시**: `app/[locale]/layout.tsx`가 메인 레이아웃이 되고, `app/layout.tsx`는 `<html>`, `<body>`만 포함.

**next-themes 선택 시**: `<html>` 태그에 `suppressHydrationWarning` 추가.

#### 4.3 providers.tsx 조건부 조합

providers.tsx는 선택된 라이브러리에 따라 **필요한 Provider만** 포함합니다:

```
선택 조합 → Provider 래핑 순서 (바깥 → 안쪽):

NextAuth만:
  SessionProvider → children

TanStack Query만:
  QueryClientProvider → children + DevTools

NextAuth + TanStack Query:
  SessionProvider → QueryClientProvider → children + DevTools

+ next-themes 추가 시:
  ... → ThemeProvider → children (가장 안쪽)

+ nuqs 추가 시:
  NuqsAdapter → ... → children (가장 바깥)
```

**금지 사항**:
- 선택하지 않은 패키지의 Provider를 포함하지 않음
- 선택하지 않은 패키지의 import문을 작성하지 않음

---

### Phase 5: 설정 파일 생성

> 모든 설정 파일의 완전한 코드 템플릿은 `references/setup-guide.md`를 참조하세요.

#### 5.1 필수 설정 파일

| 파일 | 설명 | 참조 |
|------|------|------|
| `next.config.ts` | App Router 설정, 이미지 최적화, 보안 헤더 | setup-guide.md |
| `tsconfig.json` | Strict mode, path alias `@/*` → `./src/*` | create-next-app 기본 확인 |
| `eslint.config.mjs` | next/core-web-vitals + TypeScript 규칙 | setup-guide.md |
| `postcss.config.mjs` | `@tailwindcss/postcss` (Tailwind v4) | setup-guide.md |
| `src/app/globals.css` | `@import 'tailwindcss'` + `@theme` | setup-guide.md |

#### 5.2 조건부 설정 파일

사용자 선택에 따라 Phase 4 매트릭스에 명시된 파일만 생성합니다.

**NextAuth v5 아키텍처** (Prisma + NextAuth 선택 시):
```
src/auth.config.ts   ← Edge-safe 설정 (providers만, adapter 없음)
src/auth.ts          ← 전체 설정 (PrismaAdapter + auth.config 합침)
middleware.ts        ← auth.config.ts 사용 (Edge Runtime 호환)
```

> NextAuth v5는 `auth.config.ts`(Edge-safe)와 `auth.ts`(full, with adapter)를 분리합니다.
> Middleware는 Edge Runtime에서 실행되므로 DB adapter 없이 `auth.config.ts`만 사용합니다.

**next-intl 아키텍처** (next-intl 선택 시):
```
src/i18n/request.ts  ← getRequestConfig (서버 컴포넌트용)
src/i18n/routing.ts  ← defineRouting (로케일 설정)
next.config.ts       ← createNextIntlPlugin으로 래핑
middleware.ts        ← createMiddleware(routing)
app/[locale]/        ← 로케일 기반 폴더 구조
```

---

### Phase 6: 디렉토리 구조

#### 6.1 기본 디렉토리 생성
```bash
mkdir -p src/components/shared
mkdir -p src/features
mkdir -p src/hooks
mkdir -p src/types
```

#### 6.2 조건부 디렉토리 생성
```bash
# Zustand 선택 시
mkdir -p src/lib/stores

# RHF + Zod 선택 시
mkdir -p src/lib/validations

# next-intl 선택 시
mkdir -p src/i18n
mkdir -p messages
```

#### 6.3 최종 디렉토리 구조 (전체 선택 시)

```
src/
├── app/
│   ├── [locale]/             # next-intl 선택 시
│   │   ├── layout.tsx        # NextIntlClientProvider 래핑
│   │   └── page.tsx          # 로케일 기반 홈 페이지
│   ├── layout.tsx            # RootLayout
│   ├── page.tsx              # 홈 페이지
│   ├── globals.css           # Tailwind CSS
│   └── api/
│       └── auth/[...nextauth]/route.ts  # NextAuth 선택 시
├── auth.config.ts            # NextAuth 선택 시 (Edge-safe)
├── auth.ts                   # NextAuth 선택 시 (Full config)
├── components/
│   ├── ui/                   # shadcn/ui 선택 시
│   └── shared/               # 공통 컴포넌트
├── features/                 # 기능별 모듈
├── lib/
│   ├── prisma.ts             # NextAuth 선택 시
│   ├── providers.tsx          # 조건부 (Phase 4.3 참조)
│   ├── stores/               # Zustand 선택 시
│   ├── validations/          # RHF+Zod 선택 시
│   └── utils.ts              # cn() 유틸 (항상)
├── hooks/                    # 커스텀 훅
├── i18n/                     # next-intl 선택 시
│   ├── request.ts
│   └── routing.ts
└── types/                    # 글로벌 타입
```

---

### Phase 7: 검증

#### 7.1 조건부 검증

| 검증 항목 | 명령어 | 조건 |
|----------|--------|------|
| Prisma Generate | `pnpm dlx prisma generate` | Prisma 선택 시 |
| ESLint | `pnpm lint` | 항상 |
| TypeScript | `pnpm exec tsc --noEmit` | 항상 |
| Build | `pnpm build` | 항상 |
| Dev Server | `pnpm dev` | 항상 |

#### 7.2 성공 기준

✅ Context7 MCP 검증 완료
✅ `pnpm lint` → 0 errors
✅ `pnpm exec tsc --noEmit` → 0 errors
✅ `pnpm build` → 성공
✅ 선택한 라이브러리의 보일러플레이트 정상 동작
✅ 개발 서버 실행 확인

---

## 주의사항

### Next.js App Router
- `'use client'` 디렉티브는 클라이언트 컴포넌트에만 사용
- Server Components가 기본 → 상태, 이벤트 핸들러 필요 시에만 `'use client'` 추가
- Server Actions으로 서버 로직 처리 (`'use server'`)
### TypeScript Strict Mode
- 모든 타입은 명시적으로 선언
- `any` 타입 사용 금지
- 옵셔널 체이닝(`?.`) 적극 활용
- Zod로 런타임 타입 검증

### ESLint + Build 검증
- **반드시** 0 errors로 통과해야 함
- 빌드 실패 시 문제 해결 후 재시도

---

## 다음 단계

프로젝트 생성 후:
1. 필요한 도메인(features) 추가
2. Prisma 스키마 확장 및 마이그레이션 (`pnpm dlx prisma migrate dev`)
3. NextAuth 프로바이더 설정 (GitHub, Google 등)
4. Server Actions로 API 로직 구현
5. 선택 라이브러리 통합 (다크모드, 다국어 등)
6. `nextjs-best-practice` 스킬 참조하여 기능 개발

## Resources

### 상세 문서 (references/)
1. **setup-guide.md**: 모든 설정 파일의 완전한 코드 템플릿과 상세 설명
2. **recommended-libs.md**: 추천 라이브러리 카탈로그 및 선택 가이드

### 연계 스킬
- **nextjs-best-practice**: 기존 Next.js 프로젝트에서 기능 개발
- **nextjs-frontend-guidelines**: 코딩 가이드라인 및 컨벤션
- **vibe-coding-frontend**: 환경 감지 + Context7 자동 조회
- **backend-guidelines**: Route Handlers + Prisma + NextAuth 백엔드 개발

---

**버전**: 2.0.0
**최종 업데이트**: 2026년 2월
