# MERN Stack Interview Preparation Roadmap

This roadmap covers the deep technical topics required for MERN stack interviews.


## 📊 Progress Tracker (DataviewJS)

```dataviewjs
const content = await dv.io.load(dv.current().file.path);
const lines = content.split("\n");

const modules = [];
let cur = null;

for (const line of lines) {
  if (line.startsWith("## ")) {
    cur = { title: line.slice(3).trim(), total: 0, done: 0 };
    modules.push(cur);
  } else if (cur) {
    if (/^- \[x\]/i.test(line)) { cur.done++; cur.total++; }
    else if (/^- \[ \]/i.test(line)) { cur.total++; }
  }
}

const total = modules.reduce((s, m) => s + m.total, 0);
const done = modules.reduce((s, m) => s + m.done, 0);
const pct = total ? Math.round(done / total * 100) : 0;

dv.header(2, `Overall Progress: ${done} / ${total} Completed (${pct}%)`);
dv.el("progress", "", { attr: { value: done, max: total, style: "width:100%; height:20px" } });

dv.header(3, "Module Breakdown");
for (const m of modules) {
  const mpct = m.total ? Math.round(m.done / m.total * 100) : 0;
  const bar = "█".repeat(Math.round(mpct / 5)) + "░".repeat(20 - Math.round(mpct / 5));
  dv.paragraph(`**${m.title}** — ${m.done}/${m.total} (${mpct}%) \`${bar}\``);
}
```


##  1. Node.js (The Runtime)
- [ ] **Core Architecture**
    - [ ] Event Loop (Timers, Pending Callbacks, Idle/Prepare, Poll, Check, Close)
    - [ ] Thread Pool (libuv) and Worker Threads
    - [ ] Blocking vs Non-Blocking I/O
- [ ] **Asynchronous Patterns**
    - [ ] Callbacks, Promises, and Async/Await
    - [ ] `process.nextTick()` vs `setImmediate()`
- [ ] **Modules & APIs**
    - [ ] CommonJS vs ES Modules (import/export)
    - [ ] Streams (Readable, Writable, Duplex, Transform)
    - [ ] Buffers and File System (fs)
    - [ ] EventEmitter and Custom Events
- [ ] **Performance & Security**
    - [ ] Memory Leaks and Garbage Collection
    - [ ] `process` object and Environment Variables
    - [ ] Security: Handling `node_modules`, npm audit, and vulnerable dependencies

##  2. Express.js (The Backend Framework)
- [ ] **Routing & Middleware**
    - [ ] Application-level, Router-level, and Error-handling Middleware
    - [ ] Built-in Middleware (`express.json`, `express.static`)
    - [ ] Third-party Middleware (CORS, Helmet, Morgan, Cookie-parser)
- [ ] **Request/Response Lifecycle**
    - [ ] `req` and `res` objects deep dive
    - [ ] Status Codes and Response Formats
- [ ] **Security Best Practices**
    - [ ] Rate Limiting and Brute Force Protection
    - [ ] XSS and CSRF prevention in Express
    - [ ] SQL/NoSQL Injection prevention

## 3. React (The Frontend Library)
- [ ] **Core Concepts**
    - [ ] Virtual DOM and Reconciliation (Diffing Algorithm)
    - [ ] JSX and Transpilation (Babel)
    - [ ] Components: Functional vs Class (and why Functional is preferred)
- [ ] **Hooks Deep Dive**
    - [ ] `useState` and `useEffect` (Dependency arrays)
    - [ ] `useContext` for Prop Drilling
    - [ ] `useRef` (Accessing DOM, persisting values)
    - [ ] Performance Hooks: `useMemo` and `useCallback`
    - [ ] `useReducer` for complex state
    - [ ] Custom Hooks development
- [ ] **State Management**
    - [ ] Context API vs Redux Toolkit
    - [ ] Redux Thunk/Saga for Async logic
    - [ ] Modern alternatives: Zustand or React Query (TanStack Query)
- [ ] **Advanced Features**
    - [ ] React Router (Protected Routes, Dynamic Routing)
    - [ ] Code Splitting and Lazy Loading (`React.lazy`, `Suspense`)
    - [ ] Higher-Order Components (HOCs) and Render Props
    - [ ] Server-Side Rendering (SSR) vs Static Site Generation (SSG) basics (Next.js context)

##  4. MongoDB (The Database)
- [ ] **Data Modeling**
    - [ ] Embedding vs Referencing (Normalization vs Denormalization)
    - [ ] Schema Design patterns (One-to-Many, Many-to-Many)
- [ ] **Operations & Aggregation**
    - [ ] CRUD Operations deep dive
    - [ ] Aggregation Pipeline ($match, $group, $project, $lookup, $unwind)
    - [ ] Indexes (Single, Compound, Multikey, TTL, Text)
- [ ] **Advanced MongoDB**
    - [ ] Transactions and ACID compliance
    - [ ] Capped Collections
    - [ ] Sharding and Replication concepts
- [ ] **Mongoose (ODM)**
    - [ ] Schemas, Models, and Validations
    - [ ] Middlewares (Pre/Post hooks)
    - [ ] Population (joins in MongoDB)

## 5. Full Stack Integration & Deployment
- [ ] **Authentication & Authorization**
    - [ ] JWT (JSON Web Tokens) - Access vs Refresh Tokens
    - [ ] OAuth2 and OpenID Connect (Social Login)
    - [ ] Bcrypt for password hashing
- [ ] **API Design**
    - [ ] RESTful API Principles
    - [ ] Error Handling Strategies (Global error handlers)
    - [ ] API Documentation (Swagger/OpenAPI)
- [ ] **Testing**
    - [ ] Unit Testing (Jest)
    - [ ] Integration Testing (Supertest for Express)
    - [ ] React Testing Library (Component testing)
- [ ] **DevOps & Tools**
    - [ ] Dockerizing a MERN application
    - [ ] CI/CD Pipelines (GitHub Actions)
    - [ ] Deployment (AWS, DigitalOcean, Render, Vercel)
    - [ ] Performance Monitoring (PM2, New Relic)

---
