# frontend-master

React + Next.js 프로젝트에서 모든 프론트엔드 작업을 완전히 담당하는 종합 프론트엔드 개발 스킬.

---

## 기능 설명

- **자동 감지**: 프로젝트 기술 스택을 감지하고 그에 맞게 동작
- **분석**: 작업 복잡도에 따라 워크플로우 자동 조정 (간단한 작업은 설계 단계 스킵, 복잡한 작업은 전체 아키텍처 설계)
- **구현**: Next.js App Router 모범 사례와 Server Components를 따르는 컴포넌트 구현
- **스타일링**: Tailwind CSS + shadcn/ui를 기본으로 사용
- **디자인 구현**: 디자인 참조(이미지, Figma)를 픽셀 퍼펙트 코드로 변환
- **검증**: 접근성(WCAG AA), 반응형 디자인, 성능을 자동으로 체크
- **자동 수정**: 접근성 및 반응형 문제를 묻지 않고 바로 수정

### 기술 스택
- React + Next.js (App Router)
- TypeScript (strict)
- Tailwind CSS + shadcn/ui
- 기본적으로 Server Components

### 4단계 Phase
| Phase | 이름 | 실행 시기 |
|-------|------|-----------|
| 1 | 분석 | 항상 — 프로젝트 컨텍스트, 작업 규모, 기존 컴포넌트 |
| 2 | 설계 | 규모에 따라 — 컴포넌트 아키텍처, 스타일 전략, 구현 계획 |
| 3 | 구현 | 항상 — 컴포넌트 생성, 스타일링, 상태 관리, 에러 처리 |
| 4 | 검증 | 항상 — 접근성, 반응형, 성능 체크 및 자동 수정 |

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
│           └── SKILL.md
├── .dev/
│   └── frontend/
│       ├── components/     (컴포넌트 카탈로그)
│       └── styles.md       (스타일 가이드)
└── CLAUDE.md               ← Frontend Master 섹션 추가됨
```
