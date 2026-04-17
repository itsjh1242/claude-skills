---
version: 2.0.0
updated: 2026-04-17
changelog:
  - "2.0.0: 메이저 업그레이드 — 디자인+아키텍처 스킬 병합, 라우팅 테이블 구조"
  - "  + Anthropic frontend-design: 디자인 씽킹, 미학 방향성, Anti-AI-slop"
  - "  + Addy Osmani frontend-ui-engineering: 컴포넌트 아키텍처, 조합 패턴, 상태 결정 트리"
  - "  + 라우팅 테이블 패턴: references/ 온디맨드 로딩으로 토큰 비용 최소화"
  - "1.0.0: 최초 릴리스"
---

# Frontend Master

## 설치 (Setup)

### Step 0 — 버전 확인 (필수 — 건너뛰지 않는다)
다른 작업 전에 반드시 아래 단계를 모두 수행한다:
1. 현재 프로젝트의 `.claude/skills/frontend-master/SKILL.md` 파일을 읽는다
2. 파일이 존재하지 않으면 → 신규 설치. Step 1로 이동.
3. 파일이 존재하면 → YAML frontmatter에서 `version:` 값을 추출
   - `version` 필드가 없으면 → `0.0.0`으로 간주
4. 로컬 버전과 `2.0.0` (이 파일의 버전)을 비교:
   - 로컬 < `2.0.0` → **업데이트 흐름** 실행
   - 로컬 = `2.0.0` → "frontend-master는 이미 최신 버전입니다 (v2.0.0)" 출력 후 중단. 다른 작업 하지 않는다.
5. 비교 결과를 반드시 출력한다: "로컬: v{local} / 원격: v2.0.0 → {action}"

### 업데이트 흐름
이전 버전에서 업데이트할 때:
1. `.claude/skills/frontend-master/SKILL.md`를 이 파일로 덮어쓰기
2. `CLAUDE.md`의 `# Frontend Master` 섹션을 아래 Step 3의 최신 내용으로 교체
3. 이전 버전과 현재 버전 사이의 changelog 항목을 사용자에게 표시
4. `.dev/frontend/` 디렉토리와 기존 데이터는 보존 — 삭제하지 않음
5. "frontend-master 업데이트 완료: v{old} → v{new}" 출력

### Step 1 — 디렉토리/파일 생성
다음 경로를 프로젝트 루트에 생성:
- `.dev/frontend/components/`   (컴포넌트 카탈로그 디렉토리)
- `.dev/frontend/styles.md`     (아래 초기 템플릿으로 생성)

초기 템플릿 (`styles.md`):
```
---
last_updated: (date)
project_type: unknown
ui_library: none
---
# Style Guide
<!-- Phase 1 분석 후 Frontend Master가 자동으로 채움 -->
```

### Step 2 — 스킬 파일 설치
다음 디렉토리를 프로젝트 루트에 생성:
- `.claude/skills/frontend-master/`

이 SKILL.md 파일을 fetch해서 `.claude/skills/frontend-master/SKILL.md`로 저장한다.

### Step 3 — CLAUDE.md에 추가
프로젝트 루트의 `CLAUDE.md` (없으면 생성) 에 다음 지침을 추가:

```
# Frontend Master
프론트엔드 관련 파일(React 컴포넌트, 페이지, 레이아웃, 스타일, 훅 등)이 생성되거나 수정될 때,
`.claude/skills/frontend-master/SKILL.md`를 읽고 그 가이드라인을 자동으로 따른다.
이는 모든 프론트엔드 작업에 적용된다 — 새 기능, 버그 수정, 리팩토링, 스타일링.
- 스킬 위치: .claude/skills/frontend-master/SKILL.md
- 컴포넌트 카탈로그: .dev/frontend/components/
- 스타일 가이드: .dev/frontend/styles.md
사용자가 프론트엔드 작업을 명시적으로 요청하면 (예: "랜딩 페이지 만들어줘", "UI 수정해줘", "컴포넌트 추가해줘"),
Frontend Master가 해당 작업의 전체 소유권을 갖는다.
```

### Step 4 — 검증
- `.dev/frontend/styles.md` 존재 여부 확인
- `CLAUDE.md`에 Frontend Master 섹션 존재 여부 확인
- 신규 설치 시 → "Frontend Master 셋업 완료 (v2.0.0)" 출력
- 업데이트 시 → "Frontend Master 업데이트 완료: v{old} → v{new}" 출력

---

## 역할
React + Next.js 프로젝트에서 디자인 씽킹과 컴포넌트 아키텍처부터 구현, 품질 검증까지 모든 프론트엔드 작업을 완전히 담당하는 종합 프론트엔드 개발 스킬.

## 트리거 조건
Frontend Master는 두 가지 방식으로 활성화된다:

### 자동 감지
CLI가 다음 컨텍스트를 조합하여 판단한다 — 키워드 매칭이 아니다:
- 프론트엔드 파일(`.tsx`, `.jsx`, `.css`, `.scss`, `.module.css`)이 생성 또는 수정되고 있는가?
- 사용자의 요청이 UI, 컴포넌트, 페이지, 레이아웃, 스타일링, 상태 관리와 관련이 있는가?
- 프론트엔드 구현이 필요한 새 기능에 대한 대화인가?

이 중 하나라도 참이면, CLI는 **자동으로 Frontend Master 가이드라인을 적용**한다.

### 명시적 호출
사용자가 프론트엔드 작업을 명시적으로 요청할 때:
- "랜딩 페이지 만들어줘"
- "UI 수정해줘"
- "컴포넌트 추가해줘"
- "이 디자인 구현해줘"
- "frontend-master" 키워드

CLI가 전체 소유권을 갖고 관련 Phase를 모두 실행한다.

## 로딩 가이드 (온디맨드 참조)

이 SKILL.md는 핵심 워크플로우를 포함한다. 필요시 참조 파일을 로드한다:

| 상황 | 로드 |
|------|------|
| 비주얼/UI 작업 구현, 스타일링 또는 디자인 결정 | `references/design-guide.md` |
| 컴포넌트 아키텍처 설계, 구조적 결정 | `references/architecture-guide.md` |

Complex 작업: Phase 2 전에 두 파일을 모두 로드한다.
Medium 작업: 비주얼 작업이면 `design-guide.md`, 구조 작업이면 `architecture-guide.md` 로드.
Simple 작업: 참조 파일을 로드하지 않는다 — 인라인 규칙만 적용한다.

## 핵심 원칙

### 기술 스택 기본값
- **프레임워크**: Next.js (App Router) with Server Components 기본
- **스타일링**: Tailwind CSS를 기본 스타일링 도구로 사용
- **UI 라이브러리**: shadcn/ui 컴포넌트 우선, Radix UI 프리미티브 대안
- **상태 관리**: Server Components + URL state 우선 → React Context → 필요시에만 외부
- **TypeScript**: 항상 TypeScript와 strict typing 사용

### 성능 예산
| 항목 | 목표 |
|------|------|
| JS (gzipped) | < 200KB |
| LCP | ≤ 2.5s |
| INP | ≤ 200ms |
| CLS | ≤ 0.1 |

### 주요 규칙
- **Anti-AI 미학**: 절대 제네릭 AI스러운 UI를 만들지 않는다 (보라색 그라디언트, rounded-2xl 남발, 스톡 카드 그리드). 전체 테이블은 `references/design-guide.md` 참조.
- **타이포그래피**: 아이덴티티 폰트로 Inter, Roboto, Arial, system-ui를 사용하지 않는다.
- **Server Components 우선**: 기본적으로 Server Components 사용, 인터랙티비티에만 `"use client"` 추가.
- **설정보다 조합**: 작은 조합 가능한 컴포넌트를 구축 — boolean props가 많은 모놀리식 컴포넌트가 아님.
- **스켈레톤 로딩**: 예상 콘텐츠 레이아웃과 일치하는 스켈레톤 사용 (스피너가 아님).

## Phase 1 — 분석 (항상 실행)

### 1A. 프로젝트 컨텍스트 감지
- 프로젝트 타입 감지 (Next.js App Router / Pages Router / 일반 React)
- 기존 스타일링 방식과 UI 라이브러리 식별
- TypeScript 설정 및 기존 컴포넌트 패턴 확인

### 1B. 작업 규모 평가
- **Simple**: 단일 컴포넌트 수정, 스타일 조정, 텍스트 변경 → Phase 2 건너뛰고 Phase 3로
- **Medium**: 2-5개 컴포넌트, 페이지 섹션, 새 기능 → 간단한 Phase 2
- **Complex**: 전체 페이지, 다중 페이지 기능, 아키텍처 변경 → 전체 Phase 2

출력: "작업 규모: {Simple/Medium/Complex} — {이유}"

### 1C. 기존 컴포넌트 스캔
- `.dev/frontend/components/` 카탈로그에서 재사용 가능한 컴포넌트 확인
- 프로젝트에서 유사한 기존 컴포넌트 스캔
- 설치된 shadcn/ui 컴포넌트 식별

## Phase 2 — 설계 (규모에 따라)

Complex 작업: 진행 전 `references/design-guide.md`와 `references/architecture-guide.md`를 로드한다.

### 2A. 컴포넌트 아키텍처
- UI를 컴포넌트 계층 구조로 분해
- props 및 인터페이스 정의
- Server → Client 데이터 흐름 경계 계획
- 조합 over 설정 적용 (`references/architecture-guide.md` 참조)

### 2B. 스타일 전략
- 필요한 Tailwind 유틸리티 클래스 결정
- 반응형 브레이크포인트 계획 (모바일 퍼스트: 320px, 768px, 1024px+, 1440px)
- 비주얼 작업: 디자인 씽킹 프레임워크 적용 (`references/design-guide.md` 참조)

### 2C. 구현 계획
간결한 계획을 출력:
```
## 구현 계획
- 생성할 컴포넌트: {목록}
- 수정할 컴포넌트: {목록}
- 사용할 shadcn/ui 컴포넌트: {목록}
- Server/Client 경계: {설명}
- 디자인 방향성: {해당 시}
- 예상 규모: {Simple/Medium/Complex}
```

Complex 작업의 경우에만 사용자 확인을 기다린다. Simple/Medium은 자동으로 진행.

## Phase 3 — 구현 (항상 실행)

### 3A. 컴포넌트 생성
```tsx
// 1. 임포트 (그룹핑: react/next → 라이브러리 → 내부 → 타입)
// 2. 타입 정의 (props 인터페이스)
// 3. 컴포넌트 함수 (named export)
// 4. 서브 컴포넌트 (작은 것은 같은 파일에)
// 5. 익스포트 (named, default는 Next.js 페이지만)
```

### 3B. 스타일링 규칙
- 기본적으로 Tailwind CSS 클래스 사용
- 조건부 클래스 병합에는 `lib/utils`의 `cn()` 사용
- Tailwind 접두사로 모바일 퍼스트 반응형 디자인
- Tailwind spacing scale (0.25rem 단위)으로 일관된 간격
- 테마 종속 값에는 CSS 변수
- 타이포그래피, 색상, 모션, 공간 구성 규칙 → `references/design-guide.md` 참조

### 3C. 상태 관리
상태 관리 결정 트리를 따른다 (`references/architecture-guide.md` 참조):
Local → Lifted → URL → Context → Server → External (프로젝트에서 사용하는 경우만)

### 3D. 디자인-투-코드
디자인 참조에서 구현할 때:
1. 시각적 구조 분석: 레이아웃, 간격, 타이포그래피, 색상
2. 디자인 토큰을 Tailwind 클래스에 매핑
3. shadcn/ui 컴포넌트 패턴 식별
4. 픽셀 퍼펙트 레이아웃을 먼저 구현한 후 인터랙티비티 추가
5. 반응형 동작이 디자인 의도와 일치하는지 확인

### 3E. 에러 처리 및 로딩 상태
- 예상 콘텐츠 레이아웃과 일치하는 스켈레톤 로딩
- 컴포넌트 수준 에러 처리를 위한 에러 바운더리
- Next.js `loading.tsx` 및 `error.tsx` 컨벤션
- 엣지 케이스에 대한 의미 있는 폴백 UI

## Phase 4 — 검증 (항상 실행)

### 4A. 접근성 체크
- 시맨틱 HTML 요소 (`<nav>`, `<main>`, `<section>`, `<article>`)
- 인터랙티브 요소에 ARIA 라벨
- 이미지에 의미 있는 `alt` 텍스트, 폼에 연결된 라벨
- 색상 대비가 WCAG AA 기준 충족
- 키보드 탐색 동작 (탭 순서, 포커스 관리)

### 4B. 반응형 디자인 체크
- 모바일(320px), 태블릿(768px), 데스크탑(1024px+)에서 레이아웃 동작
- 모바일에서 터치 타겟이 최소 44x44px
- 가로 스크롤 없음, 네비게이션 적응, 이미지 반응형

### 4C. 성능 체크
- 불필요한 클라이언트 사이드 JavaScript 없음 (Server Components 우선)
- 이미지가 `next/image`와 적절한 사이징 사용
- 큰 컴포넌트를 적절히 lazy-loading
- 불필요한 리렌더 없음 (필요한 곳에 적절한 메모이제이션)

### 4D. 자동 수정
- 접근성 문제는 즉시 수정 — 허락을 구하지 않는다
- 반응형 레이아웃 문제는 즉시 수정 — 허락을 구하지 않는다
- 아키텍처적 성능 우려는 제안과 함께 사용자에게 알린다

### 4E. 카탈로그 업데이트
`.dev/frontend/components/` 업데이트:
- 새 컴포넌트 항목 추가: 이름, 경로, props 요약, 의존성
- 수정된 컴포넌트 항목 업데이트
- 삭제된 컴포넌트 항목 제거

## 파일 구조 컨벤션

```
src/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   └── {route}/
│       ├── page.tsx / layout.tsx / loading.tsx / error.tsx
├── components/
│   ├── ui/           (shadcn/ui)
│   ├── {feature}/    (기능별, 콜로케이션)
│   └── shared/
├── lib/utils.ts      (cn() 및 유틸리티)
├── hooks/
└── types/
```
