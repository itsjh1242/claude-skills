# dev-starter

한 번에 전체 개발 환경 셋업: **LLM Wiki** (Karpathy-style 지식 베이스) + **Claude Harness** (에이전트 팀 & 스킬 아키텍트) 통합 설치.

---

## 기능 설명

- **LLM Wiki**: 구조화된 지식 베이스 (`.dev/wiki/`) 구성 — 개발 로그, 문서, 위키 페이지, 지식 그래프(Graphify). `CLAUDE.md`에 위키 운영 규칙을 추가해서 Claude가 지식 캡처를 자동으로 관리한다.
- **Claude Harness**: [revfactory/harness](https://github.com/revfactory/harness)를 fetch해서 설치 — 멀티 에이전트 시스템을 설계하고 스캐폴딩하는 메타 스킬. `CLAUDE.md`에 harness 트리거 추가.

두 스킬을 한 번에 설치하며, 각각의 버전을 독립적으로 추적한다.

---

## 설치

다음 프롬프트를 Claude Code에 붙여넣으면 dev-starter가 설치된다:

```
Install the dev-starter skill by following the setup instructions here:
https://raw.githubusercontent.com/itsjh1242/claude-skills/main/dev-starter/SKILL.md
```

Claude가 LLM Wiki와 Claude Harness를 모두 설치하고, `CLAUDE.md`를 업데이트하며, 각 컴포넌트의 상태를 보고한다.

**이미 설치했다면?** 동일한 프롬프트를 다시 붙여넣으면 된다 — 각 컴포넌트를 독립적으로 버전 확인하고 새 버전이 있을 때만 업데이트된다.

---

## 설치 후 생성되는 파일 구조

```
{project-root}/
├── .dev/
│   └── wiki/
│       ├── raw/logs/         ← 기능별 개발 로그
│       ├── raw/docs/         ← 설계 스펙, 아키텍처 문서
│       ├── wiki/patterns/    ← 반복 패턴
│       ├── wiki/decisions/   ← 설계 의사결정
│       ├── wiki/insights/    ← 교차 분석 인사이트
│       ├── graphify-out/     ← 지식 그래프
│       ├── index.md
│       └── log.md
├── .claude/
│   ├── agents/               ← harness가 채운다
│   └── skills/
│       ├── llm-wiki/
│       │   └── SKILL.md
│       ├── harness/
│       │   ├── SKILL.md
│       │   └── references/   ← harness 참조 파일 6개
│       ├── harness-setup/
│       │   └── SKILL.md
│       └── dev-starter/
│           └── SKILL.md
└── CLAUDE.md                 ← # LLM Wiki + # Harness 섹션 추가됨
```

---

## 설치 후 사용법

**LLM Wiki** — Claude가 완료된 기능 로깅을 제안하고, 위키 운영 명령에 응답한다:
- 개발 로그 기록 → `wiki log`
- 설계 문서 저장 → `wiki doc`
- 로그에서 위키 페이지 생성 → `wiki ingest`
- 지식 그래프 빌드 → `wiki graphify`

**Harness** — Claude가 에이전트 팀 설계 요청에 응답한다:
- `하네스 구성해줘` / `build a harness for this project`
- `이 도메인에 에이전트 팀 설계해줘`

---

## 개별 설치

하나만 설치하고 싶다면?

- **LLM Wiki만**: [llm-wiki](../../llm-wiki/README.md)
- **Harness만**: [harness-setup](../../harness-setup/README.md)
