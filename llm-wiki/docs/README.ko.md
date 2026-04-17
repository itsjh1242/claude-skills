# llm-wiki

Karpathy 스타일 LLM Wiki + Graphify 지식 베이스를 프로젝트에 설치하는 스킬.

---

## 기능 설명

프로젝트에 구조화된 개발 지식 베이스를 설치한다:

- **Raw Logs** — 기능 단위 개발 로그 (목표, 결정, 문제, 해결 기록)
- **Raw Docs** — 설계서, 기능 정의서, 아키텍처 문서 등 개발 문서
- **Wiki Pages** — Raw 데이터를 교차 분석하여 도출한 패턴, 의사결정, 인사이트
- **Knowledge Graph** — 위키 전체를 연결하는 JSON 기반 그래프 (Graphify)
- **Operations** — Log, Doc, Ingest, Graphify, Query, Lint — 셋업 후 CLAUDE.md 규칙이 자동 운영

---

## 설치

다음 프롬프트를 Claude Code에 붙여넣으면 llm-wiki가 설치된다:

```
Install the llm-wiki skill by following the setup instructions here:
https://raw.githubusercontent.com/itsjh1242/claude-skills/main/llm-wiki/SKILL.md
```

Claude가 SKILL.md를 읽고 필요한 디렉토리 생성, 스킬 설치, `CLAUDE.md` 업데이트를 자동으로 수행한다.

**이미 설치했다면?** 동일한 프롬프트를 다시 붙여넣으면 된다 — 버전을 비교해서 새 버전이 있을 때만 업데이트된다.

---

## 설치 후 생성되는 파일 구조

```
{project-root}/
├── .claude/
│   └── skills/
│       └── llm-wiki/
│           └── SKILL.md
├── .dev/
│   └── wiki/
│       ├── raw/
│       │   ├── logs/           # 기능 단위 개발 로그
│       │   └── docs/           # 개발 문서
│       ├── wiki/
│       │   ├── patterns/       # 반복되는 개발 패턴
│       │   ├── decisions/      # 설계·구현 의사결정
│       │   └── insights/       # 교차 분석 인사이트
│       ├── graphify-out/       # Knowledge Graph 출력
│       ├── index.md            # 전체 페이지 목록
│       └── log.md              # Operation 이력
└── CLAUDE.md                   ← LLM Wiki 섹션 추가됨
```
