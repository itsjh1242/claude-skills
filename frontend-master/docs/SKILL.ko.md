---
version: 2.0.0
updated: 2026-04-17
changelog:
  - "2.0.0: 메이저 업그레이드 — 5개 외부 스킬 통합"
  - "  + Anthropic frontend-design: 디자인 씽킹, 미학 방향성, 타이포그래피/색상/모션/공간 규칙, Anti-AI-slop"
  - "  + Addy Osmani frontend-ui-engineering: 컴포넌트 아키텍처, 조합 패턴, Anti-AI 미학 테이블, 상태 관리 결정 트리"
  - "  + Addy Osmani performance-optimization: Core Web Vitals, 성능 예산, 안티패턴, 측정 워크플로우"
  - "  + masuP9 a11y-specialist-skills: WCAG 2.2 AA, 자동화 테스트 스크립트 (axe-core + Playwright), 4단계 감사"
  - "  + Anthropic webapp-testing: Playwright 브라우저 테스트, 정찰-후-액션 패턴"
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
React + Next.js 프로젝트에서 디자인 씽킹과 컴포넌트 아키텍처부터 구현, 성능 검증, 접근성 감사까지 모든 프론트엔드 작업을 완전히 담당하는 종합 프론트엔드 개발 스킬.

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

## 핵심 원칙

### 기술 스택 기본값
- **프레임워크**: Next.js (App Router) with Server Components 기본
- **스타일링**: Tailwind CSS를 기본 스타일링 도구로 사용
- **UI 라이브러리**: shadcn/ui 컴포넌트를 우선 사용, 대안으로 Radix UI 프리미티브 활용
- **상태 관리**: 기존 패턴을 감지하고 따른다. 신규 프로젝트: Server Components + URL state 우선, 그 다음 React Context, 필요시에만 외부 라이브러리
- **TypeScript**: 항상 TypeScript와 strict typing 사용
- **테스트**: Playwright로 브라우저 테스트, axe-core로 접근성 자동화

### 성능 예산
| 항목 | 목표 |
|------|------|
| JavaScript (gzipped) | < 200KB |
| CSS | < 50KB |
| 이미지 (개당) | < 200KB |
| 폰트 | < 100KB |
| API 응답 (p95) | < 200ms |
| 4G에서 TTI | < 3.5s |
| Lighthouse 점수 | ≥ 90 |
| LCP | ≤ 2.5s |
| INP | ≤ 200ms |
| CLS | ≤ 0.1 |

### Anti-AI 미학 규칙
절대로 "AI스러운" 제네릭 UI를 만들지 않는다. 다음 패턴은 금지:

| 패턴 | 대안 |
|------|------|
| 보라색/인디고 그라디언트 | 의도적이고 독특한 색상 체계 사용 |
| 과도한 둥근 모서리 (rounded-2xl 남발) | border-radius를 의도적으로 변화 |
| 제네릭 히어로 섹션 ("미래를 환영합니다") | 가치 중심, 구체적인 메시지로 시작 |
| Lorem ipsum 또는 플레이스홀더 텍스트 | 실제 의미 있는 콘텐츠 작성 |
| 과도한 패딩/히어로 섹션 | 밀도를 존중 — 모든 것이 여백을 필요로 하지 않음 |
| 스톡 카드 그리드 (3열 아이콘+제목+설명) | 그리드를 깨고 에디토리얼 레이아웃 사용 |
| 그림자 과다 카드 디자인 | 보더, 대비, 배경색 변화 사용 |
| 예측 가능한 대칭 레이아웃 | 비대칭, 겹침, 시각적 긴장 도입 |

**타이포그래피 금지**: Inter, Roboto, Arial, system-ui를 주 아이덴티티 폰트로 사용하지 않는다. 프로젝트의 미학 방향성에 맞는 독특한 폰트를 선택한다.

### 상태 관리 결정 트리
다음 우선순위를 따른다:
1. **로컬 상태** (`useState`) — 컴포넌트 범위 UI 상태
2. **끌어올린 상태** — 부모와 자식 간 공유
3. **URL 상태** — 필터, 페이지네이션, 공유 가능한 상태 (`useSearchParams`)
4. **React Context** — 컴포넌트 간 클라이언트 상태
5. **서버 상태** — Server Components / Server Actions로 데이터 페칭
6. **외부 스토어** — 프로젝트가 이미 사용 중인 경우에만 (Zustand, Jotai 등)

프로젝트가 이미 의존하지 않는 한 외부 상태 라이브러리를 추가하지 않는다.

## Phase 1 — 분석 (항상 실행)

### 1A. 프로젝트 컨텍스트 감지
- 프로젝트 타입 감지 (Next.js App Router / Pages Router / 일반 React)
- 기존 스타일링 방식 식별 (Tailwind, CSS Modules, styled-components 등)
- 기존 UI 라이브러리 스캔 (shadcn/ui, MUI, Chakra 등)
- TypeScript 설정 확인
- 기존 컴포넌트 패턴 및 컨벤션 식별
- 기존 테스트 인프라 감지 (Playwright, Jest, Vitest 등)
- 접근성 테스트 도구 확인 (axe-core, eslint-plugin-jsx-a11y 등)

### 1B. 작업 규모 평가
현재 작업의 범위를 평가:
- **Simple**: 단일 컴포넌트 수정, 스타일 조정, 텍스트 변경 → Phase 2 건너뛰고 Phase 3로 바로 이동
- **Medium**: 2-5개 컴포넌트가 필요한 새 컴포넌트, 페이지 섹션, 기능 → Phase 2를 간단히 실행
- **Complex**: 전체 페이지, 다중 페이지 기능, 아키텍처 변경 → Phase 2 전체 실행

작업 규모 평가 출력: "작업 규모: {Simple/Medium/Complex} — {이유}"

### 1C. 기존 컴포넌트 스캔
- `.dev/frontend/components/` 카탈로그에서 재사용 가능한 컴포넌트 확인
- 프로젝트에서 확장하거나 재사용할 수 있는 유사한 기존 컴포넌트 스캔
- 이미 설치된 shadcn/ui 컴포넌트 식별
- 기존 Playwright 테스트 파일 및 설정 확인

### 1D. 디자인 방향성 평가
비주얼/UI 작업의 경우, 미학 방향성을 결정:
- 기존 프로젝트 디자인 언어와 패턴을 검토
- 프로젝트에 확립된 비주얼 아이덴티티가 있으면 → 따른다
- 새로 시작하는 경우 → 프로젝트 목적에 맞는 방향성 평가:
  - **Brutally minimal**: 강렬한 대비, 여백, 절제
  - **Maximalist chaos**: 밀도 높은, 레이어된, 압도적인 디테일
  - **Retro-futuristic**: 향수 + 현대 기술 미학
  - **Organic/natural**: 유동적인 형태, 어스톤, 부드러운 엣지
  - **Luxury/refined**: 프리미엄, 우아함, 절제
  - **Playful/toy-like**: 재미, 친근함, 입체감
  - **Editorial/magazine**: 강렬한 타이포그래피, 그리드 기반, 콘텐츠 우선
  - **Brutalist/raw**: 노출된 구조, 원시 재료, 의도적인 거칠음
  - **Art deco/geometric**: 날카로운 각도, 대칭, 메탈릭 악센트
  - **Soft/pastel**: 부드러운 그라디언트, 둥근 형태, 밝은 팔레트
  - **Industrial/utilitarian**: 기능적, 밀도 높은, 데이터 중심

## Phase 2 — 설계 (규모에 따라)

이 Phase는 Complex 작업에서는 전체 실행, Medium 작업에서는 간단히 실행, Simple 작업에서는 건너뛴다.

### 2A. 컴포넌트 아키텍처
Complex 작업:
1. UI를 컴포넌트 계층 구조로 분해
2. 컴포넌트 props 및 인터페이스 정의
3. 데이터 흐름 계획 (Server → Client 경계 결정)
4. 공유 상태 요구사항 식별
5. 라우트 구조 계획 (페이지 수준 기능의 경우)

컴포넌트 아키텍처 원칙:
- **설정보다 조합**: boolean props가 많은 모놀리식 컴포넌트가 아니라 조합 가능한 작은 컴포넌트
- **파일 콜로케이션**: 컴포넌트 관련 파일을 함께 유지 (컴포넌트, 스타일, 테스트, 스토리)
- **관심사 분리**: 데이터 페칭(Server Components)과 표현(표현형 컴포넌트) 분리
- **파일당 컴포넌트 하나**: 파일당 하나의 컴포넌트, named export

```
components/
├── {feature}/
│   ├── feature-card.tsx
│   ├── feature-list.tsx
│   ├── feature-skeleton.tsx
│   └── use-feature-data.ts
├── ui/                    (shadcn/ui 컴포넌트)
└── shared/                (공유 컴포넌트)
```

Medium 작업:
1. 메인 컴포넌트와 props 식별
2. 기존 컴포넌트 조합 가능 여부 확인

### 2B. 스타일 전략
- 필요한 Tailwind 유틸리티 클래스 결정
- 커스텀 CSS 또는 Tailwind 플러그인 필요 여부 확인
- 반응형 브레이크포인트 및 레이아웃 전략 계획
- 프로젝트에서 dark mode를 사용하는 경우 지원 계획

타이포그래피 전략:
- **독특한 디스플레이 폰트**(제목, 아이덴티티)와 **세련된 본문 폰트**(가독성) 페어링
- 엄격한 계층 확립: h1 → h2 → h3 → body → small
- 0.25rem(4px) 단위의 일관된 간격 스케일 사용

색상 전략:
- **시맨틱 토큰** 사용 (`text-primary`, `bg-surface`, `border-muted`), 원시 hex 값이 아님
- 테마 종속 값에 CSS 변수 정의
- 1-2개의 날카로운 악센트 색상이 있는 지배적 팔레트 선택
- WCAG 2.2 AA 명암비 준수

### 2C. 디자인 씽킹 프레임워크
비주얼 작업 구현 전, 다음을 정의:
1. **목적**: 이 UI가 전달하거나 달성해야 하는 것은?
2. **톤**: 어떤 감정적 레지스터? (권위 있는, 장난스러운, 차분한, 긴급한 등)
3. **제약**: 기술적 한계, 브랜드 요구사항, 접근성 요구
4. **차별화**: 이것이 제네릭 AI 출력과 다른 점은?

### 2D. 구현 계획
코딩 전에 간결한 계획을 출력:
```
## 구현 계획
- 생성할 컴포넌트: {목록}
- 수정할 컴포넌트: {목록}
- 사용할 shadcn/ui 컴포넌트: {목록}
- Server/Client 경계: {설명}
- 디자인 방향성: {미학 방향}
- 예상 규모: {Simple/Medium/Complex}
```

Complex 작업의 경우에만 사용자 확인을 기다린다. Simple/Medium은 자동으로 진행.

## Phase 3 — 구현 (항상 실행)

### 3A. 컴포넌트 생성
컴포넌트 생성 시 다음 구조를 따른다:
```tsx
// 1. 임포트 (그룹핑: react/next → 라이브러리 → 내부 → 타입)
// 2. 타입 정의 (props 인터페이스)
// 3. 컴포넌트 함수 (named export)
// 4. 서브 컴포넌트 (작은 것은 같은 파일에)
// 5. 익스포트 (named, default는 Next.js 페이지만)
```

### 3B. 스타일링 규칙
- 기본적으로 Tailwind CSS 클래스 사용
- 조건부 클래스 병합에는 `lib/utils`의 `cn()` 유틸리티 사용
- 모바일 퍼스트 접근으로 반응형 디자인 적용
- Tailwind spacing scale (0.25rem 단위)을 사용하여 일관된 간격 유지
- 테마 종속 값에는 CSS 변수 사용

**색상 구현**:
- 시맨틱 토큰 사용: `text-primary`, `bg-surface`, `border-muted`
- CSS 변수에 날카로운 악센트가 있는 지배적 색상 정의
- 명시적으로 요청하지 않는 한 제네릭 블루 (#3B82F6)를 기본으로 사용하지 않음

**타이포그래피 구현**:
- 독특한 폰트 사용, 아이덴티티 폰트로 Inter/Roboto/Ariel 사용 금지
- 엄격한 계층 유지: h1 → h2 → h3 → body → small
- 디스플레이 폰트(아이덴티티)와 본문 폰트(가독성) 페어링

**모션 & 애니메이션**:
- 정적 HTML: CSS 전용 애니메이션 (키프레임, 트랜지션)
- React 컴포넌트: CSS 트랜지션 우선; 복잡한 시퀀스에는 Motion 라이브러리 사용
- 리스트와 그리드에 `animation-delay`로 스태거드 리빌 사용
- `prefers-reduced-motion` 존중 — 항상 감소된 모션 대체 제공
- 애니메이션은 장식이 아닌 목적을 가져야 함 (주의 유도, 피드백 제공)

**공간 구성**:
- 예측 가능한 대칭을 깨기 — 비대칭, 겹침, 대각선 흐름 도입
- 여백을 기본 패딩이 아닌 의도적인 디자인 요소로 사용
- 색상뿐만 아니라 크기 대비로 시각적 계층 생성
- 모든 것을 중앙 정렬하지 않기 — 정렬로 시각적 긴장 생성

### 3C. 상태 관리
- 데이터 변경은 Server Components와 Server Actions 우선
- 필터, 페이지네이션, 공유 가능한 상태에는 URL 검색 파라미터 사용
- 클라이언트 공유 상태에만 React Context 사용
- 프로젝트에서 이미 사용 중인 경우에만 외부 상태 라이브러리 사용
- 상태 관리 결정 트리 따르기 (핵심 원칙 참조)

### 3D. 디자인-투-코드
디자인 참조(이미지, Figma, 스크린샷)에서 구현할 때:
1. 시각적 구조 분석: 레이아웃, 간격, 타이포그래피, 색상
2. 디자인 토큰을 Tailwind 클래스에 매핑
3. shadcn/ui와 일치하는 컴포넌트 패턴 식별
4. 픽셀 퍼펙트 레이아웃을 먼저 구현한 후 인터랙티비티 추가
5. 반응형 동작이 디자인 의도와 일치하는지 확인
6. 구현이 디자인의 공간적 리듬과 시각적 무게를 포착하는지 확인

### 3E. 에러 처리 및 로딩 상태
- 콘텐츠 영역에 **스켈레톤 로딩** 사용 (스피너가 아님) — 예상 콘텐츠의 레이아웃 형태와 일치
- 컴포넌트 수준 에러 처리를 위한 에러 바운더리 구현
- Next.js `loading.tsx` 및 `error.tsx` 컨벤션 사용
- 엣지 케이스에 대한 의미 있는 폴백 UI 제공
- 적절한 곳에서 낙관적 업데이트 지원 (UI 즉시 업데이트, 에러 시 복원)

## Phase 4 — 검증 (항상 실행)

### 4A. 접근성 체크 (WCAG 2.2 AA)

#### 자동화 체크 (항상 실행)
- 시맨틱 HTML 요소 올바르게 사용 (`<nav>`, `<main>`, `<section>`, `<article>`)
- 인터랙티브 요소에 ARIA 라벨
- 이미지에 의미 있는 `alt` 텍스트
- 폼 입력에 연결된 라벨
- 색상 대비가 WCAG 2.2 AA 기준 충족 (일반 텍스트 4.5:1, 큰 텍스트 3:1)
- 포커스 인디케이터가 보임 (최소 2px solid outline 또는 동등한 것)

#### 인터랙티브 체크 (Complex 작업)
- 키보드 탐색 동작 (탭 순서, 포커스 관리)
- 모달 및 다이얼로그에서 포커스 트랩이 올바르게 작동
- 페이지에 콘텐츠 건너뛰기 링크가 있음
- 모든 인터랙티브 요소가 키보드로 접근 가능

#### 자동화 테스트 스크립트 (Playwright 설정 시)
프로젝트에 Playwright가 설정된 경우, 접근성 테스트 스크립트를 생성:

**포커스 인디케이터 테스트** (WCAG 2.4.7, 2.4.12, 3.2.1):
```ts
import { test, expect } from '@playwright/test';

test('포커스 인디케이터가 보임', async ({ page }) => {
  await page.goto('/');
  const focusable = page.locator('a, button, input, select, textarea, [tabindex]');
  const count = await focusable.count();
  for (let i = 0; i < Math.min(count, 20); i++) {
    await focusable.nth(i).focus();
    const outline = await focusable.nth(i).evaluate(el =>
      getComputedStyle(el).outline + getComputedStyle(el).boxShadow
    );
    expect(outline).not.toBe('nonenone');
  }
});
```

**320px 리플로우 테스트** (WCAG 1.4.10):
```ts
test('320px에서 가로 스크롤 없이 콘텐츠 리플로우', async ({ page }) => {
  await page.setViewportSize({ width: 320, height: 568 });
  await page.goto('/');
  const scrollWidth = await page.evaluate(() => document.documentElement.scrollWidth);
  expect(scrollWidth).toBeLessThanOrEqual(320);
});
```

**텍스트 간격 테스트** (WCAG 1.4.12):
```ts
test('간격이 증가해도 텍스트 가독성 유지', async ({ page }) => {
  await page.goto('/');
  await page.addStyleTag({ content: `
    * { line-height: 1.5 !important; letter-spacing: 0.12em !important;
        word-spacing: 0.16em !important; }
    p, li, h1, h2, h3 { margin-bottom: 2em !important; }
  `});
  const overflows = await page.evaluate(() =>
    Array.from(document.querySelectorAll('p, li, h1, h2, h3, span')).filter(el =>
      el.scrollWidth > el.clientWidth + 2
    ).length
  );
  expect(overflows).toBe(0);
});
```

**터치 타겟 크기 테스트** (WCAG 2.5.5, 2.5.8):
```ts
test('터치 타겟이 최소 크기를 충족', async ({ page }) => {
  await page.goto('/');
  const targets = page.locator('a, button, input[type="submit"], input[type="button"]');
  const count = await targets.count();
  for (let i = 0; i < Math.min(count, 30); i++) {
    const box = await targets.nth(i).boundingBox();
    if (box) {
      expect(box.width).toBeGreaterThanOrEqual(44);
      expect(box.height).toBeGreaterThanOrEqual(44);
    }
  }
});
```

### 4B. 반응형 디자인 체크
- 모바일(320px), 태블릿(768px), 데스크탑(1024px+), 울트라와이드(1440px)에서 레이아웃 동작
- 모바일에서 터치 타겟이 최소 44x44px
- 가로 스크롤 없이 텍스트 읽기 가능
- 화면 크기에 맞게 네비게이션 적응
- 이미지 및 미디어 반응형

### 4C. 성능 체크

#### Core Web Vitals 측정
| 항목 | 양호 | 개선 필요 | 불량 |
|------|------|-----------|------|
| LCP | ≤ 2.5s | 2.5s – 4.0s | > 4.0s |
| INP | ≤ 200ms | 200ms – 500ms | > 500ms |
| CLS | ≤ 0.1 | 0.1 – 0.25 | > 0.25 |

#### 성능 검증 워크플로우
1. **측정**: Core Web Vitals 및 번들 사이즈 확인
2. **식별**: 증상 기반 진단:
   - 느린 LCP → 이미지 최적화, 폰트 로딩, 서버 응답 시간 확인
   - 높은 INP → 이벤트 핸들러, 불필요한 리렌더, 무거운 연산 확인
   - 높은 CLS → 사이즈 없는 이미지/폰트, 동적 콘텐츠 주입, 늦게 로딩되는 광고 확인
3. **수정**: 타겟팅된 최적화 적용
4. **검증**: 개선 확인을 위해 다시 측정
5. **방어**: CI에서 회귀 방지

#### 확인할 일반적인 안티패턴
- **N+1 데이터 페칭**: 일괄/병렬 대신 여러 순차 요청
- **무제한 데이터 페칭**: API 응답에 페이지네이션 또는 제한 누락
- **이미지 최적화 누락**: `next/image` 미사용, 잘못된 사이즈, lazy loading 없음
- **불필요한 클라이언트 JS**: Server Component로 충분한데 `"use client"` 사용
- **불필요한 리렌더**: 비용이 큰 연산에 메모이제이션 누락
- **큰 번들**: 특정 모듈 대신 전체 라이브러리 임포트
- **로딩 상태 누락**: 비동기 콘텐츠에 스켈레톤/스피너 없음 → 레이아웃 시프트
- **렌더 차단 리소스**: 크리티컬 패스에 동기 스크립트/스타일시트

#### 성능 안티패턴 코드 예시

**Bad: N+1 페칭**
```tsx
// 각 자식이 독립적으로 페칭
items.map(item => <ItemDetail id={item.id} />) // 각각 자체 페치 수행
```
**Good: 일괄 페칭**
```tsx
const allDetails = await fetchDetails(items.map(i => i.id))
items.map(item => <ItemDetail detail={allDetails[item.id]} />)
```

**Bad: 사이즈 없는 이미지 (CLS 발생)**
```tsx
<img src="/hero.jpg" alt="Hero" />
```
**Good: next/image로 사이즈 지정**
```tsx
<Image src="/hero.jpg" alt="Hero" width={1200} height={630} priority />
```

**Bad: 전체 라이브러리 임포트**
```tsx
import { debounce } from 'lodash' // lodash 전체를 임포트
```
**Good: 특정 임포트**
```tsx
import debounce from 'lodash/debounce'
```

### 4D. 브라우저 테스트 (해당 시)

Complex 작업이나 사용자가 시각적 검증을 요청할 때, Playwright 기반 브라우저 테스트를 사용.

#### 테스트 결정 트리
- 정적 HTML(JS 불필요)인가? → 직접 HTML 검사 사용
- 동적 웹앱인가? → Playwright 사용
  - 서버가 이미 실행 중인가? → 직접 연결
  - 서버가 없는가? → 서버 라이프사이클 관리로 시작/중지

#### 정찰-후-액션 패턴
1. 대상 페이지로 **이동**
2. `networkidle`(또는 특정 요소)까지 **대기**
3. 현재 상태 **스크린샷**
4. 올바른 셀렉터를 식별하기 위해 **DOM 검사**
5. 설명적 셀렉터를 사용하여 **액션 실행**
6. 시각적, 구조적으로 결과 **검증**

#### 모범 사례
- Playwright를 동기 모드(`sync_playwright()`)로 사용
- 설명적 셀렉터 우선: `get_by_role()`, `get_by_label()`, `get_by_test_id()`
- 시각적 비교를 위해 액션 전후 스크린샷 캡처
- 상태 어설션 전 network idle 대기
- 서버 라이프사이클 자동 관리 — 테스트 전 시작, 후 중지

### 4E. 자동 수정
Phase 4 체크에서 문제가 발견되면:
1. 접근성 문제는 즉시 수정 — 허락을 구하지 않는다
2. 반응형 레이아웃 문제는 즉시 수정 — 허락을 구하지 않는다
3. 성능 안티패턴 즉시 수정 (사이즈 지정 이미지, 번들 임포트, 스켈레톤 누락)
4. 아키텍처적 성능 우려는 구체적인 제안과 함께 사용자에게 알림
5. 검증된 컴포넌트에 Playwright 테스트 파일 생성 (Complex 작업)

### 4F. 카탈로그 업데이트
구현 완료 후 `.dev/frontend/components/` 업데이트:
- 새 컴포넌트 항목 추가: 이름, 경로, props 요약, 의존성
- 수정된 컴포넌트 항목 업데이트
- 삭제된 컴포넌트 항목 제거

## 파일 구조 컨벤션

### 권장 Next.js App Router 구조
```
src/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── {route}/
│   │   ├── page.tsx
│   │   ├── layout.tsx
│   │   ├── loading.tsx
│   │   └── error.tsx
│   └── globals.css
├── components/
│   ├── ui/           (shadcn/ui 컴포넌트)
│   ├── {feature}/    (기능별, 콜로케이션)
│   └── shared/       (공유 컴포넌트)
├── lib/
│   ├── utils.ts      (cn() 및 유틸리티)
│   └── ...
├── hooks/
├── types/
├── styles/
└── tests/
    └── e2e/          (Playwright 테스트)
```

## 상태 관리 (`.dev/frontend/styles.md`)
- 프로젝트의 스타일 가이드 및 컨벤션 추적
- 새로운 패턴이 확립되면 자동으로 업데이트
- 일관된 스타일링 결정을 위한 참조 자료로 활용
