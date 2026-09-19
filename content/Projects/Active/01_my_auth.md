
### 📋Phase 1: Domain & Data Modeling 🗄️

_The foundation layer: Database setup, schema design, and type definitions._

- [x] **Database & ORM Setup**
    
    - [x] Initialize PostgreSQL database connection.
        
    - [x] Install and configure TypeScript ORM (e.g., Drizzle ORM or Prisma).
        
    - [x] Configure environment variables (`DATABASE_URL`, validation schema).
        
    - [x] Set up database migration pipeline (scripts for local & production runs).
        
- [x] **Core Entity Schemas**
    
    - [x] **`users` table:** `id` (UUID/CUID), `email` (unique), `password_hash`, `email_verified` (boolean), `created_at`, `updated_at`.
        
    - [x] **`sessions` table:** `id`, `user_id` (FK to users), `token` (hashed or raw), `expires_at`, `ip_address`, `user_agent`, `created_at`.
        
    - [x] **`accounts` table (for OAuth):** `id`, `user_id` (FK), `provider_id` (e.g., "google"), `provider_account_id`, `access_token`, `refresh_token`, `expires_at`.
        
    - [x] **`verifications` table:** `id`, `identifier` (email), `token` (hashed), `expires_at`, `type` ("email_verify" | "password_reset").
        
- [ ] **RBAC & Advanced Schemas (Optional/Extensions)**
    
    - [ ] **`roles` & `permissions` tables:** Define user roles (e.g., "admin", "user") and permissions.
        
    - [ ] Database foreign key constraints, indexes on frequently queried fields (`email`, `token`, `user_id`).
        

### 🔒 Phase 2: Core Auth Engine Logic

_The business logic layer: Standalone TypeScript modules for hashing, token generation, and state management._

- [x] **Cryptographic Helpers**
    
    - [x] Implement password hashing and verification using **Argon2id** (or `scrypt`).
        
    - [x] Create secure random string/token generators using `crypto.getRandomValues`.
        
    - [x] Build token hashing helper (for storing verification tokens securely).
        
- [x] **Session Engine**
    
    - [x] Create session method: Generate token, hash it, store session record in DB, return unhashed token to client.
        
    - [x] Validate session method: Lookup session by token, check expiration, return session + user payload.
        
    - [x] Revoke session method: Delete specific session by ID.
        
    - [x] Revoke all sessions method: Delete all active sessions for a specific `user_id` (e.g., on password reset).
        
- [ ] **User Lifecycle Logic**
    
    - [x] Sign-up pipeline: Check duplicate email, hash password, insert user, create verification record.
        
    - [ ] Sign-in pipeline: Lookup user, verify password hash, create new session.
        
    - [ ] Email verification pipeline: Validate verification token, update `email_verified` status, purge used token.
        

### 🛡️ Phase 3: Security & Middleware Layer

_The protection layer: Request validation, rate limiting, and cookie handling._

- [ ] **Input Validation & Sanitization**
    
    - [ ] Define **Zod** schemas for incoming JSON bodies (`SignUpSchema`, `SignInSchema`, `ResetPasswordSchema`).
        
    - [ ] Build middleware to parse and reject invalid request payloads before hitting route handlers.
        
- [ ] **Cookie & Header Management**
    
    - [ ] Implement secure cookie parser and serializer (`HttpOnly`, `Secure`, `SameSite=Lax`, `Path=/`).
        
    - [ ] Implement CSRF (Cross-Site Request Forgery) protection via custom HTTP header validation or double-submit cookies.
        
    - [ ] Set up CORS (Cross-Origin Resource Sharing) middleware with dynamic origin verification.
        
- [ ] **Rate Limiting & Protection**
    
    - [ ] Setup **Redis** connection for memory caching and sliding-window rate limiting.
        
    - [ ] Attach strict rate limiters to authentication endpoints (e.g., max 5 login attempts per minute per IP).
        
    - [ ] Add basic request IP filtering / proxy header handling (`X-Forwarded-For`).
        

### 🔌 Phase 4: Public API, Plugins & Client SDK

_The delivery layer: HTTP routes, OAuth, and developer experience._

- [ ] **REST HTTP Route Handlers**
    
    - [ ] `POST /api/auth/signup`
        
    - [ ] `POST /api/auth/login`
        
    - [ ] `POST /api/auth/logout`
        
    - [ ] `GET /api/auth/me` (Protected current user endpoint)
        
    - [ ] `POST /api/auth/verify-email`
        
- [ ] **Plugin System Architecture**
    
    - [ ] Build plugin hook system (e.g., `onRequest`, `onSessionCreate`, `onUserCreate`).
        
    - [ ] **OAuth 2.0 / OIDC Plugin:** Google & GitHub login handler implementations.
        
    - [ ] **RBAC Plugin:** Authorization middleware checking user roles/permissions on protected endpoints.
        
- [ ] **Email Integration**
    
    - [ ] Integrate email delivery service (e.g., Resend, SendGrid, or Nodemailer).
        
    - [ ] Create templates for account verification and password resets.
        
- [ ] **Client SDK / Utility**
    
    - [ ] Build type-safe client wrapper for frontend applications (`authClient.signIn()`, `authClient.useSession()`).
        

