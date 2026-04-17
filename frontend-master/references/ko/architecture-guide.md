# 아키텍처 가이드

컴포넌트 아키텍처를 설계하거나 구조적 결정을 수행할 때 Frontend Master가 로드합니다.

## 컴포넌트 아키텍처 원칙

### 설정보다 조합
함께 조합 가능한 작은 집중 컴포넌트를 구축 — boolean props로 모든 것을 제어하는 모놀리식 컴포넌트가 아님.

**Bad**: `showIcon`, `showBadge`, `variant`, `size` props로 모든 것을 제어하는 하나의 컴포넌트
**Good**: `<Card>`, `<CardHeader>`, `<CardBadge>`, `<CardIcon>` — 필요에 따라 조합

### 파일 콜로케이션
컴포넌트 관련 파일을 함께 유지:

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

### 관심사 분리
- **데이터 페칭**: Server Components — 경계에서 페치하고 props로 전달
- **표현**: 표현형 컴포넌트 — 데이터를 받고 UI를 렌더링
- **인터랙티비티**: Client Components (`"use client"`) — 필요할 때만 (이벤트 핸들러, 훅, 브라우저 API)

### 파일당 컴포넌트 하나
파일당 하나의 컴포넌트, named export. default export는 Next.js 페이지에만 사용.

## 상태 관리 결정 트리

다음 우선순위를 따른다:

1. **로컬 상태** (`useState`) — 컴포넌트 범위 UI 상태
2. **끌어올린 상태** — 부모와 자식 간 공유
3. **URL 상태** — 필터, 페이지네이션, 공유 가능한 상태 (`useSearchParams`)
4. **React Context** — 컴포넌트 간 클라이언트 상태
5. **서버 상태** — Server Components / Server Actions로 데이터 페칭
6. **외부 스토어** — 프로젝트가 이미 사용 중인 경우에만 (Zustand, Jotai 등)

프로젝트가 이미 의존하지 않는 한 외부 상태 라이브러리를 추가하지 않는다.

## Server/Client 경계

기본적으로 Server Components 사용. 다음 경우에만 `"use client"` 추가:
- 이벤트 핸들러가 필요할 때 (`onClick`, `onChange` 등)
- React 훅을 사용할 때 (`useState`, `useEffect` 등)
- 브라우저 전용 API가 필요할 때 (`window`, `document` 등)
- 서드파티 클라이언트 사이드 라이브러리를 사용할 때

클라이언트 경계를 트리의 아래쪽으로 유지 — 인터랙티브 부분만 감싼다.

## 로딩 및 에러 패턴

- **스켈레톤 로딩** 사용 (스피너가 아님) — 예상 콘텐츠의 레이아웃 형태와 일치
- 적절한 곳에서 **낙관적 업데이트** 지원 (UI 즉시 업데이트, 에러 시 복원)
- Next.js `loading.tsx` 및 `error.tsx` 컨벤션 사용
- 엣지 케이스에 대한 의미 있는 폴백 UI 제공
