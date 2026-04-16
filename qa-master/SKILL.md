---
version: 1.2.0
updated: 2025-04-15
changelog:
  - "1.2.0: Force QA block output — CLI must append a fixed QA prompt block, not just internally check"
  - "1.1.0: Strengthen CLAUDE.md trigger — use mandatory check instead of passive suggestion"
  - "1.0.0: Initial release"
---

# QA Master

## Setup

### Step 0 — Version check (MANDATORY — do NOT skip)
You MUST perform all of the following steps before doing anything else:
1. Read the file `.claude/skills/qa-master/SKILL.md` in the current project
2. If the file does NOT exist → this is a fresh install. Go to Step 1.
3. If the file exists → extract the `version:` value from its YAML frontmatter
   - If the file has no `version` field → treat as `0.0.0`
4. Compare the local version with `1.2.0` (this file's version):
   - Local < `1.2.0` → go to **Update Flow**
   - Local = `1.2.0` → output "qa-master is already up to date (v1.2.0)" and STOP. Do nothing else.
5. You MUST output the comparison result: "Local: v{local} / Remote: v1.2.0 → {action}"

### Update Flow
When updating from an older version:
1. Overwrite `.claude/skills/qa-master/SKILL.md` with this file
2. Replace the `# QA Master` section in `CLAUDE.md` with the latest version from Step 3 below
3. Show the user the changelog entries between their old version and the current version
4. Preserve `.dev/qa/` directory and all existing reports — do NOT delete them
5. Output "qa-master updated: v{old} → v{new}"

### Step 1 — Create directories and files
Create the following paths at the project root:
- `.dev/qa/reports/`       (empty directory)
- `.dev/qa/pending.md`     (create with the initial template below)

Initial template for `pending.md`:
```
---
last_updated: (date)
status: idle
features: []
---
```

### Step 2 — Install skill file
Create the following directory at the project root:
- `.claude/skills/qa-master/`

Fetch this SKILL.md file and save it as `.claude/skills/qa-master/SKILL.md`.

> The CLI infers the SKILL.md URL from the GitHub URL it used to read this file,
> then fetches the raw content and installs it into the project.

### Step 3 — Add to CLAUDE.md
Add the following instructions to `CLAUDE.md` at the project root (create it if it doesn't exist):

```
# QA Master
독립적으로 동작 가능한 기능 단위의 코드 변경을 완료했을 때,
"다음 작업" 또는 "계속할까요" 를 묻기 전에 반드시 아래 블록을 응답 끝에 추가한다:

---
**QA 평가 가능**: 이 기능이 완성된 것으로 보입니다. QA 평가를 수행할까요?
→ Yes: `.claude/skills/qa-master/SKILL.md`를 읽고 Phase 2A + 2B를 수행합니다.
→ No: 건너뛰고 계속합니다.
---

이 블록을 생략하지 않는다. 자체 요약으로 대체하지 않는다.
완성된 기능인지 확실하지 않으면, 블록을 포함한다.
- 스킬 위치: .claude/skills/qa-master/SKILL.md
- 리포트 저장: .dev/qa/reports/
- 상태 파일: .dev/qa/pending.md
```

### Step 4 — Verify
- Confirm `.dev/qa/pending.md` exists
- Confirm the QA Master section exists in `CLAUDE.md`
- If fresh install → output "QA Master setup complete (v{version})"
- If update → output "QA Master updated: v{old} → v{new}"

---

## Role
A skill that proposes and performs QA when it determines that a feature implementation is complete.

## Trigger Condition
The CLI judges on its own by combining the following context — not by keyword matching:
- Was code for a specific feature unit written or modified in the recent conversation?
- Is that code in a state where it can operate independently?
- Is the user showing intent to move on to the next task?
When all three are judged to be true, the CLI proposes QA at the end of its response.

## Phase 1 — Project Analysis (Automatic)
- Detect project type (RAG, CLI, API server, web app, etc.)
- Extract the list of core features (by function / endpoint / module)
- Identify the scope of the completed feature

## Phase 2A — Functional Performance Evaluation
Check whether the code operates as intended without errors:
- Likelihood of bugs and runtime errors
- Whether edge cases are handled
- Behavior under exceptional conditions
- Result: PASS / FAIL / WARN + rationale

## Phase 2B — Quality Performance Evaluation
Check how accurately the feature produces its intended output.

### Evaluation Criteria Design Principles
The CLI analyzes the project type and feature purpose, then designs the evaluation criteria itself.
Criteria must always be numerical and measurable:
- Subjective expressions like "good / bad" are prohibited
- Define A–E grade thresholds numerically for each item
  e.g. Accuracy: A=90%↑ / B=75%↑ / C=60%↑ / D=45%↑ / E=44%↓
  e.g. Response latency: A=1s↓ / B=2s↓ / C=3s↓ / D=5s↓ / E=5s↑
- Distinguish between Core items and Standard items

Example criteria by project type:
- RAG chatbot → Accuracy (Core), Hallucination rate (Core), Response latency, Citation accuracy
- CLI parser → Parse accuracy (Core), Error rejection rate (Core), Throughput, Edge case coverage
- API server → Response accuracy (Core), Status code match rate (Core), Response latency, Payload schema match rate

### Test Scenario Generation and Execution
1. Analyze the code and feature spec, then generate 3–5 representative scenarios
   - Happy path
   - Boundary value cases
   - Intentional invalid input cases
2. Execute each scenario and collect results
3. Map collected results to the designed grade thresholds to determine per-item grades
4. Combine Core and Standard item grades to determine the final grade (A–E)

### dev/prod PASS Criteria
Determined by the environment variable `QA_MODE=dev|prod`. Default is `dev`.

| Mode | Standard Items | Core Items |
|------|---------------|------------|
| dev  | All items grade C or above | All Core items grade B or above |
| prod | All items grade B or above | All Core items grade A or above |

If any single item falls below the threshold, Phase 2B is marked FAIL.

## Phase 3 — Report Storage
- Save to `.dev/qa/reports/{feature-name}.md`
- Record 2A and 2B results in separate sections
- Update `.dev/qa/pending.md` state
- If a feature is modified or deleted, update or delete the corresponding report accordingly

### Report Lifecycle Management
At the start of each QA session, the CLI checks existing reports against the current codebase:
- If a reported feature no longer exists in the codebase → delete the report
- If a reported feature has been significantly modified → re-run QA and overwrite the report
- If a new feature is detected that has no report → add it to `pending.md` as a QA candidate

"Significantly modified" means: core logic, input/output interface, or behavior has changed — not minor refactoring or formatting.

## Report Format Template (`.dev/qa/reports/{feature}.md`)

```
# {Feature Name} QA Report

**Evaluated at**: {datetime}
**Feature description**: {one-line description of the feature}
**QA_MODE**: dev | prod

---

## Phase 2A — Functional Performance Evaluation

| Item | Result | Rationale |
|------|--------|-----------|
| Bug / Runtime errors | PASS/FAIL/WARN | ... |
| Edge case handling | PASS/FAIL/WARN | ... |
| Exception handling | PASS/FAIL/WARN | ... |

**2A Verdict**: PASS / FAIL

---

## Phase 2B — Quality Performance Evaluation

**Criteria Design**:
> {Description of per-item grade thresholds designed by the CLI}
> e.g. Accuracy — A=90%↑ / B=75%↑ / C=60%↑ / D=45%↑ / E=44%↓

**Test Scenarios**:
| # | Scenario | Type | Input | Expected | Actual |
|---|----------|------|-------|----------|--------|
| 1 | ... | happy/boundary/invalid | ... | ... | ... |
| 2 | ... | ... | ... | ... | ... |

**Per-item Evaluation**:
| Item | Category | Grade | Measured | Threshold |
|------|----------|-------|----------|-----------|
| ...  | Core     | A–E   | ...      | ...       |
| ...  | Standard | A–E   | ...      | ...       |

**Final Grade**: A / B / C / D / E
**2B Verdict**: PASS / FAIL  _(Applied criteria: dev — Standard C↑, Core B↑ / prod — Standard B↑, Core A↑)_

---

## Summary
{Improvement suggestions and next actions}
```

## State Management (`.dev/qa/pending.md`)
- Track QA progress state across sessions
- Manage incomplete QA items, last evaluation timestamp, and feature-to-report mapping table
