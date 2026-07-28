---
name: agent-guide
version: 3.1.0
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
3. Check `.agents/tasks/` for a log matching the current task; open it if it exists, create one if the task warrants it
4. State your planned actions out loud before touching any file
5. Work through tasks one at a time; check each off when done

---

## Project Documents

Four things keep the project clean and ready for handover at any time. Create any that are missing before starting work.

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

### `.agents/tasks/<date>-<title>.md` — Task Log

Working scratch space for a single task: findings, investigation notes, in-progress reasoning, the step-by-step plan. Not polished, not reviewed — a running record of what happened and why, kept separate from `PLAN.md` (what's left to do) and `AGENTS.md` (settled conventions and decisions).

**How to use:**
- Create one for any task involving investigation, a non-trivial plan, or likely to span multiple turns — skip it for trivial one-shot edits
- Filename: `.agents/tasks/2025-01-15-jwt-refresh.md` (date + short slug)
- Append as you go: findings, blockers hit, approaches tried and rejected (and why), decisions made
- On completion: fold anything durable into `AGENTS.md` (decisions/conventions) or `PLAN.md` (status) — leave the log itself in place as history, don't delete it

**Format:**
```markdown
# 2025-01-15 — JWT refresh on 401

## Goal
What this task is trying to achieve.

## Findings
Investigation notes, relevant code paths, constraints discovered.

## Plan
- [ ] step
- [ ] step

## Decisions
Choices made mid-task and why, especially anything not obvious from the diff.
```

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

## Code Review

Code is a draft until reviewed. Before committing or checking off a task, review the diff twice as a skeptical outside reviewer — not the author confirming their own work.

**Check:** correctness (edge cases, nulls, races) · matches what the task actually asked, not just the happy path · security (input validation, secrets, auth) · error handling (nothing swallowed) · consistency with `AGENTS.md` · no scope creep.

Find something → fix it → restart from pass one. Two consecutive clean passes required. The second pass actively hunts for a bug rather than confirming the first pass was fine.

## Verification

- Run tests covering the changed code (full suite if fast)
- No tests → run type-checker, linter, or build as the minimum bar
- Can't be automated (UI, config, manual flow) → describe the actual manual check performed, not "looks fine"
- State plainly what was verified and what wasn't

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
- [ ] Task log in `.agents/tasks/` updated or closed out (durable bits folded into `AGENTS.md`/`PLAN.md`)
- [ ] `git status` reviewed with the user; uncommitted changes are expected unless the user asked for a commit
- [ ] `AGENTS.md` updated — new conventions, decisions, env vars, APIs, models
- [ ] `README.md` updated — new scripts, env vars, setup changes
- [ ] `.env.example` current with all new variables
- [ ] No console errors or type errors
- [ ] UI changes verified responsive and meet the premium design standard
