
Same production-grade backend, built the TDD way: **Red → Green → Refactor** for every piece. Write the failing test first, write just enough code to pass it, then clean up. Check items off as you go.

---

## 0. TDD Setup (do this before writing any feature code)

- [ ] Install test stack: `jest` (or `mocha` + `chai`), `supertest` for HTTP-level tests
- [ ] Configure test script: `"test": "jest --watchAll"` for dev, `"test:ci": "jest --coverage"` for CI
- [ ] Separate test database/config (`.env.test`) — never touch dev/prod data
- [ ] Add `tests/` structure mirroring `src/`:
  ```
  tests/
    unit/
      utils/
      services/
    integration/
      routes/
    fixtures/
    setup.js
  ```
- [ ] Global test setup/teardown (`beforeAll`/`afterAll`) — connect & disconnect test DB
- [ ] Set a coverage threshold in config (e.g. 80%) so CI fails below it
- [ ] Adopt the Red-Green-Refactor discipline for every checklist item below:
  - [ ] 🔴 Red — write a failing test describing the desired behavior
  - [ ] 🟢 Green — write the minimum code to make it pass
  - [ ] 🔵 Refactor — clean up code (and test) without changing behavior

---

## 1. Project Initialization & Structure

- [ ] 🔴 Write a smoke test: "app module loads without throwing"
- [ ] 🟢 `npm init`, folder structure (`config`, `constants`, `controllers`, `middlewares`, `models`, `routes`, `services`, `utils`, `validators`), minimal `app.js`
- [ ] 🔵 Refactor structure once test passes; add ESLint/Prettier
- [ ] `.gitignore`, `.env` / `.env.sample`, `.env.test`
- [ ] `nodemon` for dev; keep `test` script separate from `dev`/`start`

---

## 2. Configuration & Constants

- [ ] 🔴 Test: "throws/exits if a required env var is missing"
- [ ] 🟢 Write env validation (zod/joi) in `config/`
- [ ] 🔵 Refactor into reusable config loader per environment (dev/test/prod)
- [ ] 🔴 Test: constants file exports expected enums/roles/status codes
- [ ] 🟢 Add `constants.js`

---

## 3. `app.js` vs `server.js`

- [ ] 🔴 Integration test: `supertest(app).get('/health')` expects 200 — **before** `app.js` even exists
- [ ] 🟢 Build minimal `app.js` (Express instance, no `listen()`) to pass it
- [ ] 🔵 Refactor: move `listen()` + DB connection into `server.js`, keep `app.js` pure/exportable for tests
- [ ] Confirm app boots without a real server in test mode (only `app.js` imported)

---

## 4. Database Layer

- [ ] 🔴 Unit test for model validation (e.g. required fields, schema rules) using an in-memory DB (e.g. `mongodb-memory-server`) or test DB
- [ ] 🟢 Write the model/schema to satisfy validation rules
- [ ] 🔵 Refactor schema, add indexes, add timestamps
- [ ] 🔴 Test DB connection module: "throws/retries on failed connection"
- [ ] 🟢 Implement connection + retry logic
- [ ] Migration/seed scripts — write a test that seed script inserts expected row count

---

## 5. Routing

- [ ] 🔴 Integration test: `GET /api/v1/users` returns 200 + expected shape (route not yet defined → fails)
- [ ] 🟢 Add route file + mount on central router to pass
- [ ] 🔵 Refactor: split routes by resource, apply versioned prefix `/api/v1`
- [ ] Repeat red-green-refactor per route/endpoint added

---

## 6. Controllers

- [ ] 🔴 Unit test controller function with mocked `req`/`res`/`next` — assert correct status/response called
- [ ] 🟢 Write controller logic (delegating to service layer) to pass
- [ ] 🔵 Refactor: wrap with `asyncHandler`, standardize response shape
- [ ] Ensure controllers stay thin — logic-heavy tests belong in the service layer, not here

---

## 7. Services / Business Logic Layer

- [ ] 🔴 Unit test each service function in isolation (mock DB/model calls)
- [ ] 🟢 Implement service function to satisfy the test
- [ ] 🔵 Refactor for reuse/readability, keep framework-agnostic (no `req`/`res` here)
- [ ] Cover edge cases with additional red-green cycles: empty input, not-found, duplicate entries, etc.

---

## 8. Middlewares

For each middleware, write the test first, describing expected behavior on `req`/`res`/`next`:

- [ ] 🔴 Auth middleware: "rejects request with no/invalid token" → 🟢 implement → 🔵 refactor
- [ ] 🔴 Auth middleware: "attaches user to `req` on valid token" → 🟢 implement
- [ ] 🔴 Role-based middleware: "blocks unauthorized role" → 🟢 implement
- [ ] 🔴 Validation middleware: "returns 400 on invalid payload" → 🟢 implement (Joi/Zod/express-validator)
- [ ] 🔴 Rate limiter: "blocks after N requests" → 🟢 implement (`express-rate-limit`)
- [ ] 🔴 Error-handling middleware: "formats thrown ApiError correctly" → 🟢 implement
- [ ] 🔴 404 handler: "unknown route returns 404 with standard shape" → 🟢 implement
- [ ] CORS, Helmet, compression, body-parser limits — smoke-test each is applied (e.g. check response headers)

---

## 9. Utility Files

- [ ] 🔴 Unit test `ApiError` — correct `statusCode`/`message`/`isOperational` fields
- [ ] 🟢 Implement `ApiError.js`
- [ ] 🔴 Unit test `ApiResponse` — correct shape
- [ ] 🟢 Implement `ApiResponse.js`
- [ ] 🔴 Unit test `asyncHandler` — catches rejected promise and forwards to `next`
- [ ] 🟢 Implement `asyncHandler.js`
- [ ] 🔴 Unit test `logger` — logs at correct level (mock transport)
- [ ] 🟢 Implement `logger.js` (Winston/Pino)
- [ ] 🔴 Unit test any helper (pagination, date formatting, etc.) with edge cases first
- [ ] 🟢 Implement helper

---

## 10. Validation

- [ ] 🔴 Test each schema: valid payload passes, invalid payload (missing/extra/wrong-type fields) fails with correct error message
- [ ] 🟢 Write Joi/Zod schema
- [ ] 🔵 Refactor shared schema fragments (e.g. email, password rules) into reusable pieces
- [ ] Sanitization: test that malicious input (script tags, `$where`, etc.) is stripped/rejected

---

## 11. Authentication & Authorization

- [ ] 🔴 Test: password is hashed before save, never stored plain
- [ ] 🟢 Implement bcrypt/argon2 hashing hook
- [ ] 🔴 Test: login with correct credentials returns valid access + refresh token
- [ ] 🔴 Test: login with wrong credentials returns 401
- [ ] 🟢 Implement login flow
- [ ] 🔴 Test: expired/invalid token is rejected by protected route
- [ ] 🔴 Test: refresh token flow issues new access token
- [ ] 🟢 Implement refresh + expiry logic
- [ ] 🔴 Test: logout invalidates token/session
- [ ] 🟢 Implement logout/invalidation

---

## 12. Error Handling

- [ ] 🔴 Test: thrown `ApiError` in a controller results in correct JSON error response
- [ ] 🔴 Test: unhandled/unexpected error doesn't leak stack trace in production mode
- [ ] 🟢 Implement centralized error handler covering both cases
- [ ] 🔵 Refactor to distinguish operational vs programming errors

---

## 13. Logging & Monitoring

- [ ] 🔴 Test: request logger middleware logs method/path/status/duration (spy on logger)
- [ ] 🟢 Implement request logging middleware
- [ ] 🔴 Test: `/health` endpoint returns 200 with uptime/status info
- [ ] 🟢 Implement health check route
- [ ] Manually verify integration with external monitoring (Sentry/Datadog) — not unit-testable, verify via staging

---

## 14. Security

- [ ] 🔴 Test: security headers present in response (Helmet) — assert via supertest headers
- [ ] 🔴 Test: rate limiter blocks brute-force attempts on `/login`
- [ ] 🔴 Test: injection payloads (NoSQL/SQL/XSS strings) are neutralized, not executed
- [ ] 🟢 Implement/verify protections for each
- [ ] `npm audit` / Snyk scan as part of CI (not unit test, but automated gate)

---

## 15. Performance

- [ ] 🔴 Test: cached endpoint returns cached value on second call without hitting DB (mock/spy on DB call count)
- [ ] 🟢 Implement Redis caching layer
- [ ] 🔴 Test: list endpoint respects `page`/`limit` query params correctly
- [ ] 🟢 Implement pagination
- [ ] Load testing (k6/Artillery) as a separate non-unit-test gate before major releases

---

## 16. Integration & End-to-End Testing

- [ ] Full request/response cycle tests per route (Supertest) — happy path + error paths
- [ ] Test auth-protected routes both with and without valid tokens
- [ ] Test DB is isolated and reset between test runs (`beforeEach`/`afterEach` cleanup)
- [ ] CI runs full suite (unit + integration) on every push/PR
- [ ] Enforce coverage threshold in CI; fail build if below target

---

## 17. Documentation

- [ ] API documentation (Swagger/OpenAPI) generated/updated alongside each new tested route
- [ ] README documents how to run tests (`npm test`, `npm run test:ci`), env vars, and TDD workflow expectations for contributors
- [ ] Architecture notes on test strategy (what's unit vs integration, mocking conventions)

---

## 18. Deployment & DevOps

- [ ] CI pipeline: lint → unit tests → integration tests → coverage check → build → deploy
- [ ] Dockerfile + `docker-compose` (include test DB service for CI)
- [ ] Tests run inside CI container before any deploy step — no deploy on red pipeline
- [ ] Graceful shutdown logic tested (e.g. server closes DB connections cleanly on SIGTERM)
- [ ] Rollback plan defined in case a bad deploy slips through

---

## 19. Final Pre-Launch Review

- [ ] Full test suite green, coverage threshold met
- [ ] No skipped/pending tests left unresolved (`it.skip`, `xit`, `TODO`)
- [ ] Security scan clean
- [ ] Load test results acceptable for expected traffic
- [ ] Monitoring/alerting verified live in staging
- [ ] Backup and rollback plans confirmed

---

*TDD discipline: never write implementation code without a failing test driving it. If you catch yourself writing code first, stop, delete it, write the test, watch it fail, then rebuild.*