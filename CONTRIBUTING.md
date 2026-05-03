# Contributing to AI CRM System

Thank you for your interest in contributing! 🎉

## Getting Started

1. Fork the repository
2. Clone your fork: `git clone https://github.com/YOUR_USERNAME/Crm-final.git`
3. Install dependencies: `npm install`
4. Copy env: `cp .env.example .env` and fill in keys
5. Push DB schema: `npm run db:push`
6. Start dev: `npm run dev`

## Development Workflow

```bash
# Create a feature branch
git checkout -b feature/your-feature-name

# Make your changes
# ...

# Commit with a descriptive message
git commit -m "feat: add amazing new feature"

# Push to your fork
git push origin feature/your-feature-name

# Open a Pull Request
```

## Commit Message Convention

We follow [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` — New feature
- `fix:` — Bug fix
- `docs:` — Documentation changes
- `refactor:` — Code refactoring
- `test:` — Adding tests
- `chore:` — Build process or tooling changes

## Code Standards

- **TypeScript** — All new code must be typed
- **No `any`** — Use proper types or `unknown`
- **Prettier** — Format with `npx prettier --write .`
- **ESLint** — Lint with `npx eslint src/`
- **Server/Client boundary** — Never import `prisma` in `'use client'` components

## Adding a New Feature

Before adding:
1. Check if an issue exists or create one
2. Read `docs/DEVELOPER_GUIDE.md` — especially the "Adding New Features" section
3. Read `PROJECT_CONTEXT.md` for architecture rules

## Pull Request Checklist

- [ ] Code follows the project conventions
- [ ] TypeScript types are properly defined
- [ ] No `console.log` left in production code
- [ ] README updated if new features added
- [ ] `.env.example` updated if new env vars added
- [ ] `prisma/schema.prisma` updated and `npm run db:push` tested if DB changes made

## Questions?

Open an issue or start a discussion — we're happy to help!