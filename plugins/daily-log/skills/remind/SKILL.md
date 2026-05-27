---
name: remind
description: "(daily-log) 최근 로그에서 미완료 Remind 항목을 표시한다. 현재 프로젝트의 이어서 할 작업 목록을 보여준다. Use when: user says '리마인드', 'remind', '할 일 확인', '어제 뭐 남았지', '이어서 할 거', '미완료'. Do NOT use when: user wants to write a new log (use log), or initial setup (use init)."
---

# daily-log — remind

최근 일일업무일지에서 미완료 Remind 항목을 찾아 표시한다.

> **Language**: Always respond in **Korean (한국어)**.

---

## Pre-check

1. `~/.daily-log/config.json` 존재 확인. 없으면:
   ```
   daily-log이 아직 설정되지 않았습니다.
   `/daily-log:init`을 먼저 실행해주세요.
   ```
2. config를 읽어 `local_path`, `default_branch` 확인

---

## Phase 1 — Tasks 레포 동기화

```bash
cd {local_path} && git pull origin {default_branch}
```

- pull 실패 시 경고하되 로컬 파일로 계속 진행

---

## Phase 2 — 프로젝트명 감지

1. 현재 작업 디렉토리의 `CLAUDE.md`를 읽어 프로젝트명 추출
2. CLAUDE.md가 없거나 추출 불가 시 사용자에게 질문
3. kebab-case로 정규화

(log 스킬의 Phase 2와 동일한 로직)

---

## Phase 3 — 미완료 항목 검색

1. tasks 레포에서 최근 로그 파일을 역순으로 탐색 (오늘 → 과거, 최대 7일):
   ```bash
   ls -r {local_path}/{YYYY}/{MM}/{YYYY-MM-DD}.md
   ```
   - 현재 월에서 시작, 월 경계를 넘는 경우 이전 월 폴더도 탐색

2. 각 파일에서 현재 프로젝트의 Remind 섹션을 파싱:
   - `## {project-name}` 섹션 찾기
   - 해당 섹션 내 `### Remind` 하위의 `- [ ]` (미완료) 항목 추출
   - `- [x]` (완료) 항목은 제외

3. 미완료 항목을 찾은 첫 번째 파일에서 정지 (가장 최근 기록 기준)

---

## Phase 4 — 결과 표시

**미완료 항목이 있는 경우:**

```
## 미완료 리마인드

{YYYY-MM-DD} ({요일}) 기록 기준 — {project-name}

- [ ] {항목 1}
- [ ] {항목 2}
- [ ] {항목 3}

{N}개의 미완료 항목이 있습니다.
이어서 작업을 시작하세요!
```

**미완료 항목이 없는 경우:**

```
## 미완료 리마인드

최근 7일간 {project-name} 프로젝트의 미완료 항목이 없습니다.
모든 작업이 완료된 상태입니다!
```

**로그 파일이 아예 없는 경우:**

```
## 미완료 리마인드

아직 기록된 로그가 없습니다.
`/daily-log:log`로 첫 로그를 기록해보세요.
```

---

## Constraints

- **NEVER modify any files** — remind는 읽기 전용, 어떤 파일도 수정하지 않는다
- **NEVER push or commit** — 읽기만 수행
- **ALWAYS respond in Korean**
- **ALWAYS show the source date** — 어느 날짜의 로그에서 가져온 항목인지 명시
