# Project: Corrector
Portal application for the Direct Data Control Project - an ESG correction workflow system for submitting, reviewing, and managing data corrections.

## Tech Stack
- **Framework**: Next.js 15.3.8 (App Router), React 19.1.0
- **Language**: TypeScript 5.4.5
- **Styling**: Tailwind CSS 4.1
- **Components**: @conservice/components 14.10, @heroicons/react 2.0.18
- **Forms & Validation**: React Hook Form 7.54.2, Zod 3.21.4
- **Data Tables**: @tanstack/react-table 8.9.2
- **Authentication**: NextAuth 5.0.0-beta.16 (Keycloak)
- **Testing**: Vitest 3.2.4, React Testing Library 16.3.0, MSW 2.4.9
- **Build Tool**: Vite (via Next.js)
- **Linting & Formatting**: ESLint 9.29.0, Prettier 3.5.3

## Commands
```bash
npm run dev              # Start development server (port 3045)
npm run build            # Build the project
npm test                 # Run all tests with Vitest
npm run test-watch       # Run tests in watch mode
npm run lint             # Lint code with ESLint
npm run prettier-format  # Format code with Prettier
npm run tsc              # Type check without emit
```
Run all commands from the project root unless specified.

## Architecture

### Directory Structure
```
corrector-app/
├── src/
│   ├── app/                   # Next.js App Router (pages, layouts, API routes)
│   │   ├── (corrector)/       # Route group for corrector features
│   │   │   ├── page.tsx       # Landing page
│   │   │   ├── submit-file/   # File submission
│   │   │   ├── submission-review/ # Review submissions
│   │   │   └── review-queue/  # Review queue management
│   │   └── api/               # API routes (auth, correction, employees, etc.)
│   ├── actions/               # Server actions for data fetching/mutations
│   ├── components/            # React components (FileManager, ReviewQueue, etc.)
│   ├── hooks/                 # Custom React hooks (useSubmissionReview, etc.)
│   ├── lib/                   # Utilities (API, CSV processing, validation)
│   ├── schema/                # Zod validation schemas
│   ├── types/                 # TypeScript type definitions
│   ├── platform-libs/         # Platform-owned shared libraries (see below)
│   └── middleware.ts          # Next.js middleware
├── .stratus/                  # Kubernetes deployment configs
├── .qa/                       # Playwright QA automation tests
└── doc/                       # Documentation
```

### Key Patterns
- **Routing:** Next.js App Router with route groups, basePath `/esg/corrector`
- **State Management:** React hooks + custom composed hooks (no Redux/Zustand)
- **Data Fetching:** Server actions (`'use server'`) with Zod validation
- **Forms:** React Hook Form + Zod resolver
- **UI Components:** @conservice/components (v14.10) + Tailwind CSS 4.1
- **Testing:** Vitest + React Testing Library + MSW (100% coverage required)
- **Auth:** NextAuth v5 with Keycloak

## Code Style
- Follow `@conservice/eslint-config-conservice` and `@conservice/prettier-config` standards
- Use TypeScript strict mode - all files must be properly typed
- Import paths use `@/` alias (e.g., `@/components/ui`)
- Server actions must be in separate files with `'use server'` directive
- Zod schemas required for all API response validation
- 100% test coverage enforced - all new code must have tests

## Boundaries
- ✅ Always: Run tests before suggesting commits, follow existing patterns
- ⚠️ Ask first: Adding dependencies, schema changes, architectural decisions  
- 🚫 Never: Commit secrets, edit generated files, modify CI without discussion

## Workflows

### Adding a New Feature
1. Create feature branch from `main`
2. Add/modify files in `src/app/(corrector)/` for UI
3. Add server actions in `src/actions/` for data operations
4. Create Zod schemas in `src/schema/` for validation
5. Write tests achieving 100% coverage
6. Run `npm run lint` and `npm run prettier-format`
7. Run `npm test` to verify all tests pass
8. Commit and create pull request to `main`

### Local Development Setup
1. Install Node.js 22+
2. Set `AUTOMATION_PAT` environment variable (GitHub personal access token with `read:packages`)
3. Run `npm install`
4. Set up Portal 2.0 HostShell (platform-portal must be running)
5. Create `.env.local` from `.env.template`
6. Run `npm run dev` (starts on port 3045)
7. Access app through HostShell at the corrector route

## Notes
- **Platform Ownership:** Files in `src/platform-libs/` and `src/app/api/auth/` are maintained by platform frontend team - requires their review for changes
- **Portal Dependency:** This is a micro-frontend that requires Portal 2.0 HostShell (platform-portal) to be running for local development
- **Port Configuration:** Development runs on port 3045 - must be consistent across package.json, next.config.ts, values.yaml, and Dockerfile
- **Authentication:** Uses NextAuth v5 (beta) with Keycloak - environment variables for auth must NOT be in configmap (see doc/SecretsManagement.md)
- **Certificate Handling:** Special CA bundle configuration required for API calls due to missing intermediate certificates
- **Test Coverage:** 100% coverage enforced by CI - all PRs must maintain coverage
- **No Disabled Tests:** vitest/no-disabled-tests rule is enforced - fix or remove failing tests
- **Deployment:** Automated via GitHub Actions on push to `main` - see README.md for deployment prep checklist
- See @doc/SecretsManagement.md for environment-specific secret management workflow