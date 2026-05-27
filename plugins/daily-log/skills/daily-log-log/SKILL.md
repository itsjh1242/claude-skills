---
name: daily-log-log
description: "오늘의 작업 로그를 기록한다. git log를 자동 분석하여 Completed 항목을 생성하고, Remind 항목을 사용자에게 확인받은 후, tasks 레포의 일일 마크다운 파일에 기록/커밋/푸시한다. Use when: user says '오늘 로그', 'daily log', '작업 기록', '업무 정리', '일일업무', 'log 기록'. Do NOT use when: user wants to read reminders (use remind), or initial setup (use init)."
---

# daily-log — log

현재 프로젝트의 오늘 작업을 분석하여 tasks 레포에 일일업무일지를 기록한다.

> **Language**: Always respond in **Korean (한국어)**.

---

## Pre-check

1. `~/.daily-log/config.json` 존재 확인. 없으면:
   ```
   daily-log이 아직 설정되지 않았습니다.
   `/daily-log:daily-log-init`을 먼저 실행해주세요.
   ```
2. config를 읽어 `local_path`, `repo_url`, `default_branch` 확인
3. `local_path` 디렉토리가 유효한 git repo인지 확인. 아니면 재클론 시도

---

## Phase 1 — Tasks 레포 동기화

```bash
cd {local_path} && git pull origin {default_branch}
```

- pull 실패 시 사용자에게 경고하되, 로컬 파일로 계속 진행 가능

---

## Phase 2 — 프로젝트명 감지

1. 현재 작업 디렉토리의 `CLAUDE.md` 파일을 읽는다
2. CLAUDE.md에서 프로젝트명을 추출한다:
   - H1 헤더 (`# project-name`) 확인
   - 또는 파일 초반부에서 프로젝트명을 나타내는 텍스트 확인
3. CLAUDE.md가 없거나 프로젝트명을 추출할 수 없으면:
   - 사용자에게 프로젝트명을 질문:
     ```
     현재 프로젝트의 이름을 알려주세요.
     (일일업무일지에서 프로젝트를 구분하는 데 사용됩니다)
     ```
4. 프로젝트명은 **kebab-case**로 정규화 (소문자, 공백→하이픈)

---

## Phase 3 — Git 데이터 수집

현재 프로젝트 디렉토리(tasks 레포가 아닌 사용자의 실제 프로젝트)에서:

```bash
git log --oneline --since="midnight" --author="$(git config user.name)"
```

- 커밋이 없으면 커밋 없음을 사용자에게 알리고, 수동 입력으로 전환
- 커밋이 있으면 Phase 4로 진행
- 추가로 uncommitted 변경사항도 확인:
  ```bash
  git status --short
  ```

---

## Phase 4 — Completed 항목 생성 (반자동)

1. git log의 커밋 메시지를 분석하여 Completed 항목을 자동 생성:
   - 관련 커밋끼리 그룹핑 (같은 기능에 대한 여러 커밋은 하나로 합침)
   - 핵심만 간결하게 한 줄로 요약
   - 최대 7개 항목으로 제한
   - "fix typo", "lint" 등 사소한 커밋은 별도 항목으로 만들지 않되 관련 항목에 포함
2. 사용자에게 확인 요청:

```
## 오늘의 작업 로그 — {project-name}

### Completed (자동 생성)
- [x] {요약 항목 1}
- [x] {요약 항목 2}
- [x] {요약 항목 3}

수정하거나 추가할 항목이 있으면 말씀해주세요.
그대로 진행하려면 "확인"이라고 해주세요.
```

3. 사용자가 수정을 요청하면 반영 후 다시 확인
4. 커밋이 없는 경우:

```
오늘 {project-name}에 커밋이 없습니다.
기록할 작업 내용을 직접 입력해주세요.
(작업 내용이 없으면 "없음"이라고 해주세요)
```

---

## Phase 5 — Remind 항목 수집

1. **Carry-over 확인**: tasks 레포에서 이 프로젝트의 가장 최근 로그를 찾아 미완료 Remind 항목(`- [ ]`)을 확인
2. uncommitted 변경사항이 있으면 WIP 항목으로 제안
3. 사용자에게 Remind 항목 확인:

```
### Remind

{carry-over 항목이 있으면}
이전에 남긴 미완료 항목:
- [ ] {이전 항목 1}
- [ ] {이전 항목 2}

{WIP가 있으면}
현재 작업 중인 변경사항:
- [ ] {WIP 제안}

추가하거나 수정할 리마인드 항목이 있으면 말씀해주세요.
없으면 "없음"이라고 해주세요.
```

4. 사용자의 응답을 반영하여 최종 Remind 항목 확정

---

## Phase 6 — 마크다운 파일 작성

### 파일 경로 결정

```
{local_path}/{YYYY}/{MM}/{YYYY-MM-DD}.md
```

- 오늘 날짜 기준: `date +%Y-%m-%d`
- 한국어 요일 매핑: `['일', '월', '화', '수', '목', '금', '토']`

### 디렉토리 생성

```bash
mkdir -p {local_path}/{YYYY}/{MM}
```

### 파일 생성 / 업데이트

**Case A — 파일이 존재하지 않음:**

새 파일 생성:

```markdown
# {YYYY-MM-DD} ({요일})

## {project-name}

### Completed
- [x] {항목 1}
- [x] {항목 2}

### Remind
- [ ] {항목 1}
- [ ] {항목 2}
```

**Case B — 파일이 존재하고, 해당 프로젝트 섹션이 없음:**

파일 끝에 구분선과 프로젝트 섹션 추가:

```markdown
{기존 내용}

---

## {project-name}

### Completed
- [x] {항목 1}

### Remind
- [ ] {항목 1}
```

**Case C — 파일이 존재하고, 해당 프로젝트 섹션이 있음 (멱등성):**

해당 프로젝트의 `## {project-name}` 섹션을 찾아 **전체 교체**:
- 섹션 시작: `## {project-name}` 라인
- 섹션 끝: 다음 `---` 또는 다음 `## ` 또는 파일 끝
- 시작~끝 사이의 내용만 교체, 나머지는 그대로 유지

---

## Phase 7 — Commit & Push

tasks 레포에서:

```bash
cd {local_path}
git add {YYYY}/{MM}/{YYYY-MM-DD}.md
git commit -m "log: {project-name} {YYYY-MM-DD}"
git push origin {default_branch}
```

- push 실패 시 (remote에 새 커밋이 있는 경우):
  ```bash
  git pull --rebase origin {default_branch}
  git push origin {default_branch}
  ```
- 재시도도 실패하면 사용자에게 경고:
  ```
  자동 push에 실패했습니다. 로컬에는 커밋되었습니다.
  수동으로 push해주세요: cd ~/.daily-log/repo && git push
  ```

---

## Phase 8 — 완료 확인

```
## 로그 기록 완료

| 항목 | 값 |
|---|---|
| 프로젝트 | {project-name} |
| 날짜 | {YYYY-MM-DD} ({요일}) |
| Completed | {N}개 |
| Remind | {M}개 |
| 파일 | {YYYY}/{MM}/{YYYY-MM-DD}.md |
| Push | {성공/실패} |
```

---

## Constraints

- **NEVER skip user confirmation** — Completed, Remind 모두 사용자 확인 후 저장
- **NEVER write to the current project** — 오직 tasks 레포에만 기록
- **NEVER force push** — 일반 push만 사용
- **NEVER include sensitive data** — 커밋 메시지에 있더라도 비밀번호, 키, 토큰 등은 필터링
- **ALWAYS normalize project name to kebab-case**
- **ALWAYS respond in Korean**
- **ALWAYS use idempotent update** — 같은 날 같은 프로젝트 재실행 시 섹션 교체
