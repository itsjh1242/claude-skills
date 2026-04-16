# Skill Template Guide

이 디렉토리는 새로운 Claude Code 스킬을 생성할 때 참고하는 보일러플레이트입니다.

---

## 사용법

새 스킬을 만들 때, 이 디렉토리의 템플릿 파일을 읽고 `{{변수}}`를 치환하여 새 스킬 디렉토리를 생성합니다.

### 생성 절차

1. 이 파일(`TEMPLATE.md`)을 읽고 변수 목록을 확인
2. 사용자로부터 스킬 이름과 설명을 받아 변수 값을 결정
3. 아래 템플릿 파일들을 읽고 변수를 치환:
   - `_template/SKILL.md` → `{skill-name}/SKILL.md`
   - `_template/SKILL.ko.md` → `{skill-name}/docs/SKILL.ko.md`
   - `_template/README.md` → `{skill-name}/README.md`
   - `_template/README.ko.md` → `{skill-name}/docs/README.ko.md`
4. **스킬 고유 섹션** (Role, Trigger, Phase 등)은 사용자의 설명을 기반으로 작성
5. 글로벌 `~/.claude/CLAUDE.md`에 트리거 등록을 안내

### 생성 후 체크리스트

- [ ] `{skill-name}/SKILL.md` — 영문 스킬 정의
- [ ] `{skill-name}/docs/SKILL.ko.md` — 한국어 스킬 정의
- [ ] `{skill-name}/README.md` — 영문 README
- [ ] `{skill-name}/docs/README.ko.md` — 한국어 README
- [ ] Role, Trigger, Phase 등 스킬 고유 섹션 작성 완료
- [ ] `~/.claude/CLAUDE.md` 트리거 등록 안내

---

## 변수 목록

| 변수 | 설명 | 예시 |
|------|------|------|
| `{{SKILL_NAME}}` | 스킬 디렉토리명 (kebab-case) | `planner-master` |
| `{{SKILL_TITLE}}` | 스킬 표시 이름 (Title Case) | `Planner Master` |
| `{{SKILL_DESC_EN}}` | 영문 한 줄 설명 | `A skill that automatically creates development plans` |
| `{{SKILL_DESC_KO}}` | 한국어 한 줄 설명 | `개발 계획을 자동으로 생성하는 스킬` |
| `{{SKILL_VERSION}}` | 초기 버전 (항상 `1.0.0`) | `1.0.0` |
| `{{SKILL_DATE}}` | 생성 날짜 (YYYY-MM-DD) | `2025-04-16` |
| `{{GITHUB_USER}}` | GitHub 사용자명 | `itsjh1242` |
| `{{GITHUB_REPO}}` | GitHub 리포명 | `claude-skills` |
| `{{CHANGELOG_EN}}` | 영문 초기 changelog | `1.0.0: Initial release` |
| `{{CHANGELOG_KO}}` | 한국어 초기 changelog | `1.0.0: 최초 릴리스` |
| `{{DATA_DIR}}` | 스킬 데이터 디렉토리명 (`.dev/` 하위) | `planner` |
| `{{CLAUDE_MD_EN}}` | CLAUDE.md에 추가할 영문 지시문 | (스킬별로 다름) |
| `{{CLAUDE_MD_KO}}` | CLAUDE.md에 추가할 한국어 지시문 | (스킬별로 다름) |

---

## 스킬 고유 섹션 (직접 작성 필요)

템플릿은 공통 구조만 제공합니다. 아래 섹션은 스킬의 목적에 맞게 직접 설계해야 합니다:

- **Role**: 스킬이 하는 일 한 줄 정의
- **Trigger Condition**: 어떤 상황에서 자동 실행되는지
- **Phase 구조**: 스킬의 실행 단계 (Phase 1, 2, 3...)
- **Report Format**: 결과 리포트 형식 (필요 시)
- **State Management**: 상태 관리 방식 (필요 시)

---

## 글로벌 CLAUDE.md 트리거 등록

새 스킬 생성 후, `~/.claude/CLAUDE.md`에 아래 형식으로 트리거를 추가합니다:

```
# {{SKILL_TITLE}}
사용자가 "{{SKILL_NAME}} 설치", "{{SKILL_NAME}} 업데이트", "{{SKILL_NAME}} install", "{{SKILL_NAME}} update" 등
{{SKILL_NAME}} 설치/업데이트 의도가 포함된 메시지를 입력하면:
1. https://raw.githubusercontent.com/{{GITHUB_USER}}/{{GITHUB_REPO}}/main/{{SKILL_NAME}}/SKILL.md 를 fetch한다
2. 해당 파일의 Setup 섹션(Step 0~4)을 순서대로 실행한다
3. 사용자에게 추가 확인을 요청하지 않고 자동으로 진행한다
```
