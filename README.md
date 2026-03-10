# Lion Connect 🦁

> **멋쟁이사자처럼 수료생과 중소기업을 연결하는 채용 플랫폼 MVP**
> 1개월 설계 + 1개월 개발 | 프론트엔드 단독 개발(설계 단계 프론트 2인, 개발단계 단독 개발) | 60건 지원, 4건 매칭 달성

![Next.js](https://img.shields.io/badge/Next.js-15.5.7-black?logo=next.js)
![React](https://img.shields.io/badge/React-19.1.2-61DAFB?logo=react)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript)
![TanStack Query](https://img.shields.io/badge/TanStack_Query-5.90.5-FF4154)

---

## 📖 목차

- [프로젝트 소개](#-프로젝트-소개)
- [주요 페이지](#-주요-페이지)
- [기술적 의사결정](#-기술적-의사결정)
- [잘한 부분 (설계 중심)](#-잘한-부분-설계-중심)
- [기술 스택](#-기술-스택)
- [프로젝트 구조](#-프로젝트-구조)

---

## 🎯 프로젝트 소개

### 배경

AI로 인해 IT 채용 시장이 위축되었지만, **50인 이하 중소기업은 오히려 채용난**을 겪고 있었습니다.
수료생은 어디든 지원하고 싶지만, 중소기업 공고에 노출되지 않는 문제가 있었습니다.

**가설**: "멋사 수료생과 파트너사를 직접 연결하면 채용이 발생할 것이다"

### 검증 결과

- 8개 파트너사 채용공고 게시
- **60건 지원 발생**
- **4건 채용 매칭** (50% 매칭률)
- 1개월 만에 가설 검증 성공 ✅

### MVP 개발 원칙

```
✅ 가설 검증에 필요한 핵심 기능만 구현
✅ 실제 사용자(수료생 + 기업)가 사용 가능한 최소 상태
✅ 확장 가능하지만, 지금 당장 필요 없는 건 구현 안 함

P0: 프로필 등록, 채용공고, 지원하기 (핵심)
P1: 역할별 접근 제어, 인재 검색 (필수)
P2: 알림, 채팅, 추천 (검증 후 추가) ← 과감히 제외
```

---

## 📱 주요 페이지

### 1. 랜딩 페이지 `/`

기업 대상 서비스 소개 및 CTA

<details>
<summary>주요 섹션</summary>

- Hero Section (메인 헤드라인, CTA)
- 서비스 소개
- 기능 소개
- 통계 (CountUp 애니메이션)
- 기업 문의 폼

</details>

```
<!-- 스크린샷 추가 예정 -->
![랜딩 페이지](docs/screenshots/landing.png)
```

---

### 2. 회원가입 `/signup`

3가지 회원 유형별 맞춤 플로우

**플로우**:
1. 회원 유형 선택 (일반/기업/멋사 수료자)
2. 단계별 정보 입력 (Step 1, Step 2)
3. 기업: 이메일 인증 필수
4. 자동 로그인 및 역할별 리다이렉트

```
<!-- 스크린샷 추가 예정 -->
![회원가입 - 유형 선택](docs/screenshots/signup-type.png)
![회원가입 - Step 1](docs/screenshots/signup-step1.png)
```

---

### 3. 인재 프로필 관리 `/dashboard/profile/[profileId]`

**40개 이상의 필드를 가진 복잡한 멀티 섹션 폼**

**섹션** (14개):
- 프로필 이미지
- 개인정보 (이름, 제목, 소개)
- 직무/직군 선택
- 경력, 학력, 어학, 자격증, 수상 경력
- 포트폴리오 (PDF)
- 링크, Work Driven 테스트

**기술 구현**:
```typescript
// React Hook Form FormProvider로 전체 폼 통합
<FormProvider {...methods}>
  <ProfileImageSection />
  <PersonalInfoSection />
  <CareerSection />
  {/* 14개 섹션... */}
</FormProvider>

// dirtyFields 추적으로 변경된 필드만 API 호출
const changedFields = getChangedFields(dirtyFields);
await updateProfile(changedFields); // 효율적!
```

```
<!-- 스크린샷 추가 예정 -->
![프로필 편집](docs/screenshots/profile-edit.png)
![경력 섹션](docs/screenshots/profile-career.png)
```

---

### 4. 채용 공고 관리 (기업) `/jobs`

기업이 채용 공고를 등록/수정/관리

**기능**:
- 이미지 업로드 (S3 Presigned URL)
- 공고 게시/비공개 전환
- 지원자 관리 (`/jobs/[jobId]/applicants`)

```
<!-- 스크린샷 추가 예정 -->
![채용 공고 등록](docs/screenshots/job-create.png)
![지원자 관리](docs/screenshots/job-applicants.png)
```

---

### 5. 인재 검색 (기업) `/talents`

기업이 인재 프로필을 검색 및 필터링

**기능**:
- 직무/직군/경험/스킬 필터링
- 뱃지 시스템 (부트캠프, 창업, 자격증, 전공자)
- 인재 상세 프로필 보기

```
<!-- 스크린샷 추가 예정 -->
![인재 검색](docs/screenshots/talents-search.png)
![인재 상세](docs/screenshots/talent-detail.png)
```

---

### 6. 관리자 대시보드 `/admin`

시스템 관리자 페이지

**기능**:
- 사용자 관리 (잠금/해제)
- 기업 관리
- 채용 공고 관리
- 문의사항 처리

```
<!-- 스크린샷 추가 예정 -->
![관리자 대시보드](docs/screenshots/admin-dashboard.png)
![사용자 관리](docs/screenshots/admin-users.png)
```

---

## 💡 기술적 의사결정

### 1. CSR 중심 + SEO 필요한 곳만 SSR

**왜 CSR을 선택했나?**

```
✅ 대부분 페이지가 로그인 필요 → SSR 의미 없음
✅ 에러 트래킹 명확성 (브라우저 DevTools)
✅ 빠른 개발 속도 (MVP 1개월 시간 제약)

🎯 SEO 필요한 곳만 SSR 적용:
- 채용공고 상세: generateMetadata로 동적 OG 태그
- 랜딩 페이지: 검색 엔진 노출
```

### 2. TanStack Query로 사용자가 기다리지 않게

**사용자 문제**:
- 페이지 이동할 때마다 로딩... 로딩...
- 같은 데이터를 계속 다시 불러옴

**해결**:
```typescript
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60 * 1000,  // 5분 캐싱
      gcTime: 10 * 60 * 1000,    // 10분
    },
  },
});
```

**결과**:
- 한 번 본 데이터는 즉시 표시
- 불필요한 API 호출 70% 감소
- 페이지 전환이 빠르다고 느낌 ⚡

### 3. 메모리 토큰 저장으로 XSS 방어

**쉬운 방법** (하지 않음):
```typescript
// ❌ localStorage에 토큰 저장
localStorage.setItem('accessToken', token);
// → 개발 빠름, but 악성 스크립트가 탈취 가능
```

**선택한 방법**:
```typescript
// ✅ 메모리 + HttpOnly 쿠키 조합
┌────────────────────────────────────────────────┐
│ accessToken  → 메모리만 (탈취 불가)            │
│ refreshToken → HttpOnly 쿠키 (JS 접근 불가)   │
│ user 정보    → localStorage (민감정보 아님)    │
└────────────────────────────────────────────────┘
```

**트레이드오프**:
- 개발 시간 조금 더 걸림
- **하지만 사용자 개인정보 보호** (MVP여도 타협 안 함)

### 4. Promise 캐싱으로 race condition 해결

**문제 상황**:
```
페이지 로드 → 5개 API 동시 호출
→ 토큰 만료 → 5개 모두 401 에러
→ 토큰 갱신 5번 발생 ❌ (race condition)
```

**해결**:
```typescript
let refreshPromise: Promise<string> | null = null;

async function refreshAccessToken() {
  if (refreshPromise) return refreshPromise; // 재사용!

  refreshPromise = (async () => {
    const token = await callRefreshAPI();
    refreshPromise = null;
    return token;
  })();

  return refreshPromise;
}
```

**결과**:
- 토큰 갱신 1회로 통합 ✅
- 사용자는 끊김 없이 서비스 이용

---

## ✨ 잘한 부분 (설계 중심)

### 1. 레이어드 아키텍처로 확장성 확보

```
app/          → 라우팅 + 페이지 (UI 레이어)
hooks/        → 비즈니스 로직 (useQuery, useMutation)
lib/          → 인프라 (API 클라이언트)
types/        → 타입 정의 (End-to-End 타입 안전성)
components/   → 재사용 UI
```

**효과**:
- 나중에 기능 추가해도 어디에 넣을지 명확
- 다른 개발자가 와도 구조 파악 쉬움
- 버그 발생 시 어느 레이어인지 즉시 파악

### 2. 중앙화된 API 클라이언트

```typescript
// lib/apiClient.ts
export async function request<T>(url: string, options?: RequestOptions): Promise<T> {
  // 1. 자동으로 Authorization 헤더 추가
  // 2. 401 에러 시 자동 토큰 리프레시
  // 3. 타임아웃 처리 (10초)
  // 4. 에러 코드별 분기
}

// ✅ 모든 API 호출이 이 클라이언트를 거침
// → 인증 로직 한 곳에서 관리
```

**효과**:
- 인증 로직 변경 시 한 파일만 수정
- 일관된 에러 처리
- 타임아웃, 재시도 등 공통 로직 중복 제거

### 3. Route Groups로 역할별 분리

```
app/
├── (auth)/        # 라우트 그룹: 인증 (URL에 영향 X)
├── (company)/     # 기업 서비스
├── admin/         # 관리자
└── dashboard/     # 일반 사용자
```

**효과**:
- URL 구조에 영향 없이 논리적 그룹화
- 각 그룹별 layout.tsx로 독립적 레이아웃
- 미들웨어에서 그룹별 접근 제어 쉬움

### 4. 미들웨어 기반 RBAC

```typescript
// middleware.ts
const PROTECTED_ROUTES = [
  { path: "/talents", roles: ["ADMIN", "COMPANY"], redirectTo: "/dashboard" },
  { path: "/admin", roles: ["ADMIN"], redirectTo: "/" },
];

// 페이지 진입 전에 서버에서 권한 체크
// → 클라이언트 깜빡임 없음
// → 보안 강화 (클라이언트 우회 불가)
```

### 5. 인수인계 문서 미리 작성

```markdown
# HANDOVER.md (284줄)

- 핵심 설계 결정과 이유
- 폴더 구조와 역할
- 개발 패턴과 예시 코드
- 트러블슈팅 가이드
- API 클라이언트 사용법
- 타입 정의 규칙
```

**효과**:
- "이거 어떻게 돌아가요?" 질문 예방
- 다음 개발자가 바로 작업 가능
- 내가 나중에 봐도 이해 가능

### 6. 타입 중앙화로 일관성 확보

```typescript
// types/talent.ts - Request/Response 명확히 분리
export interface EducationRequest {
  schoolName: string;
  major?: string;      // optional
}

export interface EducationResponse {
  id: number;
  schoolName: string;
  major: string | null;  // nullable
}

// 150+ 타입 정의로 End-to-End 타입 안전성
```

---

## 🛠 기술 스택

### Core

| 카테고리 | 기술 | 버전 | 선택 이유 |
|---------|------|------|----------|
| **Framework** | Next.js | 15.5.7 | App Router, 라우팅 최적화 |
| **Library** | React | 19.1.2 | 최신 기능 (Suspense, Transition) |
| **Language** | TypeScript | 5 | 타입 안전성 (150+ 타입) |
| **Build Tool** | Turbopack | - | 빠른 HMR |

### State Management

| 라이브러리 | 버전 | 사용 목적 |
|-----------|------|----------|
| **Zustand** | 5.0.8 | 클라이언트 상태 (auth, toast) |
| **TanStack Query** | 5.90.5 | 서버 상태, 캐싱 (5분 stale) |

### Form & Validation

| 라이브러리 | 버전 | 사용 목적 |
|-----------|------|----------|
| **React Hook Form** | 7.65.0 | 복잡한 멀티 섹션 폼 처리 |
| **Zod** | 4.1.12 | 런타임 검증 + 타입 추론 |

### UI

| 라이브러리 | 버전 | 사용 목적 |
|-----------|------|----------|
| **Tailwind CSS** | 4 | 유틸리티 기반 스타일링 |
| **shadcn/ui** | - | Radix UI 기반 접근성 |
| **Framer Motion** | 12.23.24 | 애니메이션 |

---

## 📂 프로젝트 구조

```
lion-connect-frontend/
├── app/                      # Next.js App Router
│   ├── (auth)/              # Route Group: 인증
│   │   ├── login/
│   │   └── signup/
│   ├── (company)/           # Route Group: 기업
│   │   ├── jobs/
│   │   └── talents/
│   ├── admin/               # 관리자
│   └── dashboard/           # 사용자
│
├── components/              # 재사용 컴포넌트
│   ├── ui/                 # shadcn/ui 기반
│   └── form/               # 폼 컴포넌트
│
├── hooks/                   # 커스텀 훅 (37개)
│   ├── auth/               # useLogin, useLogout
│   ├── talent/             # useMyProfiles
│   └── common/             # useDebounce
│
├── lib/                     # 핵심 라이브러리
│   ├── apiClient.ts        # 중앙 API 클라이언트 ⭐
│   └── api/                # API 엔드포인트
│
├── types/                   # TypeScript 타입 (150+)
│   ├── auth.ts
│   ├── talent.ts
│   └── job.ts
│
├── store/                   # Zustand 스토어
│   ├── authStore.ts
│   └── toastStore.ts
│
├── schemas/                 # Zod 스키마
│   └── auth/
│
├── constants/               # 상수 정의
│   ├── api.ts              # API 엔드포인트
│   └── jobMapping.ts
│
├── middleware.ts            # RBAC 접근 제어 ⭐
└── HANDOVER.md              # 인수인계 문서 (284줄)
```

---

## 🔧 주요 파일 설명

### `lib/apiClient.ts` (핵심 ⭐⭐⭐)

모든 HTTP 요청의 중심

```typescript
export const apiClient = {
  get: <T>(url: string, options?) => Promise<T>
  post: <T>(url: string, data?, options?) => Promise<T>
  put: <T>(url: string, data?, options?) => Promise<T>
  delete: <T>(url: string, options?) => Promise<T>
}

// 자동 기능:
// ✅ Authorization 헤더 추가
// ✅ 401 에러 시 자동 리프레시
// ✅ 타임아웃 처리 (10초)
// ✅ Promise 캐싱으로 중복 리프레시 방지
```

### `middleware.ts` (보안 핵심)

```typescript
// 페이지 진입 전 서버에서 권한 체크
const PROTECTED_ROUTES = [
  { path: "/talents", roles: ["ADMIN", "COMPANY"] },
  { path: "/admin", roles: ["ADMIN"] },
];

// HttpOnly 쿠키에서 역할 읽기
const userRoles = cookies().get("user-roles");
```

### `store/authStore.ts` (인증 상태)

```typescript
interface AuthState {
  accessToken: string | null;  // 메모리만 (XSS 방어)
  user: User | null;           // localStorage (UX)
}

// Zustand persist 설정
persist(
  (set) => ({ /* ... */ }),
  {
    name: "auth-store",
    partialize: (state) => ({ user: state.user }), // 토큰 제외!
  }
)
```

---


## 🎓 배운 점

### 기술적 성장

- **MVP 우선순위 판단**: "무엇을 안 만들지" 결정하는 능력
- **사용자 관점 기술 선택**: 기술은 목적이 아닌 수단
- **보안과 개발 속도 균형**: MVP여도 타협하지 않는 것

### 아직 부족한 점

- 테스트 코드 작성 경험 (1개월 시간 제약)
- SSR/서버 컴포넌트 심화 활용
- 대규모 트래픽 대응 경험

### 향후 계획

```
✅ 완료: MVP 가설 검증
⬜ 다음: hooks/ 폴더 단위 테스트 추가
⬜ 계획: SSR 필요 페이지 점진적 마이그레이션
⬜ 계획: 성능 모니터링 (Vercel Analytics)
```

---

## 📄 라이선스

MIT License

---

<div align="center">

**Built with 💙 by a developer who thinks before coding**

*"완벽한 프로덕트가 아닌, 검증된 MVP를 빠르게 만드는 개발자"*

</div>
