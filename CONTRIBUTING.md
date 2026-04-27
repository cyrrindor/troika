# Contributing Guide

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend / API | Next.js 14+ (App Router) |
| Language | TypeScript (strict) |
| Styling | Tailwind CSS |
| Database | PostgreSQL via Supabase |
| ORM | Prisma |
| Auth | NextAuth.js |
| Hosting | Vercel + Supabase |
| CI/CD | GitHub Actions |

## Branch Naming

```
feat/short-description
fix/short-description
chore/short-description
docs/short-description
test/short-description
```

## Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add user profile page
fix: resolve login redirect loop
chore: upgrade Next.js to 14.2
docs: update API auth section
test: add coverage for checkout flow
```

## Pull Requests

- Target `main` branch
- Fill out the PR template fully
- At least 1 approval required before merge
- All CI checks must pass
- Prefer **squash merge** to keep history clean

## TypeScript Rules

- `strict: true` is enforced — no `any` types
- Use `unknown` + type guards instead of `any`
- Use explicit return types on exported functions
- Prefer interfaces for object shapes, type aliases for unions

## Code Style

Enforced by ESLint + Prettier (run automatically in CI):

```bash
npm run lint        # ESLint check
npm run format      # Prettier format
npx tsc --noEmit    # Type check
```

## Environment Variables

Never commit `.env` files. Add new vars to:
1. `.env.example` (with placeholder values)
2. Vercel project settings (production)
3. GitHub Actions secrets (for CI)

## Database Changes

Always use Prisma migrations:

```bash
npx prisma migrate dev --name describe-change  # local dev
npx prisma migrate deploy                       # production (CI handles this)
```

Never edit the database schema directly in production.

## Testing

- Unit tests: Vitest
- Integration tests: Vitest + test database
- Run: `npm test`

Write tests for business logic and non-trivial utilities. UI component tests are optional but encouraged for complex interactions.

## Engineering Processes

For sprint cadence, detailed PR review SLAs, deployment workflow, and code ownership rules, see:

- [docs/engineering-processes.md](./docs/engineering-processes.md)

For incident severity levels, response steps, and postmortem process, see:

- [docs/incident-response.md](./docs/incident-response.md)
