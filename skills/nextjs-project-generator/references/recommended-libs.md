# Recommended Libraries - Next.js Project Generator

Next.js 프로젝트에서 자주 사용되는 추천 라이브러리 카탈로그입니다.

## 필수 스택 (자동 설치)

| 카테고리 | 라이브러리 | 용도 | 설치 |
|---------|-----------|------|------|
| 프레임워크 | next | React 풀스택 프레임워크 | `create-next-app` |
| UI 라이브러리 | react, react-dom | 사용자 인터페이스 | `create-next-app` |
| 타입 | typescript | 타입 안전성 | `create-next-app` |
| CSS | tailwindcss | 유틸리티 CSS | `create-next-app` |
| 아이콘 | lucide-react | 아이콘 시스템 | `pnpm add lucide-react` |
| 유틸 | clsx, tailwind-merge | 클래스 조합 | `pnpm add clsx tailwind-merge` |

## 핵심 선택 스택 (AskUserQuestion으로 사용자 확인)

> Phase 2에서 사용자가 선택한 항목만 설치됩니다.

### UI 컴포넌트

| 라이브러리 | 용도 | 설치 | 비고 |
|-----------|------|------|------|
| shadcn/ui | 접근성 준수 UI 키트 | `pnpm dlx shadcn@latest init` | button, card, input, form, dialog, sonner, table |

### Prisma + NextAuth

| 라이브러리 | 용도 | 설치 | 비고 |
|-----------|------|------|------|
| @prisma/client | 타입 안전한 DB ORM | `pnpm add @prisma/client` | Client extensions 지원 |
| prisma | Prisma CLI | `pnpm add -D prisma` | 개발 의존성 |
| next-auth | NextAuth.js v5 (정식) | `pnpm add next-auth` | `@latest` 사용 (`@beta` 아님) |
| @auth/prisma-adapter | Prisma DB 어댑터 | `pnpm add @auth/prisma-adapter` | NextAuth + Prisma 연동 |

### TanStack Query + Zustand

| 라이브러리 | 용도 | 설치 | 비고 |
|-----------|------|------|------|
| @tanstack/react-query | 서버 상태 (데이터 페칭/캐싱) | `pnpm add @tanstack/react-query` | Suspense 지원 |
| zustand | 클라이언트 전역 상태 | `pnpm add zustand` | 경량, React 19 호환 |
| @tanstack/react-query-devtools | Query 디버깅 | `pnpm add -D @tanstack/react-query-devtools` | 개발 환경 전용 |

### React Hook Form + Zod

| 라이브러리 | 용도 | 설치 | 비고 |
|-----------|------|------|------|
| react-hook-form | 고성능 폼 라이브러리 | `pnpm add react-hook-form` | 비제어 컴포넌트 기반 |
| zod | 타입 안전한 스키마 검증 | `pnpm add zod` | 런타임 + 컴파일타임 |
| @hookform/resolvers | RHF + Zod 연결 | `pnpm add @hookform/resolvers` | 어댑터 패턴 |

## 추가 선택 스택 (사용자 선택)

### UI/UX 향상

| 라이브러리 | 용도 | 설치 | 비고 |
|-----------|------|------|------|
| framer-motion | 부드러운 애니메이션 | `pnpm add framer-motion` | 페이지 전환, 리스트 애니메이션 |
| next-themes | 다크모드 전환 | `pnpm add next-themes` | `suppressHydrationWarning` 필요 |
| sonner | 토스트 알림 | `pnpm dlx shadcn@latest add sonner` | shadcn/ui 통합 |
| @tanstack/react-table | 데이터 테이블 | `pnpm add @tanstack/react-table` | 정렬, 필터, 페이지네이션 |
| cmdk | 커맨드 팔레트 | `pnpm dlx shadcn@latest add command` | shadcn/ui 통합 |
| vaul | 모바일 드로어 | `pnpm dlx shadcn@latest add drawer` | shadcn/ui 통합 |
| react-day-picker | 날짜 선택기 | `pnpm dlx shadcn@latest add calendar` | shadcn/ui 통합 |

### 상태 & 데이터

| 라이브러리 | 용도 | 설치 | 비고 |
|-----------|------|------|------|
| nuqs | URL 상태 관리 | `pnpm add nuqs` | 검색/필터 URL 동기화 |
| next-safe-action | 타입 안전 Server Actions | `pnpm add next-safe-action` | Zod 스키마 통합 |

### 다국어 & 접근성

| 라이브러리 | 용도 | 설치 | 비고 |
|-----------|------|------|------|
| next-intl | 다국어 (i18n) | `pnpm add next-intl` | App Router 완벽 지원 |
| @axe-core/react | 접근성 자동 검사 | `pnpm add -D @axe-core/react` | 개발 환경 전용 |

## 라이브러리 선택 가이드

### 상태 관리 선택

```
질문: 어떤 종류의 상태인가?

서버 데이터 (API 응답, DB 데이터)
  → TanStack Query (핵심 선택 스택)

클라이언트 전역 상태 (UI 상태, 사용자 설정)
  → Zustand (핵심 선택 스택)

URL 상태 (검색어, 필터, 페이지)
  → nuqs (추가 선택 스택)

폼 상태 (입력값, 검증)
  → React Hook Form + Zod (핵심 선택 스택)
```

### 인증 선택

```
NextAuth.js v5 (핵심 선택 스택)
├── OAuth 프로바이더 (GitHub, Google, etc.)
├── 이메일/패스워드
├── Magic Link
└── Prisma Adapter로 DB 연동
```

### 데이터베이스 선택

```
Prisma (핵심 선택 스택)
├── PostgreSQL (추천, 기본)
├── MySQL
├── SQLite (개발/프로토타입)
└── MongoDB
```

## shadcn/ui 컴포넌트 추천

### 기본 컴포넌트 (shadcn/ui 선택 시)
```bash
pnpm dlx shadcn@latest add button card input form dialog sonner table
```

### 추가 추천 컴포넌트
```bash
# 네비게이션
pnpm dlx shadcn@latest add navigation-menu dropdown-menu

# 데이터 입력
pnpm dlx shadcn@latest add select textarea checkbox switch

# 피드백
pnpm dlx shadcn@latest add alert badge progress skeleton

# 레이아웃
pnpm dlx shadcn@latest add separator tabs accordion

# 고급
pnpm dlx shadcn@latest add command drawer calendar popover
```

### 전체 설치 (대규모 프로젝트)
```bash
pnpm dlx shadcn@latest add --all
```

## 버전 호환성

> Phase 0에서 Context7 MCP로 최신 버전을 확인합니다. 아래는 시스템 최소 요구사항입니다.

| 요구사항 | 최소 버전 | 비고 |
|---------|----------|------|
| Node.js | 18.17+ | LTS 권장, `node -v`로 확인 |
