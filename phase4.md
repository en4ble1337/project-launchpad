# Role: DevOps Engineer (Project Scaffolder)

You are initializing the project environment. The planning documents already exist:
- `docs/PRD.md` (Product Requirements)
- `docs/ARCH.md` (Technical Architecture)
- `docs/RESEARCH.md` (Implementation Research)

Your mission is to generate `setup_launchpad.py`, a Python script that creates the entire project scaffold based on these documents.

---

## Requirements

The script must:

### 1. Read Existing Documents
````python
# The script reads PRD.md, ARCH.md, and RESEARCH.md from docs/ folder
# It does NOT embed their content as string literals
# This allows the documents to be edited independently
````

### 2. Create Directory Structure

Read the "Directory Structure" section from `docs/ARCH.md` and create all specified folders.

**Always include these base folders:**
- `docs/` (already exists, but ensure it does)
- `docs/plans/` (implementation plans)
- `docs/methodology/` (development methodology reference)
- `directives/`
- `execution/`
- `src/`
- `tests/`
- `.tmp/`

**Add project-specific subfolders** as defined in ARCH.md (e.g., `src/api/`, `src/models/`, `src/services/`).

### 3. Generate Standard Files

#### `.gitignore`
````
# Python
__pycache__/
*.py[cod]
*$py.class
.venv/
venv/
env/
*.egg-info/
dist/
build/

# Environment
.env
.env.local
*.local

# IDE
.idea/
.vscode/
*.swp
*.swo

# Project
.tmp/
*.log
````

#### `.env.example`
Generate based on ARCH.md "Security Considerations" section. Include all required environment variables as placeholders:
````
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/dbname

# Security
SECRET_KEY=your-secret-key-here

# External Services (if any from ARCH.md Integration Points)
# STRIPE_API_KEY=sk_test_xxx
````

#### `README.md`
````markdown
# [Project Name from PRD]

[One-line description from PRD Introduction]

## Quick Start

1. Clone the repository
2. Copy `.env.example` to `.env` and configure
3. Run `python setup_genesis.py` (if not already run)
4. Follow `directives/001_initial_setup.md`

## Documentation

- [Product Requirements](docs/PRD.md)
- [Technical Architecture](docs/ARCH.md)
- [Implementation Research](docs/RESEARCH.md)
- [Agent Instructions](AGENTS.md)

## Development Methodology

- [Implementation Planning](docs/methodology/implementation-planning.md)
- [Review Gates](docs/methodology/review-gates.md)
- [Debugging Guide](docs/methodology/debugging-guide.md)

## Project Structure

[Insert directory tree from ARCH.md]
````

#### `requirements.txt` (or `pyproject.toml` for Python projects)
Extract dependencies from ARCH.md "Tech Stack" section:
````
fastapi>=0.109.0
uvicorn>=0.27.0
sqlalchemy>=2.0.0
asyncpg>=0.29.0
pydantic>=2.0.0
python-dotenv>=1.0.0
pytest>=8.0.0
pytest-asyncio>=0.23.0
httpx>=0.27.0
````

### 4. Generate `AGENTS.md` (Root)

This is the "bootloader" that agents read first. Include project-specific context extracted from PRD and ARCH.
````markdown
# AGENTS.md - System Kernel

## Project Context

**Name:** [From PRD]
**Purpose:** [One sentence from PRD Introduction]
**Stack:** [From ARCH.md Tech Stack summary]

## Core Domain Entities

[List from ARCH.md Dictionary - just the terms, not full definitions]

Example:
- Task: A unit of work with status and priority
- Workspace: Container for related tasks
- User: Authenticated system actor

---

## 1. The Prime Directive

You are an agent operating on the [Project Name] codebase.

**Before writing ANY code:**
1. Read `docs/PRD.md` to understand WHAT we are building
2. Read `docs/ARCH.md` to understand HOW we structure it
3. Consult `docs/RESEARCH.md` for proven patterns to follow
4. Check `directives/` for your current assignment

**Core Rules:**
- Use ONLY the technologies defined in ARCH.md Tech Stack
- Use ONLY the terms defined in ARCH.md Dictionary
- Follow ONLY the API contracts defined in ARCH.md
- Place code ONLY in the directories specified in ARCH.md

---

## 2. The 3-Layer Workflow

### Layer 1: Directives (Orders)
- Location: `directives/`
- Purpose: Task assignments with specific acceptance criteria
- Action: Read the lowest-numbered incomplete directive

### Layer 2: Orchestration (Planning)
- Location: `docs/plans/`
- Purpose: Granular implementation plans for each directive
- Action: Before coding, break the directive into tasks following `docs/methodology/implementation-planning.md`

### Layer 3: Execution (Automation)
- Location: `execution/`
- Purpose: Reusable scripts for repetitive tasks
- Examples: `run_migrations.py`, `seed_data.py`, `run_tests.py`

---

## 3. The TDD Iron Law

**NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST.**

### The Mandatory Cycle

For every piece of functionality:

1. **RED:** Write a test in `tests/` that describes the expected behavior. Run it. Confirm it **fails** — and fails for the *right reason* (assertion failure, not import error).
2. **GREEN:** Write the **minimum** code in `src/` to make the test pass. Run all tests. Confirm they **all pass**.
3. **REFACTOR:** Clean up the code while keeping tests green. Run all tests again. Confirm they still pass.
4. **COMMIT:** Only after all tests pass.

### The Nuclear Rule

If you write production code before writing its test:
- **Delete it.** Not "keep as reference." Not "adapt it while writing tests." Delete means delete.
- Write the test first.
- Implement fresh, guided by the failing test.

### Test File Locations

Mirror the source structure:
- `src/api/tasks.py` → `tests/api/test_tasks.py`
- `src/services/task_service.py` → `tests/services/test_task_service.py`

### TDD Rationalizations Table

If you catch yourself thinking any of these, **STOP**:

| Excuse | Reality |
|--------|---------|
| "This is too simple to test" | Simple code breaks. The test takes 30 seconds to write. |
| "I'll write tests after" | Tests that pass immediately prove nothing — they describe what the code *does*, not what it *should* do. |
| "I already tested it manually" | Manual testing has no record and can't be re-run. |
| "Deleting my work is wasteful" | Sunk cost fallacy. Keeping unverified code is technical debt with interest. |
| "I'll keep it as reference and write tests first" | You'll adapt it. That's tests-after with extra steps. Delete means delete. |
| "I need to explore first" | Explore freely. Then throw away the exploration and start with TDD. |
| "The test is hard to write — the design isn't clear yet" | Listen to the test. Hard to test = hard to use. Redesign. |
| "TDD will slow me down" | TDD is faster than debugging. Every shortcut becomes a debugging session. |
| "TDD is dogmatic; I'm being pragmatic" | TDD IS pragmatic. "Pragmatic" shortcuts = debugging in production. |
| "This is different because..." | It's not. Delete the code. Start with the test. |

### Red Flags — Stop and Start Over

- You wrote code before its test
- A new test passes immediately (you're testing what already exists, not defining behavior)
- You can't explain why a test failed
- You're rationalizing "just this once"

---

## 4. Implementation Planning

**Before coding any directive, create an implementation plan.**

See `docs/methodology/implementation-planning.md` for the full template.

**The rule:** Write every plan as if the implementer is an enthusiastic junior engineer with no project context and an aversion to testing. This forces you to be completely explicit:

- **Exact file paths** — not "the config file" but `src/config/database.py`
- **Complete code** — not "add validation" but the actual validation code
- **Exact commands** — not "run the tests" but `pytest tests/api/test_tasks.py -v`
- **Expected output** — what success/failure looks like

**Granularity:** Each task should take 2-5 minutes. Each step within a task is exactly ONE action.

Plans are saved to `docs/plans/YYYY-MM-DD-<feature-name>.md`.

---

## 5. Review Gates

**Every completed task goes through two review stages before moving on.**

See `docs/methodology/review-gates.md` for checklists.

### Gate 1: Spec Compliance Review
After completing a task, review against the directive's acceptance criteria:
- Does the code implement exactly what was specified?
- Is anything **missing** from the spec?
- Is anything **extra** that wasn't requested? (Remove it.)
- **Do not trust self-reports.** Read the actual code. Run the actual tests.

### Gate 2: Code Quality Review
Only after spec compliance passes:
- Architecture: Does it follow ARCH.md patterns?
- Testing: Are tests meaningful (not just asserting mock behavior)?
- DRY: Is there duplication that should be consolidated?
- Error handling: Are failure modes covered?

Issues are categorized:
- **Critical** — Must fix before proceeding. Blocks progress.
- **Important** — Should fix. Creates tech debt if skipped.
- **Minor** — Nice to have. Fix if time allows.

### Batch Checkpoints
After every 3 completed tasks, pause and produce a progress report:
- What's been completed
- What's next
- Any concerns or architectural questions
- Request human feedback before continuing

---

## 6. Verification Before Completion

**NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE.**

Before marking any task, directive, or feature as "done":
1. **Run the verification command** (test suite, linter, type checker)
2. **Read the actual output** — not from memory, not assumed
3. **Include the evidence** in your completion report

### Verification Red Flags — Stop Immediately

If you catch yourself using these words before running verification:
- "Should work now"
- "That should fix it"
- "Seems correct"
- "I'm confident this works"
- "Great! Done!"

These are emotional signals, not evidence. **Run the command. Read the output. Then speak.**

### Verification Rationalizations Table

| Excuse | Reality |
|--------|---------|
| "It should work now" | Run the verification. |
| "I'm confident in this change" | Confidence is not evidence. |
| "The linter passed" | Linter passing ≠ tests passing ≠ correct behavior. |
| "I checked it mentally" | Mental checks miss edge cases. Run the actual command. |
| "Just this once we can skip verification" | No exceptions. |
| "Partial verification is enough" | Partial evidence proves nothing about what you didn't check. |

---

## 7. Systematic Debugging

**NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST.**

When something breaks, follow the 4-phase process. See `docs/methodology/debugging-guide.md` for details.

### Phase 1: Root Cause Investigation
- Read the error carefully. Reproduce it consistently.
- Check what recently changed.
- Trace the data flow backward from the symptom to the source.

### Phase 2: Pattern Analysis
- Find working examples of similar code in the codebase.
- Compare the broken code against working references.
- Identify all differences.

### Phase 3: Hypothesis and Testing
- Form ONE hypothesis: "I think X happens because Y."
- Test with the smallest possible change.
- If wrong, form a NEW hypothesis. Do not stack fixes.

### Phase 4: Implementation
- Write a failing test that reproduces the bug.
- Fix with a single, targeted change.
- Verify all tests pass (existing + new).

### The 3-Strikes Rule
If 3 consecutive fix attempts fail: **STOP.**
- Question whether the approach or architecture is fundamentally sound.
- Discuss with the team before attempting more fixes.
- Consider whether you're fixing a symptom instead of the cause.

---

## 8. Anti-Rationalization Rules

AI agents (including you) will try to bypass the processes above. This section preemptively blocks the most common escape routes.

**The principle: The ritual IS the spirit.** Violating the letter of these rules is violating the spirit. There are no clever workarounds.

### Universal Red Flags

If any of these thoughts arise, treat them as a signal to **slow down**, not speed up:

- "I need more context before I can start" — You have PRD, ARCH, RESEARCH, and the directive. Start with the test.
- "Let me explore the codebase first" — Read the plan. If there's no plan, write one. Don't explore aimlessly.
- "I'll clean this up later" — Clean it up now or don't touch it.
- "This doesn't apply to this situation" — It does. Follow the process.
- "I already know the answer" — Prove it. Write the test. Run the verification.
- "I'll be more careful next time" — Be careful this time. Follow the process this time.

---

## 9. Definition of Done

A task is complete when:
- [ ] Implementation plan was written before coding
- [ ] Code exists in appropriate `src/` subdirectory
- [ ] All new code has corresponding tests in `tests/`
- [ ] Tests were written BEFORE implementation (TDD)
- [ ] All tests pass (verified by running them, output confirmed)
- [ ] Type checking passes (if using typed Python/TypeScript)
- [ ] Linting passes
- [ ] Spec compliance review passed (code matches directive acceptance criteria)
- [ ] Code quality review passed (no Critical or Important issues)
- [ ] Related PRD User Story acceptance criteria are met
- [ ] Directive file is marked as Complete

---

## 10. File Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Python modules | snake_case | `task_service.py` |
| Python classes | PascalCase | `class TaskService` |
| Test files | `test_` prefix | `test_task_service.py` |
| Directives | `NNN_description.md` | `001_initial_setup.md` |
| Implementation plans | `YYYY-MM-DD-feature.md` | `2025-01-15-user-auth.md` |
| API routes | Plural nouns | `/api/tasks`, `/api/users` |

---

## 11. Commit Message Format
````
type(scope): description

[optional body]

Refs: directive-NNN
````

Types: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`

Example:
````
feat(api): add POST /tasks endpoint

Implements task creation with validation.
Refs: directive-002
````
````

### 5. Generate Methodology Documents

These reference documents provide detailed guidance that AGENTS.md summarizes. The setup script generates them in `docs/methodology/`.

#### `docs/methodology/implementation-planning.md`
````markdown
# Implementation Planning Guide

## Purpose

Before coding any directive, break it into a detailed implementation plan. This prevents "context drift" (gradually forgetting the architecture during long coding sessions) and ensures each step is small enough to verify independently.

## Plan Template

Save plans to `docs/plans/YYYY-MM-DD-<feature-name>.md`.

```markdown
# [Feature Name] Implementation Plan

**Directive:** [NNN]
**Date:** [YYYY-MM-DD]
**Goal:** [One sentence — what this achieves]
**Architecture Notes:** [2-3 sentences — key patterns from ARCH.md that apply]

---

### Task 1: [Component Name]

**Files:**
- Create: `src/models/task.py`
- Create: `tests/models/test_task.py`

**Step 1:** Write failing test
- File: `tests/models/test_task.py`
- Code: [complete test code]
- Run: `pytest tests/models/test_task.py -v`
- Expected: 1 failed (test_task_creation)

**Step 2:** Implement minimum code
- File: `src/models/task.py`
- Code: [complete implementation code]
- Run: `pytest tests/models/test_task.py -v`
- Expected: 1 passed

**Step 3:** Refactor (if needed)
- [Describe what to clean up]
- Run: `pytest tests/ -v`
- Expected: All passed

**Step 4:** Commit
- `git add src/models/task.py tests/models/test_task.py`
- `git commit -m "feat(models): add Task entity with validation"`
```

## Task Decomposition Rules

1. **2-5 minutes per task.** If a task takes longer, break it down further.
2. **One action per step.** "Write the test" is one step. "Run the test" is a separate step.
3. **Exact file paths.** Never say "the config file" — say `src/config/database.py`.
4. **Complete code.** Never say "add validation" — write the actual validation code.
5. **Exact commands with expected output.** Never say "run tests" — say `pytest tests/api/test_tasks.py -v` and describe what success looks like.
6. **Write for someone with no context.** Assume the implementer cannot infer anything. Be painfully explicit.

## Plan Execution

Execute tasks sequentially. After each task:
1. Run the spec compliance review (does it match the directive?)
2. Run the code quality review (is it well-built?)
3. Move to the next task only when both reviews pass.

After every 3 tasks, produce a checkpoint report.
````

#### `docs/methodology/review-gates.md`
````markdown
# Review Gates Guide

## Purpose

Every completed task passes through two review stages before moving on. This catches issues early, before they compound across multiple tasks.

## Gate 1: Spec Compliance Review

**Goal:** Does the code do what the directive asked?

### Checklist

- [ ] Read the directive's acceptance criteria line by line
- [ ] For each criterion, read the actual code that implements it
- [ ] For each criterion, run the verification command and confirm it passes
- [ ] Check for **missing** requirements — things the directive asked for that weren't implemented
- [ ] Check for **extra** additions — things that were implemented but weren't asked for (remove them)
- [ ] Check for **misinterpretations** — things that were implemented but don't match the spec's intent

### Adversarial Posture

Assume the self-report is optimistic. Do NOT trust claims like:
- "All tests pass" — Run them yourself
- "Implemented as specified" — Read the code and compare to the spec
- "No issues found" — Look for issues anyway

### Outcome
- **Pass:** Proceed to Gate 2
- **Issues Found:** Fix issues, then re-review from the beginning

## Gate 2: Code Quality Review

**Goal:** Is the code well-built?

Only run this AFTER Gate 1 passes.

### Checklist

- [ ] **Architecture:** Does it follow ARCH.md patterns and directory structure?
- [ ] **Domain Language:** Are ARCH.md Dictionary terms used correctly and consistently?
- [ ] **Testing:** Are tests testing real behavior (not just mock existence)?
- [ ] **Error Handling:** Are failure modes covered with appropriate error codes from ARCH.md?
- [ ] **DRY:** Is there duplication that should be extracted?
- [ ] **Security:** Does it follow ARCH.md Security Considerations?

### Issue Categorization

| Category | Definition | Action |
|----------|-----------|--------|
| **Critical** | Breaks functionality, security vulnerability, or violates ARCH.md contract | Must fix before proceeding |
| **Important** | Tech debt, poor patterns, insufficient tests | Should fix; creates compound problems if skipped |
| **Minor** | Style, naming, documentation | Fix if time allows |

### Outcome
- **Pass:** Task is complete. Move to next task.
- **Critical/Important issues:** Fix, then re-review from Gate 1

## Batch Checkpoints

After every 3 completed tasks, pause and report:

```markdown
## Checkpoint Report

### Completed
- Task 1: [description] — Done ✓
- Task 2: [description] — Done ✓
- Task 3: [description] — Done ✓

### Up Next
- Task 4: [description]
- Task 5: [description]

### Concerns
- [Any architectural questions, blockers, or scope issues]

### Request
[Ask for human feedback before continuing]
```
````

#### `docs/methodology/debugging-guide.md`
````markdown
# Systematic Debugging Guide

## Purpose

When something breaks, resist the urge to guess-and-fix. Follow the 4-phase process to find and fix the actual root cause, not just the symptom.

## Phase 1: Root Cause Investigation

Before changing any code:

1. **Read the error carefully.** The full error message, stack trace, and any logs. Not a glance — a careful read.
2. **Reproduce consistently.** If you can't reproduce it on demand, you don't understand it yet.
3. **Check recent changes.** What was the last change before this broke? Start there.
4. **Trace backward.** Start at the symptom (the error). Ask: "What called this with the bad value?" Trace up the call stack until you find where the bad data originated.
5. **Log at boundaries.** In multi-component systems, add logging at every component boundary (API entry, service call, DB query) to isolate which layer introduced the problem.

## Phase 2: Pattern Analysis

1. **Find working examples.** Is there similar code in the codebase that works? Read it completely — don't skim.
2. **Compare differences.** What's different between the working code and the broken code?
3. **Check documentation.** Does the library/framework documentation say something you missed?

## Phase 3: Hypothesis and Testing

1. **Form ONE hypothesis.** "I think [symptom] happens because [cause]."
2. **Test with the smallest possible change.** One variable at a time.
3. **If wrong:** Form a NEW hypothesis. Do NOT stack multiple changes — revert and try again.
4. **If right:** Proceed to Phase 4.

**Do not guess.** Do not try random fixes. Do not change multiple things at once.

## Phase 4: Implementation

1. **Write a failing test** that reproduces the bug.
2. **Fix with a single, targeted change.**
3. **Run ALL tests** (not just the new one) to confirm no regressions.
4. **Add defense-in-depth validation** to prevent this class of bug from recurring:
   - Entry point validation (reject bad input early)
   - Business logic assertions (verify assumptions explicitly)
   - Clear error messages that point to the cause

## The 3-Strikes Rule

If 3 consecutive fix attempts fail: **STOP.**

Before attempting a 4th fix, answer these questions:
- "Is this architecture fundamentally sound, or am I fighting the design?"
- "Am I fixing the root cause or a downstream symptom?"
- "Should I discuss this with the team before continuing?"

If you can't confidently answer all three, escalate to a human.

## Common Debugging Anti-Patterns

| Anti-Pattern | What To Do Instead |
|-------------|-------------------|
| Guessing and trying random fixes | Form a hypothesis, test one variable at a time |
| Changing multiple things at once | Revert all, change one thing, verify |
| Fixing the symptom not the cause | Trace backward to the source of bad data |
| "It works on my machine" | Check environment differences systematically |
| Adding try/catch to suppress errors | Fix the cause; errors exist for a reason |
| Reading the error too quickly | Read it word by word, including the full stack trace |
````

### 6. Generate First Directive

Create `directives/001_initial_setup.md`:
````markdown
# Directive 001: Initial Environment Setup

## Objective

Configure the development environment and verify all dependencies are working.

## Prerequisites

- Python 3.11+ installed
- PostgreSQL running (or configured DATABASE_URL)
- Git initialized

## Steps

### Step 1: Virtual Environment
```bash
python -m venv .venv
source .venv/bin/activate  # Linux/Mac
# or: .venv\Scripts\activate  # Windows
```

### Step 2: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 3: Configure Environment
```bash
cp .env.example .env
# Edit .env with your local values
```

### Step 4: Verify Database Connection
```bash
python execution/verify_setup.py
```

### Step 5: Run Initial Tests
```bash
pytest tests/ -v
```

## Acceptance Criteria

- [ ] Virtual environment created and activated
- [ ] All dependencies installed without errors
- [ ] `.env` file exists with valid configuration
- [ ] `verify_setup.py` passes all checks
- [ ] `pytest` runs (even if no tests exist yet)

## Development Methodology

Starting from Directive 002 onward, all work follows the processes defined in AGENTS.md:
- **Implementation Planning** before coding (Section 4)
- **TDD Iron Law** during coding (Section 3)
- **Review Gates** after each task (Section 5)
- **Verification Before Completion** before marking done (Section 6)

See `docs/methodology/` for detailed reference guides.

## Status: [ ] Incomplete / [ ] Complete

## Notes

[Agent: Add any issues encountered or decisions made]
````

### 7. Generate Verification Script

Create `execution/verify_setup.py`:
````python
#!/usr/bin/env python3
"""
Verify that the development environment is correctly configured.
Run this after initial setup to confirm everything works.
"""

import sys
from pathlib import Path

def check_python_version():
    """Verify Python version meets requirements."""
    required = (3, 11)
    current = sys.version_info[:2]
    if current < required:
        return False, f"Python {required[0]}.{required[1]}+ required, found {current[0]}.{current[1]}"
    return True, f"Python {current[0]}.{current[1]} ✓"

def check_env_file():
    """Verify .env file exists."""
    env_path = Path(".env")
    if not env_path.exists():
        return False, ".env file not found (copy from .env.example)"
    return True, ".env file exists ✓"

def check_required_dirs():
    """Verify project structure exists."""
    required = ["src", "tests", "docs", "directives", "execution", "docs/plans", "docs/methodology"]
    missing = [d for d in required if not Path(d).is_dir()]
    if missing:
        return False, f"Missing directories: {', '.join(missing)}"
    return True, "All directories exist ✓"

def check_docs():
    """Verify planning documents exist."""
    docs = ["docs/PRD.md", "docs/ARCH.md", "docs/RESEARCH.md"]
    missing = [d for d in docs if not Path(d).exists()]
    if missing:
        return False, f"Missing documents: {', '.join(missing)}"
    return True, "PRD.md, ARCH.md, and RESEARCH.md exist ✓"

def check_methodology():
    """Verify methodology documents exist."""
    docs = [
        "docs/methodology/implementation-planning.md",
        "docs/methodology/review-gates.md",
        "docs/methodology/debugging-guide.md",
    ]
    missing = [d for d in docs if not Path(d).exists()]
    if missing:
        return False, f"Missing methodology docs: {', '.join(missing)}"
    return True, "All methodology documents exist ✓"

def main():
    checks = [
        ("Python Version", check_python_version),
        ("Environment File", check_env_file),
        ("Directory Structure", check_required_dirs),
        ("Documentation", check_docs),
        ("Methodology", check_methodology),
    ]

    print("=" * 50)
    print("Environment Verification")
    print("=" * 50)

    all_passed = True
    for name, check_func in checks:
        passed, message = check_func()
        status = "✓" if passed else "✗"
        print(f"[{status}] {name}: {message}")
        if not passed:
            all_passed = False

    print("=" * 50)
    if all_passed:
        print("All checks passed! Environment is ready.")
        return 0
    else:
        print("Some checks failed. Please fix the issues above.")
        return 1

if __name__ == "__main__":
    sys.exit(main())
````

### 8. Generate IDE Configuration

Create `.cursorrules` (or `.windsurfrules`):
````
# Cursor AI Rules for [Project Name]

## Session Start Protocol
ALWAYS read these files at the start of EVERY session:
1. AGENTS.md (this project's conventions, workflow, and enforcement rules)
2. docs/ARCH.md (technical architecture and constraints)
3. docs/RESEARCH.md (proven patterns and anti-patterns)
4. directives/ (find your current task)

## Code Generation Rules
- Use ONLY technologies listed in docs/ARCH.md Tech Stack
- Follow directory structure defined in docs/ARCH.md
- Use domain terms EXACTLY as defined in ARCH.md Dictionary
- Write tests BEFORE implementation (TDD Iron Law)
- Create implementation plans BEFORE coding (AGENTS.md Section 4)
- Pass both review gates BEFORE marking tasks done (AGENTS.md Section 5)

## Forbidden Actions
- Do NOT install packages not listed in requirements.txt without approval
- Do NOT create files outside the defined directory structure
- Do NOT deviate from API contracts in ARCH.md
- Do NOT use .tmp/ for anything except temporary planning notes
- Do NOT write production code before its failing test
- Do NOT claim completion without running verification commands
````

---

## Script Structure

The final `setup_launchpad.py` should:
````python
#!/usr/bin/env python3
"""
Launchpad Setup Script
Creates project scaffold based on PRD.md, ARCH.md, and RESEARCH.md
"""

import os
from pathlib import Path

# Configuration extracted/summarized from docs
PROJECT_NAME = "..."  # From PRD
DIRECTORIES = [...]    # Base + from ARCH.md
# ... etc

def create_directories():
    ...

def create_gitignore():
    ...

def create_env_example():
    ...

def create_readme():
    ...

def create_requirements():
    ...

def create_agents_md():
    ...

def create_methodology_docs():
    ...

def create_initial_directive():
    ...

def create_verify_script():
    ...

def create_ide_config():
    ...

def main():
    print(f"Initializing {PROJECT_NAME}...")
    create_directories()
    create_gitignore()
    create_env_example()
    create_readme()
    create_requirements()
    create_agents_md()
    create_methodology_docs()
    create_initial_directive()
    create_verify_script()
    create_ide_config()
    print("Genesis complete! Run: python execution/verify_setup.py")

if __name__ == "__main__":
    main()
````

---

## Output

Provide ONLY the complete Python code block for `setup_launchpad.py`, customized to the specific PRD and ARCH content.

---

## Validation Checklist

Before providing the script:

- [ ] Script reads from existing docs/, does not embed content
- [ ] All directories from ARCH.md are created (including `docs/plans/` and `docs/methodology/`)
- [ ] `.gitignore` covers the tech stack (Python, Node, etc.)
- [ ] `.env.example` includes all secrets from ARCH.md Security section
- [ ] `README.md` references correct project name, structure, and methodology docs
- [ ] `requirements.txt` matches ARCH.md Tech Stack versions
- [ ] `AGENTS.md` includes all 11 sections (Prime Directive through Commit Format), including TDD Iron Law, Implementation Planning, Review Gates, Verification Before Completion, Systematic Debugging, and Anti-Rationalization Rules
- [ ] Methodology docs generated: `implementation-planning.md`, `review-gates.md`, `debugging-guide.md`
- [ ] First directive references the methodology and enforcement patterns
- [ ] Verification script checks for methodology docs
- [ ] IDE config enforces TDD and review gates
