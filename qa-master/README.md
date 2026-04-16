# qa-master

A Claude Code skill that automatically evaluates feature quality when implementation is complete.

Rather than being triggered by a slash command, qa-master reads the conversation context and code changes to judge on its own when a feature is ready — then naturally proposes a QA session.

---

## What it does

### Phase 2A — Functional Performance Evaluation
Checks whether the code operates as intended without errors:
- Bug and runtime error likelihood
- Edge case handling
- Behavior under exceptional conditions

Each item is rated **PASS / FAIL / WARN** with a rationale.

### Phase 2B — Quality Performance Evaluation
Checks how accurately the feature produces its intended output.

The CLI analyzes the project type and feature purpose, then **designs its own evaluation criteria** using measurable, numerical thresholds (A–E grades). It generates test scenarios, runs them, and produces a final grade.

Supports **dev / prod mode** via `QA_MODE` environment variable:

| Mode | Standard Items | Core Items |
|------|---------------|------------|
| `dev` (default) | All ≥ C | All Core ≥ B |
| `prod` | All ≥ B | All Core ≥ A |

### Reports
QA results are saved as Markdown files under `.dev/qa/reports/`. Reports are kept in sync with the codebase — updated when features change, deleted when features are removed.

---

## Installation

Paste the following prompt into Claude Code to install qa-master:

```
Install the qa-master skill by following the setup instructions here:
https://raw.githubusercontent.com/itsjh1242/claude-skills/main/qa-master/SKILL.md
```

Claude will read the SKILL.md file and set up the required directories, install the skill, and update your `CLAUDE.md` automatically.

**Already installed?** Paste the same prompt again — the skill will compare versions and update only if a newer version is available.

---

## File structure created after setup

```
{project-root}/
├── .claude/
│   └── skills/
│       └── qa-master/
│           └── SKILL.md
├── .dev/
│   └── qa/
│       ├── pending.md
│       └── reports/
│           └── {feature-name}.md
└── CLAUDE.md          ← QA Master section added
```
