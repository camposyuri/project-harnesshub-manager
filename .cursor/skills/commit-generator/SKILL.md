---
name: commit-generator
description: >
  Generates Conventional Commit messages for the HarnessHub Manager project by analyzing staged git changes.
  Use when the user asks to commit, write a commit message, or says "gera commit", "cria commit", "faz commit".
  Reads git diff --staged, picks the correct type and scope, and produces a ready-to-run git commit command.
disable-model-invocation: false
---

# Commit Generator — HarnessHub Manager

## Steps

1. Run `git diff --staged` to read all staged changes.
2. If nothing is staged, run `git status` and inform the user which files are unstaged, then stop.
3. Determine **type** and **scope** from the tables below.
4. Write a subject line: `type(scope): imperative summary` (max 72 chars, lowercase, no period).
5. If the change is non-obvious, add a body (blank line after subject) explaining *why*, not *what*.
6. Output the final `git commit -m` command for the user to review and run — do not run it automatically.

## Type selection

| Type       | Choose when the diff…                                      |
|------------|------------------------------------------------------------|
| `feat`     | adds a new user-visible feature or API endpoint            |
| `fix`      | corrects a bug or incorrect behavior                       |
| `refactor` | restructures code without changing behavior                |
| `perf`     | improves speed or memory without changing behavior         |
| `style`    | changes formatting, whitespace, or naming only             |
| `test`     | adds or updates tests                                      |
| `docs`     | updates documentation, comments, or README                 |
| `chore`    | tooling, config, build scripts, CI — not shipped to users  |
| `ci`       | changes to CI/CD pipeline files                            |
| `deps`     | bumps or pins a dependency version                         |

When multiple types apply, pick the one with the highest user impact (`feat` > `fix` > `refactor` > rest).

## Scope selection

Map the changed files to the nearest scope:

| Files changed in…                        | Scope        |
|------------------------------------------|--------------|
| `src/features/contacts/`                 | `contacts`   |
| `src/features/accounts/`                 | `accounts`   |
| `src/features/deals/`                    | `deals`      |
| `src/features/activities/`               | `activities` |
| `src/features/pipeline/`                 | `pipeline`   |
| `src/api/` or Axios config               | `api`        |
| `src/store/`                             | `store`      |
| `src/components/`                        | `ui`         |
| `src/hooks/`                             | `ui`         |
| `src/pages/` or route files              | `router`     |
| `src/types/`                             | `types`      |
| `src/utils/`                             | `utils`      |
| Auth, token, login/logout flow           | `auth`       |
| Config, env, build, tooling              | `config`     |
| Changes spanning 3+ unrelated scopes     | *(omit scope)* |

## Output format

```
type(scope): short summary

Optional body explaining WHY (not what).
```

Ready-to-run command:

```bash
git commit -m "type(scope): short summary"
# or with body:
git commit -m "$(cat <<'EOF'
type(scope): short summary

Body explaining why.
EOF
)"
```

## Examples

**feat — new filter on contacts list:**
```bash
git commit -m "feat(contacts): add filter by tag on list view"
```

**fix — null crash on deal detail:**
```bash
git commit -m "fix(deals): prevent crash when closeDate is null"
```

**refactor with body:**
```bash
git commit -m "$(cat <<'EOF'
refactor(api): extract error handler into shared interceptor

Each feature had its own try/catch; consolidating removes ~80 lines
and ensures consistent error reporting across all services.
EOF
)"
```

**chore — bump dependency:**
```bash
git commit -m "chore(deps): bump axios from 1.6.8 to 1.7.2"
```

## Rules

- Summary is **imperative mood** ("add", "fix", "remove" — not "added", "fixes", "removing").
- No emoji, no `[WIP]`, no `[JULES]`, no extra prefixes.
- Breaking change: append `!` to type/scope (`feat(auth)!:`) and add a `BREAKING CHANGE:` footer in the body.
- One logical change per commit — if the diff mixes unrelated changes, tell the user and suggest splitting with `git add -p`.
- Never run `git commit` automatically — always present the command for the user to review first.
