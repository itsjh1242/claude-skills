# {{SKILL_NAME}}

{{SKILL_DESC_KO}}

---

## 기능 설명

<!-- 스킬의 기능을 여기에 설명 -->

---

## 설치

다음 프롬프트를 Claude Code에 붙여넣으면 {{SKILL_NAME}}이 설치된다:

```
Install the {{SKILL_NAME}} skill by following the setup instructions here:
https://raw.githubusercontent.com/{{GITHUB_USER}}/{{GITHUB_REPO}}/main/{{SKILL_NAME}}/SKILL.md
```

Claude가 SKILL.md를 읽고 필요한 디렉토리 생성, 스킬 설치, `CLAUDE.md` 업데이트를 자동으로 수행한다.

**이미 설치했다면?** 동일한 프롬프트를 다시 붙여넣으면 된다 — 버전을 비교해서 새 버전이 있을 때만 업데이트된다.

---

## 설치 후 생성되는 파일 구조

```
{project-root}/
├── .claude/
│   └── skills/
│       └── {{SKILL_NAME}}/
│           └── SKILL.md
├── .dev/
│   └── {{DATA_DIR}}/
│       └── ...
└── CLAUDE.md          ← {{SKILL_TITLE}} 섹션 추가됨
```
