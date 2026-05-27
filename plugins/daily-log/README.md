# daily-log

GitHub 레포 기반 일일업무일지 관리 플러그인. 여러 프로젝트의 작업 기록을 하나의 tasks 레포에 날짜별 마크다운으로 정리한다.

## 설치

```bash
/plugin marketplace add itsjh1242/claude-skills
/plugin install daily-log@itsjh-plugins
```

## 명령어

### `/daily-log:init`

tasks 레포를 로컬에 클론하고 초기 설정을 생성한다.

```
/daily-log:init
```

- tasks 레포 URL 입력
- `~/.daily-log/repo/`에 clone
- `~/.daily-log/config.json` 생성

### `/daily-log:log`

오늘의 작업 로그를 기록한다.

```
/daily-log:log
```

- git log 자동 분석 → Completed 항목 생성
- 사용자 확인/수정 후 저장
- Remind 항목 수집 (이전 미완료 carry-over + 신규)
- tasks 레포에 commit & push

### `/daily-log:remind`

최근 미완료 Remind 항목을 표시한다.

```
/daily-log:remind
```

- 최근 7일간 로그에서 미완료 항목 검색
- 현재 프로젝트 기준으로 필터링

## 일일업무일지 형식

파일 경로: `{YYYY}/{MM}/{YYYY-MM-DD}.md`

```markdown
# 2026-05-27 (화)

## project-alpha

### Completed
- [x] API 엔드포인트 구현
- [x] 페이지네이션 버그 수정

### Remind
- [ ] auth 모듈 유닛 테스트 작성

---

## project-beta

### Completed
- [x] CI/CD 파이프라인 구성

### Remind
- [ ] 스테이징 환경 설정 추가
```

## 설정

`~/.daily-log/config.json`:

```json
{
  "repo_url": "https://github.com/username/my-tasks.git",
  "local_path": "~/.daily-log/repo",
  "default_branch": "main"
}
```

## 플러그인 구조

```
daily-log/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── init/
│   │   └── SKILL.md
│   ├── log/
│   │   └── SKILL.md
│   └── remind/
│       └── SKILL.md
└── README.md
```
