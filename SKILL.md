# Agent-Friendly Codebase

<!-- Skill: agent-friendly-codebase -->
<!-- Use at the START of any project or when joining any codebase. -->
<!-- This skill transforms any repository into an agent-friendly codebase — -->
<!-- one where every future agent session can work reliably without human hand-holding. -->

You are now operating under the **Agent-Friendly Codebase** methodology.

This is NOT a reference document. This is your operating system. Every action you take in this codebase must comply with these rules. Violations are not tolerated — if you catch yourself breaking a rule, you must stop, undo, and redo correctly.

## PRIME DIRECTIVE

> **The repo must do the work, not the human.**
>
> If a brand-new agent session opens this repo with zero chat history and can successfully complete a task — the repo is agent-friendly. If it can't — you have failed.

Every file you create, every decision you make, every line you write must serve this test:

**"Can the next agent — who has never seen this conversation — pick up exactly where I left off?"**

If the answer is no, you are not done.

---

## PHASE 1: PROJECT INITIALIZATION

When this skill is loaded on a NEW or EXISTING project, you MUST execute these steps BEFORE writing any feature code. Do not ask the user for permission. Do not skip steps. Do not "plan to do it later."

### Step 1: Detect Project Context

Analyze the repository to determine:
- **Language/runtime** (Go, TypeScript, Python, Java, Rust, etc.)
- **Existing framework** (Next.js, Express, Gin, FastAPI, Spring, etc.)
- **Existing package manager** (npm, pnpm, yarn, go mod, pip, cargo, etc.)
- **Existing test framework** (jest, vitest, pytest, go test, etc.)
- **Existing structure** (is there already a clear architecture, or is it a mess?)

If the project is empty, ask the user ONCE: "What are you building and in what language?" Then proceed.

### Step 2: Create Directory Architecture

Based on the detected language and project type, create the following structure. Choose the appropriate variant:

**For backend / API / complex-logic projects → Hexagonal Architecture:**

```
src/ (or internal/ for Go)
├── domain/          # Pure business logic. ZERO external dependencies.
│   ├── entities/    # Core business objects
│   ├── values/      # Value objects (immutable)
│   └── policies/    # Business rules and validations
├── app/             # Use cases. Orchestrates domain objects.
│   ├── usecases/    # One file per use case
│   └── ports/       # Interface definitions (what the app NEEDS from outside)
├── adapters/        # Infrastructure. Implements ports.
│   ├── http/        # Web handlers / API routes
│   ├── persistence/ # Database implementations
│   └── external/    # Third-party API clients
└── main.ts (or main.go, main.py, etc.)  # Composition root — wires everything together
```

**For frontend apps (React / Vue / Next.js / etc.) → Feature-Based Architecture:**

```
src/
├── features/              # One folder = one user-facing feature
│   ├── auth/
│   │   ├── components/    # UI components scoped to this feature
│   │   ├── hooks/         # Feature-specific hooks/composables
│   │   ├── services/      # API calls, business logic
│   │   ├── types.ts       # Feature-specific types
│   │   ├── auth.test.ts   # Tests
│   │   └── index.ts       # Public API of this feature
│   └── dashboard/
│       └── ...same pattern
├── shared/                # Cross-feature (MINIMAL — UI primitives, utils)
│   ├── components/        # Shared UI components (Button, Modal, etc.)
│   ├── hooks/             # Shared hooks
│   └── lib/               # Shared utilities
├── app/                   # App shell (routing, providers, layout)
└── main.tsx
```

**For fullstack → Backend hexagonal + Frontend feature-based. Separate directories or monorepo packages.**

**For monorepo → Each package/service gets its OWN `AGENTS.md` and follows the pattern independently. Root `AGENTS.md` describes the overall structure and cross-package rules.**

**For EXISTING projects with messy structure → Do NOT force a full rewrite.** Instead:
1. Create `AGENTS.md` and `Makefile` immediately (zero-risk, pure addition).
2. Create `.agent/` directory immediately.
3. For NEW files going forward, place them in the correct architecture.
4. Mark existing messy areas with `TODO(arch): migrate to domain/app/adapters` comments.
5. Refactor existing code incrementally — one module per session, with tests.

Create the directories NOW. Add a `.gitkeep` in each empty directory. If you cannot create directories (e.g., no terminal access), describe the exact structure to the user and ask them to create it, then verify.

### Step 3: Create AGENTS.md

Create `AGENTS.md` in the project root with REAL content based on what you detected in Step 1. Do NOT leave any brackets or placeholders — every line must contain actual project information.

Example (for a Python meeting-notes app):

```markdown
# AGENTS.md

## Project
MeetMind — AI-powered meeting transcription and summarization tool.
Language: Python 3.12. Framework: FastAPI.
Architecture: Hexagonal.

## Architecture Rules
- `domain/` (or `entities/`) contains pure business logic with ZERO external imports.
- `app/` contains use cases that depend ONLY on domain.  
- `adapters/` implements ports defined in `app/ports/`.
- `main` is the composition root — the ONLY place that wires adapters to ports.
- **Dependency direction: domain ← app ← adapters ← main. Never reversed.**

## Commands
- `make bootstrap` — Install all dependencies, set up environment
- `make test` — Run ALL tests (unit + integration)
- `make lint` — Run linter and format check  
- `make run` — Start the application locally

These are the ONLY commands you may use. Do NOT invent scripts.

## How to Make Changes
1. Identify which USE CASE owns this behavior (look in `app/usecases/`)
2. Write a FAILING test first (in the same directory as the code)
3. Implement domain/app changes with MINIMAL code to pass the test
4. Update adapters ONLY if the change requires infrastructure changes
5. Run `make test` and `make lint` — both must pass
6. Write a commit message that states WHICH LAYER changed and WHY

## Testing
- domain/ changes → unit test in domain/ (pure, no mocks, no IO)
- app/ changes → unit test in app/ (mock ports)
- adapters/ changes → integration test (only when necessary)
- New code without tests = REJECTED

## Forbidden
- NEVER import adapters from domain or app
- NEVER skip tests
- NEVER commit without running `make test`
- NEVER put business logic in adapters
- NEVER create files outside the architecture structure
- NEVER modify AGENTS.md without explicit human approval
```

### Step 4: Create Makefile (Golden Path)

Create a `Makefile` in the project root that wraps the actual build tools. Detect the language and write REAL commands:

**TypeScript/Node.js example:**
```makefile
.PHONY: bootstrap test lint run check clean

bootstrap:
	npm ci

test:
	npm test

lint:
	npm run lint

run:
	npm start

check: lint test
	@echo "✅ All checks passed"

clean:
	rm -rf dist/ node_modules/.cache coverage/
```

**Go example:**
```makefile
.PHONY: bootstrap test lint run check clean

bootstrap:
	go mod download

test:
	go test ./...

lint:
	golangci-lint run

run:
	go run ./cmd/app/

check: lint test
	@echo "✅ All checks passed"
```

**Python example:**
```makefile
.PHONY: bootstrap test lint run check clean

bootstrap:
	pip install -r requirements.txt

test:
	pytest

lint:
	ruff check . && ruff format --check .

run:
	python -m app

check: lint test
	@echo "✅ All checks passed"
```

Write the REAL Makefile for the detected language. Not a template.

**Windows compatibility:** If the project runs on Windows where `make` is unavailable, create equivalent `npm scripts` in `package.json`, or a `Justfile` (https://github.com/casey/just), or PowerShell scripts in `scripts/`. The key is: there must be 4 fixed entry commands — bootstrap, test, lint, run — that every agent uses. The tool doesn't matter; the contract does.

### Step 5: Create Boundary Enforcement

Add lint rules that PREVENT architecture violations at compile/lint time:

**TypeScript** — add to `.eslintrc.json` or `eslint.config.js`:
```json
{
  "overrides": [
    {
      "files": ["src/domain/**"],
      "rules": {
        "no-restricted-imports": ["error", {
          "patterns": [{ "group": ["**/adapters/**", "**/app/**"], "message": "domain MUST NOT import app or adapters" }]
        }]
      }
    },
    {
      "files": ["src/app/**"],
      "rules": {
        "no-restricted-imports": ["error", {
          "patterns": [{ "group": ["**/adapters/**"], "message": "app MUST NOT import adapters" }]
        }]
      }
    }
  ]
}
```

**Go** — add `depguard` rules to `.golangci.yml`.

**Python** — add `import-linter` config to `pyproject.toml`.

If the language doesn't have a compile-time check, add a `make arch-check` target that greps for violations.

### Step 6: Create Session Continuity Infrastructure

Create the `.agent/` directory and seed the session log. **This directory MUST be committed to git** — it is the cross-session handoff mechanism. Without it in the repo, the next agent starts from zero.

```
.agent/
├── session-log.md    # Append-only log of every session's work
└── decisions.md      # Append-only log of architectural decisions
```

Add to `.agent/session-log.md`:
```markdown
# Session Log

<!-- Every agent session MUST append to this file before ending. -->
<!-- Format: -->
<!-- ## [Date] [Time] — [Summary] -->
<!-- ### Done: [what was completed] -->
<!-- ### Pending: [what remains] -->  
<!-- ### Decisions: [any choices made and why] -->
<!-- ### Next: [what the next session should do first] -->
```

Add to `.agent/decisions.md`:
```markdown
# Architectural Decision Log

<!-- Append-only. Never delete entries. -->
<!-- Format: -->
<!-- ## ADR-[N]: [Title] -->
<!-- **Date:** [date] -->
<!-- **Decision:** [what was decided] -->
<!-- **Rationale:** [why] -->
<!-- **Alternatives rejected:** [what else was considered] -->
```

### Step 7: Verify Initialization

After completing steps 1-6:

**Create a seed test file** so the test runner has something to execute. This prevents "no tests found" errors on empty projects:

- **Python:** `tests/test_smoke.py` with `def test_project_starts(): assert True`
- **TypeScript/Jest:** `src/__tests__/smoke.test.ts` with `test('project starts', () => expect(true).toBe(true))`
- **Go:** `smoke_test.go` with `func TestSmoke(t *testing.T) {}`

Then run:
```bash
make bootstrap
make test
make lint
```

All three MUST pass. If any fail, fix them NOW before proceeding. Do NOT skip this — a broken golden path means every future session starts with confusion.

**Report to the user:**
```
✅ Agent-Friendly Codebase initialized:
- Architecture: [Hexagonal/Feature-Based]
- Language: [detected]
- AGENTS.md: created
- Golden Path: make bootstrap/test/lint/run
- Boundary enforcement: [ESLint rules/depguard/grep]
- Session continuity: .agent/ directory ready

Any agent that opens this project will automatically know:
- Where to put code
- How to test it
- What's forbidden
- What happened in previous sessions
```

---

## PHASE 2: EVERY-SESSION PROTOCOL

These rules apply to EVERY agent session — including the first one after initialization, and every subsequent one. This is not optional. This is not "best practice." This is mandatory.

### On Session Start (BEFORE doing anything else)

```
1. READ AGENTS.md              → Understand the project contract
2. READ .agent/session-log.md  → Know what the last session did
3. READ .agent/decisions.md    → Know why things are the way they are
4. RUN make test               → Confirm the project is healthy
5. If make test FAILS          → Fix it FIRST before doing anything else
```

If you cannot execute terminal commands, ask the user to run `make test` and report the result. Do NOT proceed until you have confirmation that tests pass.

If any of these files don't exist, you are in an uninitialized project. Go back to PHASE 1.

### The Iron Laws of Every Change

These are absolute. No exceptions. No "just this once." No "it's too simple for this."

#### LAW 1: TEST FIRST

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST.

If you wrote code before writing a test:
→ DELETE the code
→ Write the test
→ Watch it FAIL
→ THEN write the code
→ Watch it PASS

"Delete" means delete. Not comment out. Not "keep for reference." Delete.
```

#### LAW 2: MINIMAL DIFF

```
Every change should touch THE FEWEST FILES POSSIBLE.

If your change touches > 3 files → STOP. Rethink boundaries.
If your change touches > 5 files → Something is architecturally wrong. 
                                    Do NOT proceed. Fix the architecture first.
```

#### LAW 3: LAYER DISCIPLINE

```
Before writing ANY code, answer: "Which layer does this belong to?"

FOR BACKEND (Hexagonal):
- Is it a business rule/validation?     → domain/
- Is it orchestrating domain objects?   → app/usecases/
- Is it talking to a database/API/UI?   → adapters/
- Does it need a new external contract? → First define the port in app/ports/,
                                          THEN implement in adapters/

FOR FRONTEND (Feature-Based):
- Is it specific to ONE feature?        → features/[that-feature]/
- Is it a reusable UI primitive?        → shared/components/
- Is it app-level wiring (routes/providers)? → app/
- Does it call an external API?         → features/[feature]/services/

If you can't answer this question → You don't understand the requirement yet.
                                     Go back and clarify.
```

#### LAW 4: VERIFY BEFORE CLAIMING DONE

```
You are NOT done until:
□ make test    → ALL PASS
□ make lint    → ZERO violations
□ Every new function has a test
□ No TODO/FIXME/HACK left behind
□ Commit message states which layer changed and why

"I think it works" is NOT verification. Evidence or it didn't happen.
```

#### LAW 5: LEAVE A TRAIL

```
Before ending your session, you MUST append to .agent/session-log.md:

## [Today's Date] — [One-line summary]

### Done
- [List what you completed, with file paths]

### Pending  
- [List what's not finished]

### Decisions
- [Any architectural choices you made, and WHY]

### Next
- [What the next session should do first]

If you don't write this → the next agent starts from zero → 
you have FAILED the prime directive.
```

---

## PHASE 3: THE CHANGE WORKFLOW

When the user asks you to implement something, follow this exact sequence. Do not skip steps. Do not reorder.

```
┌─────────────────────────────────────────────────┐
│ 1. UNDERSTAND                                    │
│    - Restate the requirement in your own words   │
│    - Identify: which layer? which files?         │
│    - If > 3 files affected → decompose further   │
├─────────────────────────────────────────────────┤
│ 2. PLAN (silently — do NOT dump plans to user)   │
│    - List the exact files to create/modify       │
│    - Define the test for each change             │
│    - Estimate: should be 2-3 files max           │
├─────────────────────────────────────────────────┤
│ 3. TEST (RED)                                    │
│    - Write a failing test that describes         │
│      the desired behavior                        │
│    - Run it. Confirm it FAILS.                   │
│    - If it passes → your test is wrong. Rewrite. │
├─────────────────────────────────────────────────┤
│ 4. IMPLEMENT (GREEN)                             │
│    - Write the MINIMUM code to pass the test     │
│    - No extra features. No "while I'm here..."   │
│    - Run the test. Confirm it PASSES.            │
├─────────────────────────────────────────────────┤
│ 5. REFACTOR (optional)                           │
│    - Clean up only if needed                     │
│    - Tests must stay green throughout            │
├─────────────────────────────────────────────────┤
│ 6. VERIFY                                        │
│    - make test  → ALL PASS                       │
│    - make lint  → ZERO violations                │
├─────────────────────────────────────────────────┤
│ 7. COMMIT                                        │
│    - Message format:                             │
│      [layer](scope): description                 │
│      Examples:                                   │
│        domain(pricing): add discount policy      │
│        app(orders): create order use case        │
│        adapters(db): implement order repository  │
├─────────────────────────────────────────────────┤
│ 8. LOG                                           │
│    - Append to .agent/session-log.md             │
│    - If you made an architectural decision →     │
│      Append to .agent/decisions.md               │
└─────────────────────────────────────────────────┘
```

---

## COMMON RATIONALIZATIONS (and why they're wrong)

| What you might think | Why it's wrong | What to do instead |
|---|---|---|
| "This is too simple for a test" | Simple code has the most long-lived bugs | Write the test. It takes 30 seconds. |
| "I'll add tests later" | You won't. The next agent won't. | Write the test NOW. |
| "I need to refactor first" | Refactoring without tests = gambling | Write tests for current behavior first. |
| "This doesn't fit the architecture" | Then the architecture needs a new port | Define the port, then implement the adapter. |
| "I'll just put this in utils/" | `utils/` is where code goes to die | Find the right layer. If it's shared logic, it's domain. |
| "The user wants it fast" | Fast + broken = slower than slow + correct | Follow the process. It IS the fastest path. |
| "I don't need to log this session" | The next agent will waste 30 min figuring out what you did | Write the log. It takes 2 minutes. |

---

## DETECTING ENTROPY (Red Flags)

If you observe ANY of these, STOP and fix before proceeding:

| 🚨 Red Flag | What it means | Action |
|---|---|---|
| File in wrong layer | Architecture violation | Move it to correct layer + add lint rule |
| `domain/` importing from `adapters/` | Critical boundary breach | Extract a port interface immediately |
| Test requires database/HTTP to run | Test is at wrong level | Rewrite as unit test with mocked port |
| Change touches > 5 files | Feature is too coupled | Decompose into smaller changes |
| No tests for new code | TDD was skipped | Delete code, write test first, reimplementation |
| `utils/`, `helpers/`, `misc/` directories | Architectural smell | Redistribute into proper layers |
| `.agent/session-log.md` is empty/stale | Session continuity broken | Reconstruct from git log, then maintain going forward |
| `make test` takes > 2 minutes | Test suite is too heavy | Split into fast unit tests + separate integration suite |

---

## ARCHITECTURE DECISION QUICK REFERENCE

When you need to decide where code goes:

```
"Where does this code belong?"

BACKEND (Hexagonal):
  Is it a pure business rule (no IO, no framework)?   → domain/
  Does it orchestrate domain objects for a user goal?  → app/usecases/
  Does it define what the app NEEDS from outside?      → app/ports/ (as an interface)
  Does it talk to a database, API, queue, or filesystem? → adapters/
  Does it wire everything together at startup?         → main

FRONTEND (Feature-Based):
  Does it belong to a specific user-facing feature?    → features/[feature]/
  Is it a reusable UI building block (Button, Modal)?  → shared/components/
  Is it a shared utility (formatting, validation)?     → shared/lib/
  Is it routing, providers, or app-level layout?       → app/

BOTH:
  None of the above?  → You probably don't need it. YAGNI.
```

---

## SUMMARY

This skill makes you a reliable, consistent, trustworthy engineering agent. Not by making you smarter, but by making the codebase impossible to misunderstand.

**What the user gets:**
- Every session picks up where the last one left off
- Code is always in the right place
- Every change has a test
- Every change is small and reviewable
- The project never rots

**What you must do:**
1. Initialize the project structure (PHASE 1) — once
2. Follow the session protocol (PHASE 2) — every time
3. Follow the change workflow (PHASE 3) — for every change
4. Never skip, never shortcut, never rationalize

The process IS the product.
