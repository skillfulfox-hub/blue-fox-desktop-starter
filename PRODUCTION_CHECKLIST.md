# Production Checklist (Starter Kit Consumer)

Use this checklist before deploying an application built on top of this starter kit.

## 1. Security

- [ ] Replace JWT secret with a strong environment variable (never commit secrets).
- [ ] Rotate all demo/default credentials (`admin/admin123`) and enforce password policy.
- [ ] Review demo guardrails: bootstrap user `admin` is protected from deletion in demo mode; remove or redesign this rule for real production workflows.
- [ ] Disable Swagger in production (`SWAGGER_ENABLED=false`) unless explicitly required.
- [ ] Tighten CORS to known frontend origins only.
- [ ] Enable secure cookies (`secure: true`) behind HTTPS.
- [ ] Add request throttling for auth endpoints (`/api/auth/login`, `/api/auth/register`).
- [ ] Add `helmet` and review security headers.

## 2. Database

- [ ] Disable TypeORM auto-sync in production (`synchronize: false`).
- [ ] Introduce and run database migrations.
- [ ] Ensure unique constraints for identity fields (username/email).
- [ ] Configure backups and restore procedure.

## 3. Configuration

- [ ] Move all environment-specific values to `.env` / secret manager.
- [ ] Validate required env vars at startup (fail fast).
- [ ] Separate dev/stage/prod config profiles.

## 4. Auth and Sessions

- [ ] Review JWT expiration and refresh strategy.
- [ ] Keep JWT storage strategy consistent (desktop: bearer token, web: cookie fallback by default in this kit).
- [ ] Add account lockout or delay strategy for repeated failed login attempts.

## 5. Observability and Operations

- [ ] Add structured logging (with request correlation id).
- [ ] Remove sensitive data from logs.
- [ ] Add health checks and uptime monitoring.
- [ ] Add error tracking and alerting.

## 6. Quality Gates

- [ ] Ensure server unit tests and client tests pass in CI.
- [ ] Ensure client e2e smoke tests pass across target browsers.
- [ ] Add CI pipeline with lint + test + build + artifact validation.
