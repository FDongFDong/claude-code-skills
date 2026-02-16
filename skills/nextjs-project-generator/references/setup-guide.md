# Setup Guide - Next.js Project Generator

모든 설정 파일의 완전한 템플릿과 상세 설명입니다.

## next.config.ts

```typescript
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  // 타입 안전한 라우팅 (선택사항, 실험적 기능)
  // experimental: {
  //   typedRoutes: true,
  // },

  // 이미지 최적화
  images: {
    formats: ['image/avif', 'image/webp'],
    remotePatterns: [
      {
        protocol: 'https',
        hostname: '**.example.com',
      },
    ],
  },

  // 개발 환경 fetch 로깅
  logging: {
    fetches: {
      fullUrl: true,
    },
  },

  // 헤더 설정 (보안)
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [
          {
            key: 'X-Frame-Options',
            value: 'DENY',
          },
          {
            key: 'X-Content-Type-Options',
            value: 'nosniff',
          },
          {
            key: 'Referrer-Policy',
            value: 'strict-origin-when-cross-origin',
          },
        ],
      },
    ]
  },
}

export default nextConfig
```

## tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2017",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

## eslint.config.mjs

```javascript
import { dirname } from 'path'
import { fileURLToPath } from 'url'
import { FlatCompat } from '@eslint/eslintrc'

const __filename = fileURLToPath(import.meta.url)
const __dirname = dirname(__filename)

const compat = new FlatCompat({
  baseDirectory: __dirname,
})

const eslintConfig = [
  ...compat.extends(
    'next/core-web-vitals',
    'next/typescript'
  ),
  {
    rules: {
      '@typescript-eslint/no-unused-vars': [
        'error',
        { argsIgnorePattern: '^_', varsIgnorePattern: '^_' },
      ],
      '@typescript-eslint/no-explicit-any': 'error',
      'prefer-const': 'error',
      'no-console': ['warn', { allow: ['warn', 'error'] }],
    },
  },
]

export default eslintConfig
```

## prisma/schema.prisma

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

// NextAuth.js 연동 모델
model User {
  id            String    @id @default(cuid())
  name          String?
  email         String    @unique
  emailVerified DateTime?
  image         String?
  accounts      Account[]
  sessions      Session[]
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

model Account {
  id                String  @id @default(cuid())
  userId            String
  type              String
  provider          String
  providerAccountId String
  refresh_token     String? @db.Text
  access_token      String? @db.Text
  expires_at        Int?
  token_type        String?
  scope             String?
  id_token          String? @db.Text
  session_state     String?
  user              User    @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@unique([provider, providerAccountId])
}

model Session {
  id           String   @id @default(cuid())
  sessionToken String   @unique
  userId       String
  expires      DateTime
  user         User     @relation(fields: [userId], references: [id], onDelete: Cascade)
}

model VerificationToken {
  identifier String
  token      String   @unique
  expires    DateTime

  @@unique([identifier, token])
}
```

## .env.local

```bash
# ============================
# Database
# ============================
DATABASE_URL="postgresql://user:password@localhost:5432/mydb?schema=public"

# ============================
# Auth.js (NextAuth v5)
# ============================
AUTH_SECRET="" # 생성: openssl rand -base64 32

# ============================
# OAuth Providers (선택)
# ============================
# NextAuth v5 auto-detection: AUTH_[PROVIDER]_ID / AUTH_[PROVIDER]_SECRET 패턴 필수
# auth.config.ts에서 GitHub, Google을 인자 없이 import 시 이 패턴으로 자동 감지됨

# GitHub
AUTH_GITHUB_ID=""
AUTH_GITHUB_SECRET=""

# Google
AUTH_GOOGLE_ID=""
AUTH_GOOGLE_SECRET=""

# ============================
# 기타 (선택)
# ============================
# Vercel Analytics (자동 설정됨)
```

## .gitignore 추가 항목

create-next-app이 생성한 .gitignore에 아래 항목이 포함되어 있는지 확인합니다:

```gitignore
# env files
.env
.env.local
.env.*.local

# prisma
prisma/*.db
prisma/*.db-journal

# IDE
.idea/
.vscode/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db
```

## Tailwind CSS v4 설정

### postcss.config.mjs (Tailwind v4)
```javascript
/** @type {import('postcss-load-config').Config} */
const config = {
  plugins: ["@tailwindcss/postcss"],
}

export default config
```

### src/app/globals.css (Tailwind v4)
```css
@import 'tailwindcss';

/* shadcn/ui 선택 시: shadcn init이 생성한 CSS 변수(:root, .dark 블록)를 여기에 유지하세요 */

@theme {
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --font-sans: var(--font-geist-sans);
  --font-mono: var(--font-geist-mono);
}
```

> **참고**: Tailwind v4에서는 `tailwind.config.ts`가 필요 없습니다.
> CSS 파일에서 `@theme` 디렉티브로 커스터마이징합니다.
> create-next-app의 최신 버전이 자동으로 v4 설정을 생성합니다.
> **shadcn/ui 사용 시**: `shadcn init`이 `:root`와 `.dark` 블록에 CSS 변수(`--background`, `--foreground`, `--card`, `--primary` 등)를 추가합니다. 이 변수들을 `@import 'tailwindcss'`와 `@theme` 블록 사이에 유지하세요.

## middleware.ts (NextAuth 보호 라우트)

```typescript
import NextAuth from 'next-auth'
import { authConfig } from '@/auth.config'

export default NextAuth(authConfig).auth

export const config = {
  matcher: [
    // 보호할 라우트 패턴
    '/dashboard/:path*',
    '/settings/:path*',
    '/api/protected/:path*',
  ],
}
```

## src/lib/prisma.ts

```typescript
import { PrismaClient } from '@prisma/client'

const globalForPrisma = globalThis as unknown as {
  prisma: PrismaClient | undefined
}

export const prisma = globalForPrisma.prisma ?? new PrismaClient()

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma
```

## src/auth.config.ts (NextAuth v5 - Edge-safe)

> **아키텍처 설명**: NextAuth v5는 `auth.config.ts`(Edge-safe, providers만)와 `auth.ts`(full, adapter 포함)를 분리합니다.
> Middleware는 Edge Runtime에서 실행되므로 DB adapter 없이 `auth.config.ts`만 사용합니다.
> **Edge 제약**: `auth.config.ts`의 `authorized` 콜백은 Edge Runtime에서 실행됩니다. DB 접근, Prisma 호출 등 Node.js 전용 API를 사용하면 안 됩니다. DB 의존 로직은 반드시 `auth.ts`에 배치하세요.

```typescript
import type { NextAuthConfig } from 'next-auth'
import GitHub from 'next-auth/providers/github'
import Google from 'next-auth/providers/google'

export const authConfig = {
  providers: [
    GitHub,
    Google,
  ],
  pages: {
    signIn: '/auth/signin',
  },
  callbacks: {
    authorized({ auth, request: { nextUrl } }) {
      const isLoggedIn = !!auth?.user
      const isOnDashboard = nextUrl.pathname.startsWith('/dashboard')
      if (isOnDashboard) {
        return isLoggedIn
      }
      return true
    },
  },
} satisfies NextAuthConfig
```

## src/auth.ts (NextAuth v5 - Full config with adapter)

```typescript
import NextAuth from 'next-auth'
import { PrismaAdapter } from '@auth/prisma-adapter'
import { prisma } from '@/lib/prisma'
import { authConfig } from '@/auth.config'

export const { handlers, auth, signIn, signOut } = NextAuth({
  ...authConfig,
  adapter: PrismaAdapter(prisma),
  session: { strategy: "jwt" },
  callbacks: {
    ...authConfig.callbacks,
    session({ session, token }) {
      if (token.sub) session.user.id = token.sub
      return session
    },
  },
})
```

> **참고**: PrismaAdapter를 사용하려면 `pnpm add @auth/prisma-adapter`가 필요합니다.
> providers는 프로젝트 요구사항에 맞게 선택합니다.
> `next-auth@latest` (v5 정식)을 사용합니다 (`@beta` 아님).

## src/types/next-auth.d.ts (NextAuth 타입 확장)

> **필수**: `auth.ts`에서 `session.user.id`를 설정하므로, TypeScript가 이 필드를 인식하려면 타입 확장이 필요합니다.

```typescript
import { DefaultSession } from 'next-auth'

declare module 'next-auth' {
  interface Session {
    user: {
      id: string
    } & DefaultSession['user']
  }
}
```

## src/lib/providers.tsx (조건부 구성)

> **중요**: 이 파일은 사용자가 선택한 라이브러리에 따라 필요한 Provider만 포함합니다.
> 선택하지 않은 패키지의 import/Provider는 절대 포함하지 마세요 (빌드 에러 발생).

### NextAuth + TanStack Query + next-themes 전체 선택 시

```typescript
'use client'

import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { ReactQueryDevtools } from '@tanstack/react-query-devtools'
import { SessionProvider } from 'next-auth/react'
import { ThemeProvider } from 'next-themes'
import { useState, type ReactNode } from 'react'

export function Providers({ children }: { children: ReactNode }) {
  const [queryClient] = useState(
    () =>
      new QueryClient({
        defaultOptions: {
          queries: {
            staleTime: 60 * 1000,
            refetchOnWindowFocus: false,
          },
        },
      })
  )

  return (
    <SessionProvider>
      <QueryClientProvider client={queryClient}>
        <ThemeProvider
          attribute="class"
          defaultTheme="system"
          enableSystem
          disableTransitionOnChange
        >
          {children}
        </ThemeProvider>
        <ReactQueryDevtools initialIsOpen={false} />
      </QueryClientProvider>
    </SessionProvider>
  )
}
```

### TanStack Query만 선택 시

```typescript
'use client'

import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { ReactQueryDevtools } from '@tanstack/react-query-devtools'
import { useState, type ReactNode } from 'react'

export function Providers({ children }: { children: ReactNode }) {
  const [queryClient] = useState(
    () =>
      new QueryClient({
        defaultOptions: {
          queries: {
            staleTime: 60 * 1000,
            refetchOnWindowFocus: false,
          },
        },
      })
  )

  return (
    <QueryClientProvider client={queryClient}>
      {children}
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  )
}
```

### next-themes만 선택 시

```typescript
'use client'

import { ThemeProvider } from 'next-themes'
import type { ReactNode } from 'react'

export function Providers({ children }: { children: ReactNode }) {
  return (
    <ThemeProvider
      attribute="class"
      defaultTheme="system"
      enableSystem
      disableTransitionOnChange
    >
      {children}
    </ThemeProvider>
  )
}
```

> **다른 조합**: 위 3가지 예제를 기반으로 필요한 Provider만 포함/제거하세요.
> NextAuth 선택 시 `SessionProvider`로 래핑, 미선택 시 해당 import/Provider 제거.

### nuqs 포함 시 (NuqsAdapter 추가)

> nuqs v2+에서는 `NuqsAdapter`가 필수입니다. 가장 바깥쪽에 배치합니다.

```typescript
'use client'

import { NuqsAdapter } from 'nuqs/adapters/next/app'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { ReactQueryDevtools } from '@tanstack/react-query-devtools'
import { SessionProvider } from 'next-auth/react'
import { ThemeProvider } from 'next-themes'
import { useState, type ReactNode } from 'react'

export function Providers({ children }: { children: ReactNode }) {
  const [queryClient] = useState(
    () =>
      new QueryClient({
        defaultOptions: {
          queries: {
            staleTime: 60 * 1000,
            refetchOnWindowFocus: false,
          },
        },
      })
  )

  return (
    <NuqsAdapter>
      <SessionProvider>
        <QueryClientProvider client={queryClient}>
          <ThemeProvider
            attribute="class"
            defaultTheme="system"
            enableSystem
            disableTransitionOnChange
          >
            {children}
          </ThemeProvider>
          <ReactQueryDevtools initialIsOpen={false} />
        </QueryClientProvider>
      </SessionProvider>
    </NuqsAdapter>
  )
}
```

> **참고**: 위 예제는 전체 선택 시입니다. 실제로는 선택한 라이브러리의 Provider만 포함하세요.

## src/lib/utils.ts

```typescript
import { clsx, type ClassValue } from 'clsx'
import { twMerge } from 'tailwind-merge'

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}
```

## src/app/layout.tsx

```typescript
import type { Metadata } from 'next'
import type { ReactNode } from 'react'
import { Geist, Geist_Mono } from 'next/font/google'
import './globals.css'
import { Providers } from '@/lib/providers'

const geistSans = Geist({ variable: '--font-geist-sans', subsets: ['latin'] })
const geistMono = Geist_Mono({ variable: '--font-geist-mono', subsets: ['latin'] })

export const metadata: Metadata = {
  title: '프로젝트 이름',
  description: '프로젝트 설명',
}

export default function RootLayout({
  children,
}: {
  children: ReactNode
}) {
  return (
    <html lang="ko">
      <body className={`${geistSans.variable} ${geistMono.variable}`}>
        <Providers>{children}</Providers>
      </body>
    </html>
  )
}
```

## src/app/api/auth/[...nextauth]/route.ts

```typescript
import { handlers } from '@/auth'

export const { GET, POST } = handlers
```

## next-themes 설정 (선택)

> providers.tsx 조건부 구성에 이미 ThemeProvider가 포함되어 있습니다 (위 참조).

### layout.tsx의 html 태그 수정 (next-themes 선택 시)

```typescript
<html lang="ko" suppressHydrationWarning>
```

## next-intl 설정 (선택)

> **아키텍처**: next-intl은 `[locale]` 폴더 기반 라우팅을 사용합니다.

### src/i18n/routing.ts
```typescript
import { defineRouting } from 'next-intl/routing'

export const routing = defineRouting({
  locales: ['ko', 'en'],
  defaultLocale: 'ko',
})
```

### src/i18n/request.ts
```typescript
import { getRequestConfig } from 'next-intl/server'
import { routing } from './routing'

export default getRequestConfig(async ({ requestLocale }) => {
  let locale = await requestLocale

  if (!locale || !routing.locales.includes(locale as 'ko' | 'en')) {
    locale = routing.defaultLocale
  }

  return {
    locale,
    messages: (await import(`../../messages/${locale}.json`)).default,
  }
})
```

### next.config.ts (next-intl 플러그인 추가)
```typescript
import createNextIntlPlugin from 'next-intl/plugin'
import type { NextConfig } from 'next'

const withNextIntl = createNextIntlPlugin()

const nextConfig: NextConfig = {
  // ... 기존 설정
}

export default withNextIntl(nextConfig)
```

### middleware.ts (next-intl만 선택 시)

```typescript
import createMiddleware from 'next-intl/middleware'
import { routing } from '@/i18n/routing'

export default createMiddleware(routing)

export const config = {
  matcher: '/((?!api|trpc|_next|_vercel|.*\\..*).*)',
}
```

> **matcher 패턴 설명**: 네거티브 룩어헤드로 API 라우트, tRPC, Next.js 내부 경로(`_next`), Vercel 내부 경로(`_vercel`), 정적 파일(확장자 포함)을 자동 제외합니다. 로케일을 하드코딩하지 않으므로 `routing.ts`에서 로케일을 추가/삭제해도 middleware를 수정할 필요가 없습니다.

### middleware.ts (NextAuth + next-intl 동시 선택 시)

> **주의**: 두 라이브러리 모두 middleware가 필요하므로 수동으로 조합합니다.

```typescript
import createIntlMiddleware from 'next-intl/middleware'
import { routing } from '@/i18n/routing'
import { authConfig } from '@/auth.config'
import NextAuth from 'next-auth'

const intlMiddleware = createIntlMiddleware(routing)
const { auth } = NextAuth(authConfig)

const protectedPaths = ['/dashboard', '/settings']

function isProtectedPath(pathname: string) {
  return protectedPaths.some((path) =>
    pathname.includes(path)
  )
}

// NextAuth v5 공식 미들웨어 패턴: auth() 래퍼로 내보내기
// req.auth에서 세션 접근 (await auth()는 Server Component 전용)
export default auth((req) => {
  const { pathname } = req.nextUrl

  // 보호된 경로인 경우 인증 체크
  if (isProtectedPath(pathname) && !req.auth) {
    const signInUrl = new URL('/auth/signin', req.url)
    signInUrl.searchParams.set('callbackUrl', pathname)
    return Response.redirect(signInUrl)
  }

  // 모든 요청에 next-intl 미들웨어 적용
  return intlMiddleware(req)
})

export const config = {
  matcher: '/((?!api|trpc|_next|_vercel|.*\\..*).*)',
}
```

### src/app/[locale]/layout.tsx
```typescript
import type { ReactNode } from 'react'
import { NextIntlClientProvider } from 'next-intl'
import { getMessages } from 'next-intl/server'
import { notFound } from 'next/navigation'
import { routing } from '@/i18n/routing'

export default async function LocaleLayout({
  children,
  params,
}: {
  children: ReactNode
  params: Promise<{ locale: string }>
}) {
  const { locale } = await params

  if (!routing.locales.includes(locale as 'ko' | 'en')) {
    notFound()
  }

  const messages = await getMessages()

  return (
    <NextIntlClientProvider messages={messages}>
      {children}
    </NextIntlClientProvider>
  )
}
```

### src/app/[locale]/page.tsx
```typescript
import { useTranslations } from 'next-intl'

export default function HomePage() {
  const t = useTranslations('common')

  return (
    <main className="flex min-h-screen flex-col items-center justify-center p-24">
      <h1 className="text-4xl font-bold">{t('welcome')}</h1>
    </main>
  )
}
```
