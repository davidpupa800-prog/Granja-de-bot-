---
name: advanced-software-engineering
description: Professional software engineering workflow for this user's stack - Node/Express, Python, static web/PWA, Android/Gradle, Supabase (Postgres, SQL, RLS, storage), Railway and Docker deploys, Git/GitHub. Use for any feature, bug fix, API, database/migration, deploy, refactor, test, performance or defensive-security task.
---

# Advanced Software Engineering

## Workflow (stages, verify each)
1. **Understand:** read README/CLAUDE.md, recall memory (`advanced-memory`), locate code with grep. Reproduce bugs before fixing.
2. **Plan:** smallest change that solves it; reuse existing modules; note risks (data loss, auth, deploy).
3. **Implement:** follow the repo's existing style, module system (ESM vs CJS), and structure.
4. **Verify:** run the narrowest test, then `npm test` / `pytest` / `./gradlew test`; run the app when behavior is user-visible (`run` skill). Add/adjust tests for every fix.
5. **Review:** `/code-review` for correctness, `/security-review` for auth/input/secrets changes, `/simplify` for cleanup.
6. **Ship:** commit on the designated branch with a clear message; push; PR only when asked.
7. **Record:** save non-obvious decisions, fixes, architecture to memory.

## Backend & APIs
- Validate all input at the boundary; consistent JSON error shape; correct status codes.
- Express: `helmet`, body size limits, `multer` limits/filters for uploads, no stack traces in prod responses, health endpoint for Railway.
- Config from env vars only; fail fast on missing required vars; never commit `.env`.

## Supabase / SQL
- Schema changes as versioned files in `supabase/migrations/` (timestamped, idempotent where possible: `if not exists`).
- Enable RLS on every exposed table; write explicit policies; `service_role` key only server-side.
- Index foreign keys and filter columns; parameterized queries; `security definer` functions with fixed `search_path`.
- Storage: bucket policies, size/MIME limits.

## Deploy (Railway / Docker)
- `railway.json`: start command, healthcheck path that doesn't depend on optional services.
- Dockerfiles: slim base, pinned versions, `--no-cache-dir`, non-root user, copy dependency manifests before source for layer caching.
- Bind to `0.0.0.0:$PORT`. Secrets via Railway variables.

## Frontend / PWA
- Mobile-first, accessible (labels, contrast, focus), lazy-load heavy media, version the service-worker cache.

## Android / Gradle
- Keep SDK/AGP versions consistent; no keystores or signing passwords in the repo.

## Defensive security checklist
Secrets out of code and logs · authz on every route · input validation & output encoding (XSS) · parameterized SQL · CORS restricted · rate limiting on auth/upload · dependency audit (`npm audit`, `pip-audit`) · least-privilege keys.

## Git
Small focused commits; never force-push shared branches; never commit secrets, build outputs or large media without need.
