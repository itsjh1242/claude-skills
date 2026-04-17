# frontend-master

React + Next.js 프로젝트에서 모든 프론트엔드 작업을 완전히 담당하는 종합 프론트엔드 개발 스킬.

---

## 기능 설명

- **자동 감지**: 프로젝트 기술 스택을 감지하고 그에 맞게 동작
- **분석**: 작업 복잡도에 따라 워크플로우 자동 조정 (간단한 작업은 설계 단계 스킵, 복잡한 작업은 전체 아키텍처 설계)
- **설계**: 디자인 씽킹 프레임워크 적용 — 목적, 톤, 제약, 차별화
- **구현**: Next.js App Router 모범 사례와 Server Components를 따르는 컴포넌트 구현
- **스타일링**: Tailwind CSS + shadcn/ui를 기본으로 사용, Anti-AI 미학 규칙 준수
- **디자인 구현**: 디자인 참조(이미지, Figma)를 픽셀 퍼펙트 코드로 변환
- **검증**: 접근성(WCAG AA), 반응형 디자인, Core Web Vitals 자동 체크
- **자동 수정**: 접근성, 반응형 문제를 묻지 않고 바로 수정

### 기술 스택
- React + Next.js (App Router)
- TypeScript (strict)
- Tailwind CSS + shadcn/ui
- 기본적으로 Server Components

### 4단계 Phase
| Phase | 이름 | 실행 시기 |
|-------|------|-----------|
| 1 | 분석 | 항상 — 프로젝트 컨텍스트, 작업 규모, 기존 컴포넌트 |
| 2 | 설계 | 규모에 따라 — 디자인 씽킹, 컴포넌트 아키텍처, 구현 계획 |
| 3 | 구현 | 항상 — 컴포넌트 생성, 스타일링, 상태 관리, 디자인-투-코드 |
| 4 | 검증 | 항상 — 접근성, 반응형, 성능, 자동 수정 |

### v2.0 하이라이트
- **라우팅 테이블 패턴**: SKILL.md를 경량 라우터로 사용 (~200줄), 상세 가이드는 `references/` 폴더에 온디맨드 로딩으로 토큰 비용 최소화
- **디자인 미학**: 디자인 씽킹 프레임워크, 11가지 미학 방향성, Anti-AI-slop 규칙, 타이포그래피/색상/모션/공간 가이드라인
- **컴포넌트 아키텍처**: 설정보다 조합, 파일 콜로케이션, 상태 관리 결정 트리
- **성능 목표**: LCP ≤2.5s, INP ≤200ms, CLS ≤0.1

---

## 설치

다음 프롬프트를 Claude Code에 붙여넣으면 frontend-master가 설치된다:

```
Install the frontend-master skill by following the setup instructions here:
https://raw.githubusercontent.com/itsjh1242/claude-skills/main/frontend-master/SKILL.md
```

Claude가 SKILL.md를 읽고 필요한 디렉토리 생성, 스킬 설치, `CLAUDE.md` 업데이트를 자동으로 수행한다.

**이미 설치했다면?** 동일한 프롬프트를 다시 붙여넣으면 된다 — 버전을 비교해서 새 버전이 있을 때만 업데이트된다.

---

## 설치 후 생성되는 파일 구조

```
{project-root}/
├── .claude/
│   └── skills/
│       └── frontend-master/
│           ├── SKILL.md
│           └── references/
│               ├── design-guide.md       (디자인 미학, 온디맨드 로딩)
│               └── architecture-guide.md (컴포넌트 패턴, 온디맨드 로딩)
├── .dev/
│   └── frontend/
│       ├── components/     (컴포넌트 카탈로그)
│       └── styles.md       (스타일 가이드)
└── CLAUDE.md               ← Frontend Master 섹션 추가됨
```
