# HarnessHub Manager — CRM CRUD System

## Project Overview

HarnessHub Manager is a CRM (Customer Relationship Management) system focused on managing contacts, leads, accounts, deals, and activities. It provides a full CRUD interface for all core entities, enabling sales and support teams to track customer relationships throughout the entire lifecycle.

## Tech Stack

- **Frontend**: React.js (version defined in `.nvmrc`)
- **Styling**: TailwindCSS + component library (check `package.json`)
- **State Management**: Check the project — prefer Context API or Zustand over Redux unless Redux is already in use
- **HTTP Client**: Axios (check for existing interceptors and base configuration)
- **Routing**: React Router v6
- **Forms**: React Hook Form + Zod (validation)
- **Backend**: REST API (check `.env` for base URL)
- **Auth**: JWT-based (Bearer token in Authorization header)

## Core CRM Entities

| Entity       | Description                                              |
|--------------|----------------------------------------------------------|
| `Contact`    | Individual people (leads, customers, partners)           |
| `Account`    | Companies or organizations linked to contacts            |
| `Deal`       | Sales opportunities with stage, value, and close date    |
| `Activity`   | Calls, emails, meetings, tasks linked to any entity      |
| `User`       | System users (sales reps, managers, admins)              |
| `Pipeline`   | Sales funnel stages for deals                            |
| `Tag`        | Labels applied to contacts, accounts, or deals           |

## CRUD Conventions

Every entity follows this standard CRUD pattern:

- **List page** — paginated table with search, filters, and bulk actions
- **Detail page** — read-only view of a single record with related data panels
- **Create/Edit form** — modal or dedicated page with validation
- **Delete** — confirmation dialog before any destructive action

### API Pattern

```
GET    /api/{entity}           → list (paginated, filterable)
GET    /api/{entity}/:id       → single record
POST   /api/{entity}           → create
PUT    /api/{entity}/:id       → full update
PATCH  /api/{entity}/:id       → partial update
DELETE /api/{entity}/:id       → soft delete (check `deletedAt` field)
```

### Pagination & Filtering

All list endpoints accept:
- `page`, `limit` — pagination
- `search` — full-text search
- `sortBy`, `sortOrder` — sorting
- Entity-specific filters as query params

## Folder Structure (expected)

```
src/
├── api/            # Axios instances, service files per entity
├── components/     # Shared UI components
│   ├── common/     # Buttons, inputs, modals, tables, badges
│   └── crm/        # CRM-specific components (DealCard, ContactAvatar, etc.)
├── features/       # Feature modules per CRM entity
│   ├── contacts/
│   ├── accounts/
│   ├── deals/
│   ├── activities/
│   └── pipeline/
├── hooks/          # Custom React hooks
├── layouts/        # App shell, sidebar, topbar
├── pages/          # Route-level page components
├── store/          # Global state (Zustand or Context)
├── types/          # TypeScript interfaces and enums
└── utils/          # Helpers, formatters, constants
```

## Key Behaviors & Rules

- Always check the existing folder structure before creating new files — match the established pattern.
- All dates must be formatted consistently. Check `utils/` for existing date formatters before writing new ones.
- Currency and number formatting: check `utils/` for existing formatters.
- API errors must be handled via the existing error handler — do not swallow errors silently.
- Soft-delete pattern: never perform hard deletes on the frontend without verifying backend support.
- Role-based UI: some actions (delete, export, admin settings) are hidden based on the user's role. Check the auth context before rendering sensitive controls.

## Environment Variables

Follow the `.env.example` file at the project root. Never hardcode URLs, keys, or credentials.

## Authentication

- Token stored in `localStorage` or `httpOnly` cookie — verify the current implementation before touching auth flow.
- Protected routes use a route guard component — check `src/layouts/` or `src/routes/`.
- Token refresh logic lives in the Axios interceptor — do not duplicate it.

## Testing

- Unit tests: Vitest or Jest (check `package.json`)
- Component tests: React Testing Library
- Run tests with `npm test` before submitting any change
- Do not bypass or delete existing tests

## Coding Standards

- TypeScript is required for all new files — no plain `.js` except config files
- No `any` types — use proper interfaces defined in `src/types/`
- Prefer functional components and hooks — no class components
- Keep components small and single-responsibility
- Co-locate component styles, tests, and stories when applicable
