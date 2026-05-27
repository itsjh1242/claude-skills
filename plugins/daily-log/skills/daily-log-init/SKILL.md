---
name: daily-log-init
description: "초기 설정. tasks 레포 URL을 입력받아 로컬에 클론하고 설정 파일을 생성한다. Use when: user says 'daily-log 설정', 'daily-log init', '일일 로그 초기화', '업무일지 셋업'. Do NOT use when: already initialized and just wanting to log or check reminders."
---

# daily-log — init

tasks 레포를 로컬에 클론하고 `~/.daily-log/config.json` 설정 파일을 생성한다.

> **Language**: Always respond in **Korean (한국어)**.

---

## Phase 1 — 기존 설정 확인

1. `~/.daily-log/config.json` 파일이 존재하는지 확인한다
2. 존재하면 현재 설정을 표시하고 재설정할지 사용자에게 확인:

```
## 기존 daily-log 설정 발견

| 항목 | 값 |
|---|---|
| Tasks 레포 | {repo_url} |
| 로컬 경로 | {local_path} |
| 브랜치 | {default_branch} |

재설정하시겠습니까?
```

3. 사용자가 재설정을 원하지 않으면 종료

---

## Phase 2 — 레포 URL 수집

사용자에게 tasks 레포 URL을 질문:

```
## daily-log 초기 설정

일일업무일지를 저장할 GitHub 레포 URL을 알려주세요.
(예: https://github.com/username/my-tasks.git)
```

- HTTPS 또는 SSH URL 모두 허용
- URL 형식이 올바른지 기본 검증 (`.git`으로 끝나거나 `github.com/` 포함)

---

## Phase 3 — Clone / Pull

1. `~/.daily-log/` 디렉토리 생성 (없으면)
2. `~/.daily-log/repo/` 디렉토리 상태 확인:
   - **디렉토리 없음**: `git clone {repo_url} ~/.daily-log/repo/`
   - **디렉토리 있고 git repo**: remote URL 확인
     - 같은 URL → `git pull`
     - 다른 URL → 사용자에게 경고, 덮어쓸지 확인
   - **디렉토리 있지만 git repo 아님**: 사용자에게 경고, 삭제 후 재클론할지 확인
3. clone/pull 성공 확인

---

## Phase 4 — 설정 파일 저장

`~/.daily-log/config.json` 작성:

```json
{
  "repo_url": "{user_provided_url}",
  "local_path": "~/.daily-log/repo",
  "default_branch": "main"
}
```

- `default_branch`는 clone된 레포의 기본 브랜치를 자동 감지하여 설정:
  ```bash
  git -C ~/.daily-log/repo rev-parse --abbrev-ref HEAD
  ```

---

## Phase 5 — 완료 안내

```
## daily-log 초기 설정 완료

| 항목 | 값 |
|---|---|
| Tasks 레포 | {repo_url} |
| 로컬 경로 | ~/.daily-log/repo |
| 브랜치 | {default_branch} |

### 사용 방법
- `/daily-log:daily-log-log` — 오늘의 작업 로그 기록
- `/daily-log:daily-log-remind` — 미완료 리마인드 항목 확인
```

---

## Constraints

- **NEVER store project name in config** — 프로젝트명은 log/remind 실행 시 CLAUDE.md에서 매번 감지
- **NEVER modify the tasks repo content** — init은 clone/pull만 수행
- **NEVER proceed without user confirmation** — URL 입력, 덮어쓰기 등 모든 중요 단계에서 확인
- **ALWAYS respond in Korean**
