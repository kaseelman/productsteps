---
name: coding-standards
description: Apply your standard coding approach to all development tasks. Use this skill before spawning any coding agent or writing code for any project. Triggers on any coding, implementation, or build task. Encodes the working pattern: logical chunks, documented changes, commented code, automated tests.
---

# Coding Standards Skill

Apply this whenever writing or delegating code.

## Core Approach

1. **Implement in logical chunks** — not one giant PR. Each chunk = one cohesive piece of functionality that can be tested independently and reviewed clearly.
2. **Document all changes** — every PR/commit gets a clear description of what changed and why. Write it in the commit message or a CHANGES.md entry.
3. **Comment the code** — inline comments should explain *what you're doing and why*, not just what the code says. Especially on non-obvious logic, AI pipeline calls, auth flows, and DB queries.
4. **Automated tests** — every feature and bug fix ships with tests. Minimum: happy path + one error/edge case. Preferred: unit test per function, integration test per route/API.

## Chunking Strategy

Break work into phases or stories and implement each fully before moving on:
- Schema / DB migration → test migration
- API route → test route
- UI component → test component behaviour
- Integration → test end-to-end flow

## Comment Style

```typescript
// ✅ Good — explains intent, not mechanics
// Supabase RLS ensures users can only access their own projects.
// Service role key is used here for admin operations only.
const supabase = createServerClient(serviceRoleKey);

// ❌ Bad — restates the code
// Create a supabase client
const supabase = createServerClient(serviceRoleKey);
```

## Test Requirements

For Next.js / TypeScript projects:
- Unit tests: `*.test.ts` / `*.test.tsx` using Vitest or Jest
- API route tests: test request/response shape and error handling
- Component tests: test render, user interactions, and loading/error states
- Do not mark a feature done until tests pass

## Git Workflow

### Branch Discipline
- Every task gets its own branch. No exceptions.
- Branch naming: `feat/[description]` or `fix/[description]`
- **Never commit to `main` directly** — main is always clean and deployable
- **Never merge your own PR** — all agent-generated PRs require human review before merge

### Git Worktrees for Parallel Work
When multiple tasks run in parallel, use git worktrees to keep them fully isolated:

```bash
# Create a worktree for a new feature (do this before spawning the agent)
git worktree add ../[project]-[task-slug] feat/[task-slug]

# Example
git worktree add ../productbuilders-feat-auth feat/user-auth
```

Each agent gets its own worktree directory. They cannot touch each other's files. Review each worktree independently and merge when ready.

Clean up worktrees after merging:
```bash
git worktree remove ../[project]-[task-slug]
```

### PR Discipline
- PR title: `feat:` / `fix:` / `refactor:` prefix
- PR description: what changed, why, how to test it
- Link the relevant task or issue
- The agent's job ends when it opens the PR. Niko's job begins at review.

## Quality Gates

Every PR must pass all gates before it is reviewable. Configure CI to enforce these:

```
PR opened →
  lint (ESLint/Biome) →
  type check (tsc --noEmit) →
  unit tests (Vitest/Jest) →
  build (next build / tsc) →
  ← all pass → ready for review
```

Never skip the quality gate stack. An agent that can't pass lint and types has produced broken code regardless of whether it looks right.

For higher-stakes changes, add:
- Integration tests
- E2E tests (Playwright/Cypress)

## Code Review Standards

When reviewing agent-generated code, check:
- **Logic correctness** — does it actually do what the spec says?
- **Edge cases** — empty input, null values, concurrent requests, large datasets
- **Scope creep** — did the agent do more than asked? Useful sometimes; risky often.
- **Test quality** — are tests verifying behaviour, or just testing implementation details?
- **Consistency** — does it follow the patterns established in the rest of the codebase?

**Give precise feedback.** "This is wrong" produces a random rewrite. Specific feedback produces targeted fixes:

```
❌ "The pagination is broken."
✅ "The offset is wrong — should be (page - 1) * limit, not page * limit. See src/api/users.ts:47 for the existing pattern."
```

## Context Management for Long Tasks

Long coding sessions fill context windows. Add this instruction to every coding agent spawn:

> Compact context after each major milestone before proceeding to the next phase.

At milestone boundaries (schema done, API done, UI done), the agent should summarise what was built and discard raw implementation history before continuing. This preserves working memory for the next phase.

## Agent Spawn Template

When spawning a coding agent (Claude Code), always include:

```
You are a senior [stack] engineer working on [Project Name].

## Repo
- Location: ~/Desktop/[project]/
- Worktree for this task: ~/Desktop/[project]-[slug]/
- Branch: feat/[slug] (already created — work here only)
- Stack: [stack details]
- Tests: [test framework]
- CI: GitHub Actions (runs on every PR)

## Rules
- Work only in the provided worktree — never touch main or other branches
- Write tests alongside implementation (not after)
- Comment non-obvious code to explain intent, not mechanics
- Run `[lint command] && [typecheck command] && [test command]` before opening a PR
- If you discover out-of-scope work, open a separate issue — don't include it in this PR
- Do NOT merge this PR — open it and stop. Human review is required.
- Compact context after each major milestone before proceeding

## When done
1. Run the full quality gate (lint → typecheck → tests → build)
2. Open a PR: clear title, description of what changed and why, how to test
3. Set task status to "review" in the task registry
4. Run: openclaw system event --text "Done: [brief summary]" --mode now

## Communication
- If blocked or something is unclear, ask before proceeding
- Don't guess at business logic — verify with existing tests or ask

Work in logical chunks. After each chunk:
1. Write/update tests
2. Comment non-obvious logic
3. Commit with a clear message
Do not move to the next chunk until the current chunk's tests pass.
```
