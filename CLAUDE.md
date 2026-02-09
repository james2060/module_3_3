# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 프로젝트 개요

**LogWatch Admin** - 실시간 로그 수집, 분석 및 모니터링을 위한 웹 기반 어드민 대시보드

## 명령어

```bash
npm install          # 의존성 설치
npm run dev          # 개발 서버 시작 (Turbopack, localhost:3000)
npm run build        # 프로덕션 빌드
npm run start        # 프로덕션 서버 시작
npm run lint         # ESLint 린트 검사
```

**사전 요구사항**: Node.js 18 이상, npm

## 아키텍처

**Next.js 16 App Router** 기반의 FE + BE 통합 프로젝트. TypeScript 전체 적용, Tailwind CSS v4 스타일링, Turbopack 번들러.

### 라우팅 구조

라우트 그룹을 사용하여 레이아웃 분리. URL에는 나타나지 않음:

- **`src/app/(dashboard)/`** — 대시보드 라우트 그룹
  - `/` - 대시보드 메인 (실시간 로그 스트림, 통계 차트)
  - `/logs` - 로그 검색 및 필터링
  - `/alerts` - 알림 규칙 관리
  - `/settings` - 시스템 설정
  - 레이아웃: `Sidebar` + `Header`

- **`src/app/(auth)/`** — 인증 라우트 그룹
  - `/login` - 로그인
  - `/register` - 회원가입
  - 레이아웃: 중앙 정렬 (사이드바 없음)

- **`src/app/api/`** — API 라우트 핸들러
  - `/api/logs` - 로그 목록/상세 조회
  - `/api/alerts` - 알림 규칙 CRUD
  - `/api/stats` - 대시보드 통계
  - `/api/auth/*` - 인증 처리

### 디렉토리 구조

```
src/
├── app/              # Next.js App Router (페이지, 레이아웃, API)
├── components/
│   ├── ui/           # 재사용 UI 프리미티브 (Button, Input, Card)
│   ├── layout/       # Sidebar, Header
│   └── features/     # 기능별 복합 컴포넌트 (로그 뷰어, 차트 등)
├── lib/              # 유틸리티, 상수, 헬퍼
├── hooks/            # 커스텀 React 훅
├── types/            # TypeScript 타입 정의
├── services/         # API 클라이언트 함수
└── styles/           # 글로벌 스타일
```

## 주요 기능

- **대시보드**: 실시간 로그 스트림, 레벨별 필터링 (ERROR/WARN/INFO/DEBUG), 시간대별 추이 차트
- **로그 검색**: 키워드 검색, 날짜/시간 범위 필터, 소스별 분류
- **알림 관리**: 임계값 기반 알림 규칙, 채널 관리 (이메일, 슬랙)
- **시스템 관리**: 사용자 계정, 로그 수집 소스 설정, 보존 정책

## 코딩 컨벤션

- **import 별칭**: `@/*`는 `src/*`에 매핑
  ```typescript
  import { Sidebar } from "@/components/layout/Sidebar";
  ```

- **컴포넌트 export**: named export 사용 (default export 금지)
  ```typescript
  export function LogTable() { /* ... */ }
  ```

- **API 응답**: `NextResponse.json()` 사용
  ```typescript
  import { NextResponse } from "next/server";
  export async function GET() {
    return NextResponse.json({ logs: [] });
  }
  ```

- **언어**: 사용자 대면 문자열과 주석에 한국어 사용
