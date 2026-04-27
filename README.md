# Project

## Stack

| Layer | Technology | Why |
|-------|-----------|-----|
| Frontend / API | Next.js 14+ (App Router) | Full-stack React, SSR, great DX |
| Language | TypeScript (strict) | Type safety, maintainability |
| Styling | Tailwind CSS | Utility-first, consistent |
| Database | PostgreSQL via Supabase | Managed, free tier, real-time |
| ORM | Prisma | Type-safe DB access, migrations |
| Auth | NextAuth.js | OAuth providers, JWT sessions |
| Hosting | Vercel + Supabase | Zero-config deploys, scalable |
| CI/CD | GitHub Actions | Native GitHub integration |

## Getting Started

```bash
# Install dependencies
npm install

# Set up environment
cp .env.example .env
# Fill in values in .env

# Run DB migrations
npx prisma migrate dev

# Start development server
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

## Key Commands

```bash
npm run dev      # Start dev server
npm run build    # Production build
npm run lint     # ESLint check
npm run format   # Prettier format
npm test         # Run tests
npx tsc --noEmit # Type check
```

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for branching strategy, commit conventions, and code standards.

## Environment Variables

Copy `.env.example` to `.env` and fill in the values. Never commit `.env`.
