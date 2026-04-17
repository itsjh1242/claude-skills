# LLM Wiki — Setup Skill

Karpathy-style LLM Wiki + Graphify 지식 베이스를 프로젝트에 설치합니다.
이 스킬은 **셋업 전용**입니다. 셋업 완료 후에는 CLAUDE.md에 등록된 운영 규칙이 매 대화마다 자동으로 적용됩니다.

---

## Step 1 — 디렉토리 구조 생성

프로젝트 루트에 다음 경로를 생성합니다:

```
.dev/wiki/
├── raw/
│   ├── logs/                   # 기능 단위 개발 로그
│   └── docs/                   # 개발 문서 (설계서, 기능 정의서 등)
├── wiki/
│   ├── patterns/               # 반복되는 개발 패턴
│   ├── decisions/              # 설계·구현 의사결정 기록
│   └── insights/               # 교차 분석 인사이트
├── graphify-out/               # Knowledge Graph 출력
├── index.md
└── log.md
```

## Step 2 — index.md 초기화

`.dev/wiki/index.md`:

```markdown
# Wiki Index

> 마지막 업데이트: {오늘 날짜}

## Raw — Dev Logs

| 파일 | 기능 | 상태 | 생성일 |
|------|------|------|--------|

## Raw — Dev Docs

| 파일 | 제목 | 유형 | 생성일 |
|------|------|------|--------|

## Patterns

| 파일 | 주제 | 마지막 업데이트 |
|------|------|-----------------|

## Decisions

| 파일 | 주제 | 마지막 업데이트 |
|------|------|-----------------|

## Insights

| 파일 | 주제 | 마지막 업데이트 |
|------|------|-----------------|
```

## Step 3 — log.md 초기화

`.dev/wiki/log.md`:

```markdown
# Wiki Log

Operation 이력을 시간순으로 기록합니다.
형식: `## [{YYYY-MM-DD}] {operation} | {대상 또는 결과 요약}`

---

## [{오늘 날짜}] init | 위키 구조 초기화

- 디렉토리 구조 생성 완료
- index.md, log.md 초기화
```

## Step 4 — CLAUDE.md에 운영 규칙 등록

CLAUDE.md에 아래 내용을 추가합니다. 기존에 LLM Wiki 관련 섹션이 있으면 교체합니다.

````markdown
# LLM Wiki

Karpathy-style LLM Wiki + Graphify 개발 지식 베이스.
Wiki root: `.dev/wiki/`

## 구조

```
.dev/wiki/
├── raw/logs/{slug}.md          # 기능 단위 개발 로그 (불변)
├── raw/docs/{slug}.md          # 개발 문서 — 설계서, 기능 정의서 등 (불변)
├── wiki/patterns/{topic}.md    # 반복되는 개발 패턴
├── wiki/decisions/{topic}.md   # 설계·구현 의사결정
├── wiki/insights/{topic}.md    # 교차 분석 인사이트
├── graphify-out/graph.json     # Knowledge Graph
├── index.md                    # 전체 페이지 목록 (진입점)
└── log.md                      # Operation 이력
```

## Operations

### Log — 개발 로그 기록

기능 단위 작업(구현, 버그픽스, 리팩토링 등)이 완결되었다고 판단될 때 자연스럽게 제안한다.

`.dev/wiki/raw/logs/{feature-slug}.md`:

```markdown
---
feature: {기능명}
slug: {feature-slug}
type: {feature | bugfix | refactor | config | docs}
status: {done | wip}
created: {YYYY-MM-DD}
updated: {YYYY-MM-DD}
files_changed:
  - {변경된 파일 경로}
tags: [{키워드}]
---

# {기능명}

## 목표
{해결하려는 문제 또는 달성 목표 — 1~3문장}

## 구현 요약
{무엇을 어떻게 — 코드 구조, 접근 방식}

## 핵심 결정
- **결정**: {A vs B — A를 선택한 이유}

## 문제와 해결
- **문제**: {증상} → **원인**: {근본 원인} → **해결**: {해결책}

## 회고 (optional)
{다시 한다면 다르게 할 점}
```

- slug: kebab-case 영문
- 같은 slug 파일 존재 시 기존 파일에 append
- `status: wip` → 작업 계속 시 업데이트, 완료 시 `done`

### Doc — 개발 문서 저장

설계서, 기능 정의서, 아키텍처 문서 등 개발 문서를 작성하거나 정리할 때 저장한다.

`.dev/wiki/raw/docs/{doc-slug}.md`:

```markdown
---
title: {문서 제목}
slug: {doc-slug}
doc_type: {design | spec | architecture | runbook | guide | adr}
status: {draft | final | deprecated}
created: {YYYY-MM-DD}
updated: {YYYY-MM-DD}
related_features: [{관련 feature slug}]
tags: [{키워드}]
---

# {문서 제목}

{본문}
```

- 본문 형식은 doc_type에 따라 자유
- `related_features`로 개발 로그와 연결

### Ingest — 위키 페이지 생성/업데이트

raw 로그 + raw 문서를 교차 분석하여 위키 페이지를 생성·업데이트한다.

1. `index.md` 읽어 기존 위키 현황 파악
2. `status: done`(로그) 또는 `status: final`(문서) 중 미반영 항목 식별
3. 세 종류 위키 페이지 생성:

**patterns/{topic}.md — 반복되는 개발 패턴**:
문의 유형이 아닌 *개발 과정*에서 반복 관찰되는 접근 방식·문제·해결법.
사례 테이블에 `[[raw/logs/{slug}]]` 또는 `[[raw/docs/{slug}]]`로 출처 연결.

**decisions/{topic}.md — 설계·구현 의사결정**:
맥락, 선택지(장단점 비교), 최종 결정과 근거.
관련 로그·문서 역링크.

**insights/{topic}.md — 교차 분석 인사이트**:
여러 로그·문서를 교차하여 발견한 비자명한 인사이트.
근거 데이터, 시사점, 관련 페이지 링크.

4. `index.md` · `log.md` 갱신

### Graphify — Knowledge Graph 빌드

위키 전체를 `.dev/wiki/graphify-out/graph.json`으로 구조화한다.

노드 타입: `feature`, `document`, `pattern`, `decision`, `insight`, `file`, `tag`
엣지 타입: `implements`, `decided_in`, `modifies`, `related_to`, `supersedes`, `discovered_from`, `documents`, `tagged`
하이퍼엣지: 여러 노드가 하나의 흐름에 참여하는 경우

### Query — 지식 조회

새 작업 시작 시 또는 요청 시, index.md와 graph.json에서 관련 지식을 탐색하여 제공한다.
- 이전에 비슷한 영역에서 발견된 패턴
- 관련 의사결정·설계 문서 이력
- 주의할 인사이트

### Lint — 위키 건강 점검

고아 페이지, 깨진 위키링크, 미반영 raw, 오래된 wip, 그래프 커버리지를 점검한다.
결과를 log.md에 Grade A~E로 기록.

## 제약

- `raw/` 파일은 status 필드 변경과 append만 허용, 기존 내용 수정·삭제 금지
- 페이지 생성·삭제 시 반드시 `index.md` 함께 갱신
- 모든 Operation은 `log.md`에 기록
- 위키링크: `[[경로/파일명]]` (확장자 없이)
- slug: kebab-case 영문
````

## Step 5 — 검증

- [ ] `.dev/wiki/raw/logs/` 디렉토리 존재
- [ ] `.dev/wiki/raw/docs/` 디렉토리 존재
- [ ] `.dev/wiki/wiki/patterns/` 디렉토리 존재
- [ ] `.dev/wiki/wiki/decisions/` 디렉토리 존재
- [ ] `.dev/wiki/wiki/insights/` 디렉토리 존재
- [ ] `.dev/wiki/graphify-out/` 디렉토리 존재
- [ ] `.dev/wiki/index.md` 존재
- [ ] `.dev/wiki/log.md` 존재
- [ ] `CLAUDE.md`에 `# LLM Wiki` 섹션 존재

모두 통과하면 "LLM Wiki setup complete" 출력.
