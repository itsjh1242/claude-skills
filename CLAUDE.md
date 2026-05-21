# claude-skills

Claude Code 마켓플레이스 플러그인 저장소.

## 구조

- `plugins/` — 마켓플레이스 플러그인 (각 하위 디렉토리가 하나의 플러그인)
- `docs/TEMPLATE.md` — 새 플러그인 생성 시 참고하는 템플릿

## 새 플러그인 생성

사용자가 새로운 플러그인을 만들어달라고 요청하면:
1. `docs/TEMPLATE.md`를 먼저 읽고 마켓플레이스 플러그인 구조를 확인한다
2. `plugins/{plugin-name}/` 디렉토리를 생성한다
3. `.claude-plugin/plugin.json` 매니페스트를 작성한다
4. `skills/{command-name}/SKILL.md`를 작성한다 (프론트매터: `name` + `description`)
5. `README.md`를 작성한다
6. `docs/TEMPLATE.md` 하단 Reference 테이블에 새 플러그인을 추가한다
