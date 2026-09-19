

A production-grade checklist to follow when building a Node.js backend, from project setup to deployment. Check off items as you go.

---
## 1. Project Initialization & Structure

- [ ] `npm init` / set up `package.json` with correct name, version, description, license
- [ ] Initialize git repo, add `.gitignore` (node_modules, .env, dist, logs)
- [ ] Set up folder structure:
  ```
  src/
    config/
    constants/
    controllers/
    middlewares/
    models/
    routes/
    services/
    utils/
    validators/
    app.js
    server.js (entry point, separate from app.js)
  ```
- [ ] Set up `.env` and `.env.sample` (never commit real `.env`)
- [ ] Configure `nodemon` for dev, proper `npm scripts` (`dev`, `start`, `test`, `lint`)
- [ ] Set up ESLint + Prettier for consistent code style
- [ ] Choose module system (CommonJS vs ESM) and stick to it consistently

---

## 2. Configuration & Constants

- [ ] `config/` folder — centralize DB config, cloud config, env-based config
- [ ] `constants.js` — app name, enums, roles, status codes, default values
- [ ] Load and validate env variables at startup (fail fast if missing, e.g. via `zod`/`joi`)
- [ ] Separate configs for `development`, `test`, `production`

---

## 3. `app.js` vs `server.js`

- [ ] `app.js` — only Express app setup (middlewares, routes, error handlers) — exported, no `listen()`
- [ ] `server.js` — imports `app`, connects DB, then calls `app.listen()`
- [ ] Keeps app testable without spinning up a real server

---

## 4. Database

- [ ] DB connection file in `config/` or `db/` with retry logic
- [ ] Use connection pooling
- [ ] Models/schemas in `models/` (Mongoose/Prisma/Sequelize/etc.)
- [ ] Add indexes on frequently queried fields
- [ ] Migrations set up (if using SQL / Prisma)
- [ ] Seed scripts for local/dev data

---

## 5. Routing

- [ ] Routes split by resource/module in `routes/` (e.g. `user.routes.js`, `auth.routes.js`)
- [ ] Central router (`index.js`) that mounts all sub-routers
- [ ] Versioned API prefix (`/api/v1/...`)
- [ ] RESTful naming conventions followed consistently

---

## 6. Controllers

- [ ] Controllers only handle req/res logic — no business logic inside
- [ ] Wrap async controllers with an `asyncHandler` utility to catch errors
- [ ] Consistent response format (success/error shape) across all endpoints

---

## 7. Services / Business Logic Layer

- [ ] Business logic separated into `services/` — controllers call services, not DB directly
- [ ] Keep services reusable and testable independent of Express

---

## 8. Middlewares

- [ ] Global error-handling middleware (last in the stack)
- [ ] 404 / not-found handler
- [ ] Auth middleware (JWT/session verification)
- [ ] Role-based access control middleware
- [ ] Request validation middleware (Joi/Zod/express-validator)
- [ ] Logging middleware (morgan or custom)
- [ ] CORS middleware configured properly (not `*` in production)
- [ ] Rate limiting middleware (`express-rate-limit`)
- [ ] Body parsers (`express.json()`, `express.urlencoded()`) with size limits
- [ ] Helmet for security headers
- [ ] Compression middleware (`compression`)
- [ ] Request ID / correlation ID middleware for tracing

---

## 9. Utility Files

- [ ] `ApiError.js` — standard custom error class
- [ ] `ApiResponse.js` — standard success response class
- [ ] `asyncHandler.js` — wraps controllers to catch async errors
- [ ] `logger.js` — centralized logger (Winston/Pino)
- [ ] Helper functions (date formatting, pagination, string utils, etc.)

---

## 10. Validation

- [ ] Input validation on every route that accepts data (body/query/params)
- [ ] Sanitize inputs to prevent injection attacks
- [ ] Validate file uploads (type, size limits)

---

## 11. Authentication & Authorization

- [ ] Password hashing (bcrypt/argon2)
- [ ] JWT access + refresh token strategy
- [ ] Secure cookie flags (`httpOnly`, `secure`, `sameSite`) if using cookies
- [ ] Token expiry & refresh flow
- [ ] Role/permission-based access control
- [ ] Logout / token invalidation strategy

---

## 12. Error Handling

- [ ] Centralized error handler middleware catches all errors
- [ ] Differentiate operational errors vs programming errors
- [ ] Never leak stack traces in production responses
- [ ] Consistent error response shape (`statusCode`, `message`, `errors[]`)

---

## 13. Logging & Monitoring

- [ ] Structured logging (Winston/Pino) with log levels (info, warn, error, debug)
- [ ] Log rotation / external log shipping (e.g. to a log service)
- [ ] Request/response logging (method, path, status, duration)
- [ ] Health check endpoint (`/health` or `/status`)
- [ ] Uptime/monitoring integration (e.g. Sentry, Datadog, New Relic)

---

## 14. Security

- [ ] Helmet configured
- [ ] Rate limiting on sensitive routes (login, signup, OTP)
- [ ] Sanitize against NoSQL/SQL injection
- [ ] Prevent XSS (sanitize output, CSP headers)
- [ ] CSRF protection where relevant
- [ ] Secrets never hardcoded — only via env vars
- [ ] Dependency vulnerability scan (`npm audit`, Snyk)
- [ ] Enforce HTTPS in production

---

## 15. Performance

- [ ] Caching layer (Redis) for expensive/frequent queries
- [ ] Pagination on list endpoints
- [ ] Avoid N+1 queries
- [ ] Compression enabled
- [ ] Load testing done (k6, Artillery) before major release

---

## 16. Testing

- [ ] Unit tests for services/utils (Jest/Mocha)
- [ ] Integration tests for API routes (Supertest)
- [ ] Test DB separate from dev/prod DB
- [ ] CI runs tests automatically on push/PR
- [ ] Reasonable code coverage target set

---

## 17. Documentation

- [ ] API documentation (Swagger/OpenAPI or Postman collection)
- [ ] README with setup instructions, env vars, scripts
- [ ] Architecture/decision notes for future maintainers

---

## 18. Deployment & DevOps

- [ ] Dockerfile + `.dockerignore`
- [ ] `docker-compose` for local dev (app + DB + Redis etc.)
- [ ] CI/CD pipeline (GitHub Actions/GitLab CI)
- [ ] Environment-specific configs for staging/production
- [ ] Graceful shutdown handling (close DB connections, finish in-flight requests)
- [ ] Process manager (PM2) or containerized orchestration (Docker/K8s)
- [ ] Zero-downtime deployment strategy

---

## 19. Final Pre-Launch Review

- [ ] All env vars documented and set correctly per environment
- [ ] Error monitoring live and alerting configured
- [ ] Backups configured for the database
- [ ] Rollback plan in place
- [ ] Load tested for expected traffic
- [ ] Security review / audit done

---

*Tip: duplicate this file per project and check items off as you build — it doubles as a running audit trail of what's covered and what's still pending.*