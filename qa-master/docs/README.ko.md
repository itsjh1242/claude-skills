# qa-master

기능 구현이 완료되면 자동으로 품질을 평가하는 Claude Code 스킬.

슬래시 커맨드로 실행되는 방식이 아니라, 대화 맥락과 코드 변화를 스스로 읽고 기능이 완성됐다고 판단되면 자연스럽게 QA를 제안한다.

---

## 기능 설명

### Phase 2A — 기능 성능 평가
코드가 오류 없이 의도대로 동작하는지 점검:
- 버그 및 런타임 오류 가능성
- 엣지케이스 처리 여부
- 예외 상황에서의 동작

각 항목은 **PASS / FAIL / WARN** + 근거로 평가된다.

### Phase 2B — 품질 성능 평가
기능의 결과물이 얼마나 정확한지 점검.

CLI가 프로젝트 유형과 기능 목적을 분석해서 **평가 기준을 직접 설계**하고, 수치 기반 A~E 등급 기준으로 테스트 시나리오를 생성·실행해 최종 등급을 산출한다.

`QA_MODE` 환경변수로 **dev / prod 모드**를 지원:

| 모드 | 일반 항목 | 핵심(Core) 항목 |
|------|-----------|----------------|
| `dev` (기본값) | 전 항목 C등급 이상 | 핵심 항목 B등급 이상 |
| `prod` | 전 항목 B등급 이상 | 핵심 항목 A등급 이상 |

### 리포트
QA 결과는 `.dev/qa/reports/` 아래에 마크다운 파일로 저장된다. 기능이 수정되면 리포트가 최신화되고, 삭제되면 리포트도 함께 삭제된다.

---

## 설치

다음 프롬프트를 Claude Code에 붙여넣으면 qa-master가 설치된다:

```
Install the qa-master skill by following the setup instructions here:
https://raw.githubusercontent.com/itsjh1242/claude-skills/main/qa-master/SKILL.md
```

Claude가 SKILL.md를 읽고 필요한 디렉토리 생성, 스킬 설치, `CLAUDE.md` 업데이트를 자동으로 수행한다.

**이미 설치했다면?** 동일한 프롬프트를 다시 붙여넣으면 된다 — 버전을 비교해서 새 버전이 있을 때만 업데이트된다.

---

## 설치 후 생성되는 파일 구조

```
{project-root}/
├── .claude/
│   └── skills/
│       └── qa-master/
│           └── SKILL.md
├── .dev/
│   └── qa/
│       ├── pending.md
│       └── reports/
│           └── {feature-name}.md
└── CLAUDE.md          ← QA Master 섹션 추가됨
```
