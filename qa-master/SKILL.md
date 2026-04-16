---
version: 2.0.0
updated: 2025-04-16
changelog:
  - "2.0.0: Auto-execute QA without user prompt — auto-improve loop until all items reach grade A (max 3 rounds)"
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
4. Compare the local version with `2.0.0` (this file's version):
   - Local < `2.0.0` → go to **Update Flow**
   - Local = `2.0.0` → output "qa-master is already up to date (v2.0.0)" and STOP. Do nothing else.
5. You MUST output the comparison result: "Local: v{local} / Remote: v2.0.0 → {action}"

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

### Step 3 — Add to CLAUDE.md
Add the following instructions to `CLAUDE.md` at the project root (create it if it doesn't exist):

```
# QA Master
When a unit of independently runnable feature code is completed,
you MUST read `.claude/skills/qa-master/SKILL.md` and execute QA automatically
BEFORE asking "what's next" or "shall I continue".
Do NOT ask the user whether to run QA. Just run it and output the results.
- Skill location: .claude/skills/qa-master/SKILL.md
- Report storage: .dev/qa/reports/
- State file: .dev/qa/pending.md
```

### Step 4 — Verify
- Confirm `.dev/qa/pending.md` exists
- Confirm the QA Master section exists in `CLAUDE.md`
- If fresh install → output "QA Master setup complete (v{version})"
- If update → output "QA Master updated: v{old} → v{new}"

---

## Role
A skill that automatically executes QA when it determines that a feature implementation is complete, and auto-improves the code until all evaluation items reach grade A.

## Trigger Condition
The CLI judges on its own by combining the following context — not by keyword matching:
- Was code for a specific feature unit written or modified in the recent conversation?
- Is that code in a state where it can operate independently?
- Is the user showing intent to move on to the next task?

When all three are judged to be true, the CLI **immediately executes QA** without asking the user.

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

## Phase 2C — Auto-Improve Loop (NEW)
After Phase 2A + 2B, if any evaluation item is below grade A, the CLI automatically enters the improvement loop.

### Loop Flow
```
Round 1: Phase 2A + 2B → check all items
  → All items grade A? → YES → PASS → go to Phase 3
  → NO → identify items below A → analyze root cause → auto-fix code → re-run Phase 2A + 2B

Round 2: re-evaluate
  → All items grade A? → YES → PASS → go to Phase 3
  → NO → auto-fix again → re-run

Round 3: re-evaluate
  → All items grade A? → YES → PASS → go to Phase 3
  → NO → STOP loop → go to Phase 3 with current state
```

### Loop Rules
1. **Goal**: All evaluation items (Core + Standard) must reach grade A
2. **Max rounds**: 3 (initial evaluation counts as round 1)
3. **Per-round actions**:
   - Identify all items below grade A
   - Analyze the root cause for each item
   - Apply targeted code fixes
   - Re-run Phase 2A + 2B to measure improvement
4. **Loop exit conditions**:
   - All items reach grade A → exit with PASS
   - Round 3 completed without reaching all-A → exit, save current state
5. **After max rounds without all-A**:
   - Save the report with the best result achieved
   - Output a summary of remaining issues to the user
   - The user decides next steps manually

### Improvement Strategy
When fixing code to improve grades:
- Fix one root cause at a time — avoid shotgun changes
- Prioritize Core items over Standard items
- Preserve existing passing behavior — do not regress
- Each fix should be minimal and targeted

## Phase 3 — Report Storage
- Save to `.dev/qa/reports/{feature-name}.md`
- Record 2A, 2B, and 2C (loop history) results in separate sections
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
**Rounds**: {number of rounds executed} / 3
**Final result**: ALL-A / PARTIAL (details below)

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

## Phase 2C — Auto-Improve Loop History

| Round | Items below A | Fix applied | Result after fix |
|-------|--------------|-------------|-----------------|
| 1     | {list}       | (initial)   | {grades}        |
| 2     | {list}       | {description}| {grades}       |
| 3     | {list}       | {description}| {grades}       |

**Loop outcome**: ALL-A reached at round {n} / Stopped after 3 rounds — {remaining issues}

---

## Summary
{Improvement suggestions and next actions if ALL-A was not reached}
```

## State Management (`.dev/qa/pending.md`)
- Track QA progress state across sessions
- Manage incomplete QA items, last evaluation timestamp, and feature-to-report mapping table
