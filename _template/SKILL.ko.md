---
version: {{SKILL_VERSION}}
updated: {{SKILL_DATE}}
changelog:
  - "{{CHANGELOG_KO}}"
---

# {{SKILL_TITLE}}

## 설치 (Setup)

### Step 0 — 버전 확인 (필수 — 건너뛰지 않는다)
다른 작업 전에 반드시 아래 단계를 모두 수행한다:
1. 현재 프로젝트의 `.claude/skills/{{SKILL_NAME}}/SKILL.md` 파일을 읽는다
2. 파일이 존재하지 않으면 → 신규 설치. Step 1로 이동.
3. 파일이 존재하면 → YAML frontmatter에서 `version:` 값을 추출
   - `version` 필드가 없으면 → `0.0.0`으로 간주
4. 로컬 버전과 `{{SKILL_VERSION}}` (이 파일의 버전)을 비교:
   - 로컬 < `{{SKILL_VERSION}}` → **업데이트 흐름** 실행
   - 로컬 = `{{SKILL_VERSION}}` → "{{SKILL_NAME}}는 이미 최신 버전입니다 (v{{SKILL_VERSION}})" 출력 후 중단. 다른 작업 하지 않는다.
5. 비교 결과를 반드시 출력한다: "로컬: v{local} / 원격: v{{SKILL_VERSION}} → {action}"

### 업데이트 흐름
이전 버전에서 업데이트할 때:
1. `.claude/skills/{{SKILL_NAME}}/SKILL.md`를 이 파일로 덮어쓰기
2. `CLAUDE.md`의 `# {{SKILL_TITLE}}` 섹션을 아래 Step 3의 최신 내용으로 교체
3. 이전 버전과 현재 버전 사이의 changelog 항목을 사용자에게 표시
4. `.dev/{{DATA_DIR}}/` 디렉토리와 기존 데이터는 보존 — 삭제하지 않음
5. "{{SKILL_NAME}} 업데이트 완료: v{old} → v{new}" 출력

### Step 1 — 디렉토리/파일 생성
다음 경로를 프로젝트 루트에 생성:
- `.dev/{{DATA_DIR}}/`       (빈 디렉토리 — 필요에 따라 하위 디렉토리 추가)

<!-- 스킬에 필요한 초기 파일을 여기에 추가 -->

### Step 2 — 스킬 파일 설치
다음 디렉토리를 프로젝트 루트에 생성:
- `.claude/skills/{{SKILL_NAME}}/`

이 SKILL.md 파일을 fetch해서 `.claude/skills/{{SKILL_NAME}}/SKILL.md`로 저장한다.

### Step 3 — CLAUDE.md에 추가
프로젝트 루트의 `CLAUDE.md` (없으면 생성) 에 다음 지침을 추가:

```
{{CLAUDE_MD_KO}}
```

### Step 4 — 검증
- `.dev/{{DATA_DIR}}/` 존재 여부 확인
- `CLAUDE.md`에 {{SKILL_TITLE}} 섹션 존재 여부 확인
- 신규 설치 시 → "{{SKILL_TITLE}} 셋업 완료 (v{{SKILL_VERSION}})" 출력
- 업데이트 시 → "{{SKILL_TITLE}} 업데이트 완료: v{old} → v{new}" 출력

---

## 역할
<!-- 이 스킬이 하는 일을 한 문장으로 설명 -->

## 트리거 조건
<!-- 이 스킬이 언제 활성화되는지 정의 -->

<!-- 스킬 고유 Phase를 아래에 추가 -->
<!-- 예시:
## Phase 1 — 분석
## Phase 2 — 실행
## Phase 3 — 리포트
-->
