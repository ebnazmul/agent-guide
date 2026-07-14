---
name: agent-guide
version: 3.0.0
description: >
  Universal execution protocol for AI agents on any frontend project.
  Activate at the start of every coding session — before writing or modifying
  any code. Triggers on: "start coding", "begin session", "pick up where we left off",
  "continue the project", "let's build", "what's next", or any time a new task begins.
  Also activate when updating AGENTS.md, PLAN.md, README.md, or any project doc.
license: MIT
---

# AGENT-GUIDE

Read fully before touching any file.

---

## Boot Sequence

Every session starts here — no exceptions.

1. Read `AGENTS.md` — conventions, architecture, decisions
2. Read `PLAN.md` — find the next unchecked task
3. State your planned actions out loud before touching any file
4. Work through tasks one at a time; check each off when done

---

## Project Documents

Three files keep the project clean and ready for handover at any time. Create any that are missing before starting work.

---

### `AGENTS.md` — Single Source of Truth

Everything an agent or developer needs to understand and work on this project.

**Sections:**

- **Project** — one-paragraph description of what it does and who it's for
- **Stack** — framework, language, styling, state, build tool, package manager (with versions)
- **Structure** — annotated folder tree
- **Conventions** — naming, design tokens, component rules, state/API rules
- **Environment** — table of all `.env` variables (key, purpose, required)
- **Do Not** — explicit prohibitions; concrete, not vague
- **Decisions** — date-stamped log of architectural choices and their reasoning
- **API Reference** — endpoint definitions: method, path, request body, response shape, error codes
- **Data Models** — TypeScript interfaces for all core entities
- **Component API** — prop interfaces for all shared UI primitives

**Update when:** a convention changes, an architectural decision is made, a new env variable is added, an endpoint is built, a data model changes, a shared component is created.

**Format:** headers, tables, fenced code blocks. Every rule is a concrete action. Professional English only.

---

### `PLAN.md` — Task Board

The single ordered list of work. Tasks are written out upfront, executed one at a time, and marked as done immediately upon completion.

**How to use:**
- At session start: review the list; the first unchecked item is the current task
- During work: do one task at a time — never start the next until the current is checked off
- New tasks discovered mid-session: append to the bottom, do not interrupt the current task
- Completed tasks: mark `[x]` and add the completion date inline

**Format:**
```markdown
# PLAN.md

## Active

- [x] feat(auth): scaffold login form — done 2025-01-15
- [x] feat(auth): wire up API call and error states — done 2025-01-15
- [ ] feat(auth): add JWT refresh on 401 — in progress
- [ ] feat(dashboard): build weekly stats chart
- [ ] fix(nav): mobile menu does not close on route change

## Backlog

- [ ] Add dark mode support
- [ ] Write integration tests for checkout flow

## Blocked

- [ ] feat(payments): integrate Stripe — waiting on API keys from client
```

**Rules:**
- Tasks use the same `type(scope): description` format as git commits
- One task = one logical unit of work = one commit
- Never batch unrelated work under a single task

---

### `README.md` — Project Overview

Enough for anyone to go from zero to running with no prior context.

**Sections:** one-line description, prerequisites with versions, quick-start commands, environment variables table, available scripts table, folder structure, deployment notes.

**Update when:** a new env variable is added, a script changes, prerequisites change, deployment changes.

---

## Code Standards

### Design Tokens

All colors, spacing, and typography must use named variables. Never hardcode raw values.

```css
/* ✅ */  color: var(--color-primary);
/* ❌ */  color: #4f46e5;
```

### Premium UI

| Concern | Standard |
|---|---|
| Color | Curated palettes — no plain browser primaries |
| Depth | Layered surfaces, subtle shadows, translucency where appropriate |
| Motion | Smooth transitions on all interactive elements |
| Typography | Strong hierarchy, consistent scale, no browser defaults |
| Responsiveness | Verified at mobile (≤768px), tablet (≤1024px), desktop |

### Components

1. **Single responsibility** — one job per component; pass callbacks as props
2. **Reuse first** — check existing primitives before creating anything new
3. **No magic values** — tokens, variables, or named constants only
4. **Stateless display** — typed props; lift state up
5. **Organized** — group by domain/feature; never dump into root

### API & State

- Always use the project's shared API client — never raw `fetch`/`axios` in components
- One store per domain; keep stores flat
- All environment values live in `.env`

---

## Code Review Loop

Writing the code is not the end of the task — it's a draft. Run this loop before touching git or marking anything done in `PLAN.md`:

1. **Re-read the diff fresh**, as an independent reviewer would, against these checks specifically:
   - **Correctness** — does it actually do what the task asked, including edge cases (empty states, null/undefined, zero, off-by-one, race conditions)?
   - **Requirement match** — re-read the original task; does the implementation satisfy all of it, not just the happy path?
   - **Security** — input validation, injection risk, auth/permission checks, secrets not hardcoded or logged
   - **Error handling** — failures surfaced, not swallowed; no unhandled promise rejections
   - **Consistency** — matches existing patterns/conventions in `AGENTS.md` and the surrounding code, no stray style
   - **Scope** — no unrelated changes crept in
2. **Issue found** → fix it, then restart at step 1 with a fresh read.
3. **No issue found** → run a second pass, actively trying to find something wrong rather than confirming the first pass was fine (assume there's a bug and hunt for it).
4. **Second pass also clean** → proceed to Verification.

Two consecutive clean passes are required before code counts as reviewed. One clean pass is never sufficient.

## Verification

- Run the test suite if it exists and is fast; at minimum run tests covering the changed code.
- No tests exist → run the type-checker, linter, or build as a minimum bar.
- Can't be automated (UI, config, manual flow) → describe the manual check actually performed, not just "looks fine."
- Report explicitly what was verified and what wasn't — don't imply full coverage if it wasn't achieved.

---

## Project Hygiene

- `.env` holds all secrets — never commit it; always in `.gitignore`
- `.env.example` mirrors every key with a placeholder and one-line comment
- Every page has a descriptive `<title>` and `<meta name="description">`
- Semantic HTML throughout — `<main>`, `<nav>`, `<article>`, `<section>`
- All interactive elements are keyboard-navigable with visible focus states
- Images have meaningful `alt` text; decorative images use `alt=""`
- WCAG AA color contrast minimum

---

## Git Protocol

**Never commit automatically.** Work stays uncommitted until the user explicitly asks for a commit ("commit this", "commit and push", etc.). Finishing a task does not imply a commit.

When the user does ask to commit:

```bash
git add <specific-files>
git commit -m "<type>(<scope>): <what and why>"
```

| Type | Use |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, no logic change |
| `refactor` | Restructure, no behavior change |
| `chore` | Tooling, config, dependencies |

✅ `feat(auth): add JWT refresh on 401 response`  
❌ `fix stuff` / `update` / `changes`

If several completed tasks are sitting uncommitted when the user asks to commit, split them into separate commits (one task = one commit) rather than bundling.

---

## Session Checklist

Before ending any session:

- [ ] Current task checked off in `PLAN.md`
- [ ] `git status` reviewed with the user; uncommitted changes are expected unless the user asked for a commit
- [ ] `AGENTS.md` updated — new conventions, decisions, env vars, APIs, models
- [ ] `README.md` updated — new scripts, env vars, setup changes
- [ ] `.env.example` current with all new variables
- [ ] No console errors or type errors
- [ ] UI changes verified responsive and meet the premium design standard
