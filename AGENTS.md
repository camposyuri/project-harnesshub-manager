# HarnessHub Manager

React 18, TypeScript, TailwindCSS, Zustand, Axios, React Router v6, React Hook Form + Zod. REST API backend with JWT auth.

## Commands

- `npm run dev`: Dev server (port 5173)
- `npm run build`: Production build
- `npm test`: Vitest test suite
- `npm run lint`: ESLint check
- `npm run typecheck`: TypeScript type check — run before every commit

## Architecture

- `/src/api/`          Axios instance + one service file per entity
- `/src/features/`     Feature modules (`contacts/`, `accounts/`, `deals/`, `activities/`, `pipeline/`)
- `/src/components/`   Shared UI (`common/`) and CRM-specific (`crm/`) components
- `/src/hooks/`        Custom hooks
- `/src/pages/`        Route-level page components
- `/src/store/`        Zustand stores
- `/src/types/`        TypeScript interfaces and enums — no inline type definitions elsewhere
- `/src/utils/`        Formatters, helpers, constants

## Code Style

- TypeScript for all new files. No `any` types.
- Functional components and hooks only. No class components.
- Named exports only — no default exports except route-level pages.
- Check `src/utils/` before writing a new date, currency, or number formatter.
- All API errors must go through the existing error handler — never swallow errors.

## Commits

Use **Conventional Commits**. Required shape:

```
type(scope): short summary in imperative mood
```

**Allowed types:**

| Type       | When to use                                      |
|------------|--------------------------------------------------|
| `feat`     | New feature or user-visible behavior             |
| `fix`      | Bug fix                                          |
| `refactor` | Code change with no functional impact            |
| `perf`     | Performance improvement                          |
| `style`    | Formatting only (no logic change)                |
| `test`     | Adding or updating tests                         |
| `docs`     | Documentation only                               |
| `chore`    | Tooling, config, deps, CI — not shipped to users |
| `ci`       | CI/CD pipeline changes                           |
| `deps`     | Dependency updates                               |

**Allowed scopes** (match feature folders): `contacts`, `accounts`, `deals`, `activities`, `pipeline`, `auth`, `api`, `ui`, `store`, `router`, `types`, `utils`, `config`

**Valid examples:**

```
feat(deals): add pipeline stage drag-and-drop
fix(contacts): prevent duplicate email on create
refactor(api): extract error handler into shared interceptor
chore(deps): bump axios to 1.7.2
docs: add AGENTS.md with commit conventions
```

**Invalid — will be rejected:**

```
✨ feat: add stuff          ← no emoji
Feature: add contacts       ← wrong casing / type
fix add null check          ← missing colon
[WIP] feat: work in progress ← no extra prefixes
chore(deps):                ← missing summary
```

**Rules:**
- Summary is lowercase, imperative, no period at the end.
- Max 72 characters on the subject line.
- Add a body (blank line after subject) for any non-obvious reasoning.
- Breaking changes: append `!` to the type (`feat(auth)!:`) and add a `BREAKING CHANGE:` footer.
- One logical change per commit — do not batch unrelated changes.
- Run `npm run typecheck` before committing. Do not commit type errors.

## Rules

- Never hard-delete records — use soft-delete (`deletedAt`) and verify backend support first.
- Check the auth context before rendering any delete, export, or admin action (RBAC).
- Token refresh lives in the Axios interceptor — do not duplicate it.
- Never hardcode URLs, keys, or credentials — use `.env` variables only.
- Never modify generated files.
