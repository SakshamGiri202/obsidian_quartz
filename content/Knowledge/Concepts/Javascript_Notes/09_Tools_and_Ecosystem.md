# Tools & Ecosystem

> Modern JavaScript tooling: package managers, bundlers, transpilers, and testing frameworks

---

## 1️⃣ Package Managers

### What & Why
Manage dependencies, versions, scripts, and project metadata. Essential for reproducible builds and sharing code.

---

### npm (Node Package Manager)

**Default with Node.js** — largest registry (~2M packages).

```bash
# Initialize project
npm init -y                    # Accept defaults
npm init                       # Interactive

# Install dependencies
npm install <pkg>              # Production (dependencies)
npm install <pkg> --save-dev   # Development (devDependencies)
npm install <pkg> -g           # Global
npm i <pkg>@<version>          # Specific version
npm i <pkg>@latest             # Latest version

# Install from lockfile (CI)
npm ci                         # Clean install from package-lock.json

# Update & audit
npm outdated                   # Check outdated
npm update                     # Update within semver
npm audit                      # Security audit
npm audit fix                  # Auto-fix vulnerabilities

# Run scripts
npm run <script>               # Custom script
npm start                      # start script
npm test                       # test script
npm run build                  # build script

# Publish
npm login
npm publish                    # Publish to registry
npm version patch|minor|major  # Bump version
```

#### `package.json` Key Fields

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "App description",
  "main": "index.js",
  "type": "module",                    // ES modules
  "scripts": {
    "start": "node index.js",
    "dev": "node --watch index.js",
    "test": "jest",
    "build": "vite build",
    "lint": "eslint .",
    "prepare": "husky install"         // Runs on npm install
  },
  "dependencies": {
    "express": "^4.18.0",              // Caret: minor/patch updates
    "lodash": "~4.17.21",              // Tilde: patch only
    "react": "18.2.0",                 // Exact
    "utils": "file:../utils"           // Local path
  },
  "devDependencies": {
    "jest": "^29.0.0",
    "vite": "^5.0.0"
  },
  "peerDependencies": {                // Host must provide
    "react": ">=17"
  },
  "optionalDependencies": {            // Fail silently if missing
    "fsevents": "^2.3.0"
  },
  "engines": {                         // Required Node version
    "node": ">=18.0.0"
  },
  "browserslist": [                    // Target browsers for transpilation
    ">0.2%",
    "not dead",
    "not op_mini all"
  ],
  "sideEffects": false,                // Enable tree-shaking
  "exports": {                         // Package entry points (modern)
    ".": {
      "import": "./dist/index.js",
      "require": "./dist/index.cjs"
    }
  }
}
```

#### `package-lock.json` — Lockfile
- Exact versions for reproducible installs
- Commit to git!
- `npm ci` uses it for fast, deterministic installs

---

### Yarn (Yet Another Resource Negotiator)

**Facebook/Google** — faster, deterministic, workspaces.

```bash
# Install Yarn
npm install -g yarn
# OR corepack (Node 16.10+)
corepack enable
corepack prepare yarn@stable --activate

# Commands (similar to npm)
yarn init -y
yarn add <pkg>              # Production
yarn add <pkg> -D           # Dev
yarn add <pkg> -E           # Exact version
yarn remove <pkg>
yarn upgrade                # Update all
yarn upgrade <pkg>@latest   # Update one

# Install from lockfile
yarn install --frozen-lockfile  # CI mode

# Workspaces (monorepos)
# package.json at root:
{
  "private": true,
  "workspaces": [
    "packages/*",
    "apps/*"
  ]
}
# yarn install → hoists shared deps to root node_modules
```

#### Yarn Features

| Feature | Description |
|---|---|
| **Plug'n'Play (PnP)** | No `node_modules`; uses `.pnp.cjs` for resolution |
| **Zero-Installs** | Commit `.pnp.cjs` + `.yarn/cache` → no install needed |
| **Workspaces** | Native monorepo support |
| **Constraints** | Enforce rules across workspaces |
| **Berry (v2+)** | Modern architecture, TypeScript support |

---

### pnpm (Performant npm)

**Fast, disk-efficient** — hardlinks/ symlinks to global store.

```bash
# Install
npm install -g pnpm
# OR corepack
corepack enable pnpm

# Usage
pnpm add <pkg>
pnpm add -D <pkg>
pnpm install --frozen-lockfile
pnpm run <script>

# Disk efficient: one copy per version in global store
# ~/.pnpm-store → hardlinked to project node_modules
```

#### Why pnpm?
- **Strict** — only access declared dependencies (no phantom deps)
- **Fast** — global store, no duplication
- **Monorepo** — excellent workspace support

---

### Package Manager Comparison

| Feature | npm | Yarn (v1) | Yarn (Berry) | pnpm |
|---|---|---|---|---|
| **Speed** | Medium | Fast | Fastest (PnP) | Fastest |
| **Disk Usage** | High | High | Low (PnP) | Lowest |
| **Monorepo** | Workspaces | Workspaces | Workspaces + Constraints | Workspaces |
| **Lockfile** | package-lock.json | yarn.lock | yarn.lock | pnpm-lock.yaml |
| **Phantom Deps** | Possible | Possible | Impossible (PnP) | Impossible |
| **Node Version** | Built-in | Separate | corepack | corepack |

---

## 2️⃣ Bundlers

### What & Why
Transform, optimize, and bundle modules for browser/Node. Handle:
- **Module resolution** (ESM, CommonJS, JSON, CSS, assets)
- **Tree shaking** (dead code elimination)
- **Code splitting** (lazy loading)
- **Minification** (terser)
- **Dev server** (HMR, proxy)

---

### Webpack — Mature, Configurable

```bash
npm install -D webpack webpack-cli webpack-dev-server
```

#### `webpack.config.js`

```javascript
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const { DefinePlugin } = require("webpack");

module.exports = (env, argv) => {
  const isProd = argv.mode === "production";
  
  return {
    // Entry points
    entry: {
      main: "./src/index.js",
      vendor: ["lodash", "axios"] // Separate vendor chunk
    },
    
    // Output
    output: {
      path: path.resolve(__dirname, "dist"),
      filename: isProd ? "[name].[contenthash].js" : "[name].js",
      chunkFilename: isProd ? "[name].[contenthash].chunk.js" : "[name].chunk.js",
      clean: true,                    // Clean dist before build
      publicPath: "/",                // CDN path
      assetModuleFilename: "assets/[hash][ext][query]"
    },
    
    // Mode
    mode: isProd ? "production" : "development",
    
    // Devtool (source maps)
    devtool: isProd ? "source-map" : "eval-cheap-module-source-map",
    
    // Dev server
    devServer: {
      static: "./dist",
      hot: true,                      // HMR
      port: 3000,
      open: true,
      historyApiFallback: true,       // SPA routing
      proxy: {
        "/api": "http://localhost:4000"
      }
    },
    
    // Module resolution
    resolve: {
      extensions: [".js", ".jsx", ".ts", ".tsx", ".json"],
      alias: {
        "@": path.resolve(__dirname, "src"),
        "@components": path.resolve(__dirname, "src/components")
      },
      modules: ["node_modules", "src"]
    },
    
    // Module rules (loaders)
    module: {
      rules: [
        // JavaScript/TypeScript
        {
          test: /\.[jt]sx?$/,
          exclude: /node_modules/,
          use: {
            loader: "babel-loader",
            options: {
              presets: [
                ["@babel/preset-env", { targets: "defaults" }],
                "@babel/preset-react",
                "@babel/preset-typescript"
              ]
            }
          }
        },
        // CSS
        {
          test: /\.css$/,
          use: [
            isProd ? MiniCssExtractPlugin.loader : "style-loader",
            "css-loader",
            "postcss-loader"
          ]
        },
        // Sass
        {
          test: /\.s[ac]ss$/,
          use: [
            isProd ? MiniCssExtractPlugin.loader : "style-loader",
            "css-loader",
            "postcss-loader",
            "sass-loader"
          ]
        },
        // Images/Fonts/Assets
        {
          test: /\.(png|jpe?g|gif|svg|woff2?|ttf|eot)$/,
          type: "asset/resource"
        },
        // Inline small images
        {
          test: /\.(png|jpe?g|gif|svg)$/,
          type: "asset",
          parser: { dataUrlCondition: { maxSize: 8 * 1024 } }
        }
      ]
    },
    
    // Plugins
    plugins: [
      new HtmlWebpackPlugin({
        template: "./public/index.html",
        minify: isProd
      }),
      new MiniCssExtractPlugin({
        filename: isProd ? "[name].[contenthash].css" : "[name].css"
      }),
      new DefinePlugin({
        "process.env.NODE_ENV": JSON.stringify(process.env.NODE_ENV),
        "process.env.API_URL": JSON.stringify(process.env.API_URL)
      })
    ],
    
    // Optimization
    optimization: {
      splitChunks: {
        chunks: "all",
        cacheGroups: {
          vendor: {
            test: /[\\/]node_modules[\\/]/,
            name: "vendors",
            priority: 10
          },
          common: {
            minChunks: 2,
            priority: 5,
            reuseExistingChunk: true
          }
        }
      },
      runtimeChunk: "single",
      minimize: isProd,
      minimizer: [
        new TerserPlugin({
          terserOptions: {
            compress: { drop_console: isProd }
          }
        })
      ]
    },
    
    // Performance hints
    performance: {
      maxAssetSize: 250000,
      maxEntrypointSize: 250000
    }
  };
};
```

#### Key Webpack Concepts

| Concept | Description |
|---|---|
| **Entry** | Starting point(s) for dependency graph |
| **Output** | Where/how to emit bundles |
| **Loaders** | Transform non-JS files (babel, css, file) |
| **Plugins** | Hook into build lifecycle (HtmlWebpackPlugin, DefinePlugin) |
| **Chunks** | Output files (entry, async, vendor) |
| **Tree Shaking** | Remove unused exports (needs ESM + `sideEffects: false`) |
| **Code Splitting** | `import()` dynamic imports → separate chunks |

---

### Vite — Fast, Modern, Unbundled Dev

```bash
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install
npm run dev
```

#### `vite.config.js`

```javascript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import path from "path";

export default defineConfig(({ mode }) => {
  const isProd = mode === "production";
  
  return {
    plugins: [react()],
    
    resolve: {
      alias: {
        "@": path.resolve(__dirname, "./src"),
      }
    },
    
    // Dev server
    server: {
      port: 3000,
      open: true,
      proxy: {
        "/api": {
          target: "http://localhost:4000",
          changeOrigin: true
        }
      }
    },
    
    // Build (uses Rollup under the hood)
    build: {
      outDir: "dist",
      sourcemap: true,
      minify: "terser",
      terserOptions: {
        compress: { drop_console: isProd }
      },
      rollupOptions: {
        output: {
          manualChunks: {
            vendor: ["react", "react-dom", "react-router-dom"],
            utils: ["lodash", "date-fns"]
          }
        }
      },
      chunkSizeWarningLimit: 500
    },
    
    // CSS
    css: {
      modules: {
        localsConvention: "camelCase"
      },
      preprocessorOptions: {
        scss: {
          additionalData: `@use "@/styles/variables" as *;`
        }
      }
    },
    
    // Environment variables
    envPrefix: "VITE_",  // Only VITE_* exposed to client
    define: {
      __APP_VERSION__: JSON.stringify(process.env.npm_package_version)
    }
  };
});
```

#### Vite vs Webpack

| Aspect | Webpack | Vite |
|---|---|---|
| **Dev Start** | Slow (bundles all) | Instant (native ESM) |
| **HMR** | Good | Excellent (fine-grained) |
| **Config** | Complex | Simple, sensible defaults |
| **Production** | Webpack | Rollup |
| **Plugins** | Huge ecosystem | Growing, Rollup-compatible |
| **Legacy Browser** | Built-in | Plugin (`@vitejs/plugin-legacy`) |
| **TypeScript** | Loader | Native (esbuild) |

---

### Rollup — Library Bundler

```bash
npm install -D rollup @rollup/plugin-node-resolve @rollup/plugin-commonjs @rollup/plugin-typescript
```

#### `rollup.config.js`

```javascript
import resolve from "@rollup/plugin-node-resolve";
import commonjs from "@rollup/plugin-commonjs";
import typescript from "@rollup/plugin-typescript";
import { terser } from "rollup-plugin-terser";
import dts from "rollup-plugin-dts";

const packageJson = require("./package.json");

export default [
  // Main bundle (ESM + CJS)
  {
    input: "src/index.ts",
    output: [
      {
        file: packageJson.main,        // dist/index.cjs
        format: "cjs",
        sourcemap: true
      },
      {
        file: packageJson.module,      // dist/index.js
        format: "esm",
        sourcemap: true
      }
    ],
    plugins: [
      resolve({ browser: true }),
      commonjs(),
      typescript({ tsconfig: "./tsconfig.json" }),
      terser()
    ],
    external: ["react", "lodash"]     // Don't bundle peer deps
  },
  // Type declarations
  {
    input: "src/index.ts",
    output: [{ file: "dist/index.d.ts", format: "esm" }],
    plugins: [dts()],
    external: [/\.css$/]
  }
];
```

---

### esbuild — Extremely Fast (Go-based)

```bash
npm install -D esbuild
```

```javascript
// build.js
require("esbuild").build({
  entryPoints: ["src/index.ts"],
  bundle: true,
  outfile: "dist/index.js",
  platform: "node",           // or "browser"
  format: "esm",              // or "cjs", "iife"
  target: "node18",
  sourcemap: true,
  minify: true,
  external: ["react"],
  define: { "process.env.NODE_ENV": "\"production\"" }
}).catch(() => process.exit(1));
```

- **10-100x faster** than Webpack/Rollup
- Used by Vite, Next.js (Turbopack), Remix
- Limited plugin ecosystem vs Webpack

---

## 3️⃣ Transpilers

### What & Why
Convert modern JS/TS to target environment compatible code.

---

### Babel — The Standard

```bash
npm install -D @babel/core @babel/cli @babel/preset-env @babel/preset-react @babel/preset-typescript
```

#### `babel.config.js`

```javascript
module.exports = (api) => {
  api.cache(true);
  
  const isProd = process.env.NODE_ENV === "production";
  
  return {
    presets: [
      // Auto-detect target environments from browserslist
      ["@babel/preset-env", {
        targets: { browsers: [">0.2%", "not dead"] },
        useBuiltIns: "usage",        // Polyfill only what's used
        corejs: 3,                   // core-js version
        modules: false,              // Keep ESM for bundler tree-shaking
        bugfixes: true               // Transpile less for modern targets
      }],
      ["@babel/preset-react", {
        runtime: "automatic",        // New JSX transform (no React import)
        development: !isProd
      }],
      "@babel/preset-typescript"     // Strip types (no type checking!)
    ],
    
    plugins: [
      // Stage 3+ proposals
      "@babel/plugin-proposal-decorators",
      // Optional: transform runtime (smaller bundles)
      ["@babel/plugin-transform-runtime", {
        corejs: 3,
        helpers: true,
        regenerator: true
      }],
      // Remove console.log in prod
      isProd && ["transform-remove-console", { exclude: ["error", "warn"] }]
    ].filter(Boolean),
    
    // Environment-specific overrides
    env: {
      test: {
        presets: [
          ["@babel/preset-env", { targets: { node: "current" } }]
        ]
      }
    }
  };
};
```

#### Key Babel Concepts

| Concept | Description |
|---|---|
| **Presets** | Pre-configured plugin sets (`@babel/preset-env`, `@babel/preset-react`) |
| **Plugins** | Individual transformations |
| **Polyfills** | `core-js` + `regenerator-runtime` for missing APIs |
| **browserslist** | Shared target config (in package.json or `.browserslistrc`) |
| **Transform Runtime** | Reuse helpers, avoid duplication |

---

### TypeScript Compiler (tsc)

```bash
npm install -D typescript
npx tsc --init
```

#### `tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2022", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "moduleResolution": "bundler",
    "jsx": "react-jsx",
    "strict": true,
    "noEmit": true,                    // Type-check only (let bundler emit)
    "isolatedModules": true,           // Required for Babel/esbuild
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "skipLibCheck": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    },
    "typeRoots": ["./node_modules/@types", "./src/types"]
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist"]
}
```

#### TypeScript vs Babel

| Feature | TypeScript | Babel |
|---|---|---|
| **Type Checking** | Yes | No (strip only) |
| **Speed** | Slower | Faster |
| **Config** | tsconfig.json | babel.config.js |
| **Output** | .js + .d.ts | .js only |
| **Non-standard syntax** | Limited | Via plugins |

---

### SWC / oxc — Rust-based Fast Alternatives

```bash
# SWC (Speedy Web Compiler)
npm install -D @swc/core @swc/cli

# oxc (newer, even faster)
npm install -D oxc
```

```javascript
// .swcrc
{
  "jsc": {
    "parser": { "syntax": "typescript", "tsx": true },
    "transform": { "react": { "runtime": "automatic" } },
    "target": "es2022"
  },
  "module": { "type": "es6" }
}
```

- **20-70x faster** than Babel
- Used by Next.js, Remix, Vite (optional)
- Drop-in replacement for Babel in most cases

---

## 4️⃣ Testing

### What & Why
Verify correctness, prevent regressions, document behavior, enable refactoring.

---

### Jest — Zero-Config, Batteries Included

```bash
npm install -D jest @types/jest ts-jest
npx jest --init
```

#### `jest.config.js`

```javascript
export default {
  // Test environment
  testEnvironment: "jsdom",           // or "node"
  
  // TypeScript
  preset: "ts-jest",
  transform: {
    "^.+\\.tsx?$": ["ts-jest", { useESM: true }]
  },
  
  // Module resolution
  moduleNameMapper: {
    "^@/(.*)$": "<rootDir>/src/$1",
    "\\.(css|less|scss|sass)$": "identity-obj-proxy"
  },
  
  // Setup
  setupFilesAfterEnv: ["<rootDir>/jest.setup.ts"],
  
  // Coverage
  collectCoverageFrom: [
    "src/**/*.{ts,tsx}",
    "!src/**/*.d.ts",
    "!src/main.tsx"
  ],
  coverageThreshold: {
    global: { branches: 80, functions: 80, lines: 80, statements: 80 }
  },
  
  // Patterns
  testMatch: ["**/__tests__/**/*.test.{ts,tsx}"],
  testPathIgnorePatterns: ["/node_modules/", "/dist/"],
  
  // Watch mode
  watchPlugins: [
    "jest-watch-typeahead/filename",
    "jest-watch-typeahead/testname"
  ],
  
  // Mocks
  clearMocks: true,
  resetMocks: true,
  restoreMocks: true
};
```

#### `jest.setup.ts`

```typescript
import "@testing-library/jest-dom";
import { TextEncoder, TextDecoder } from "util";

// Polyfills for jsdom
global.TextEncoder = TextEncoder;
global.TextDecoder = TextDecoder;

// Mock window.matchMedia
Object.defineProperty(window, "matchMedia", {
  writable: true,
  value: jest.fn().mockImplementation(query => ({
    matches: false,
    media: query,
    onchange: null,
    addListener: jest.fn(),
    removeListener: jest.fn(),
    addEventListener: jest.fn(),
    removeEventListener: jest.fn(),
    dispatchEvent: jest.fn()
  }))
});

// Suppress console.error in tests (optional)
const originalError = console.error;
beforeAll(() => {
  console.error = (...args) => {
    if (args[0]?.includes?.("Warning: ReactDOM.render")) return;
    originalError.call(console, ...args);
  };
});
afterAll(() => { console.error = originalError; });
```

#### Writing Tests

```typescript
// math.test.ts
import { add, multiply } from "./math";

describe("Math utilities", () => {
  describe("add", () => {
    test("adds two positive numbers", () => {
      expect(add(2, 3)).toBe(5);
    });
    
    test("handles negative numbers", () => {
      expect(add(-1, 1)).toBe(0);
    });
    
    // Parameterized tests
    test.each([
      [1, 2, 3],
      [0, 0, 0],
      [-1, -2, -3]
    ])("add(%i, %i) = %i", (a, b, expected) => {
      expect(add(a, b)).toBe(expected);
    });
  });
  
  // Async testing
  test("fetchUser resolves with user data", async () => {
    const user = await fetchUser(1);
    expect(user).toEqual({ id: 1, name: "Alice" });
  });
  
  // Mocking
  test("calls API with correct params", async () => {
    const mockFetch = jest.fn().mockResolvedValue({ json: () => ({ id: 1 }) });
    global.fetch = mockFetch;
    
    await fetchUser(1);
    
    expect(mockFetch).toHaveBeenCalledWith("/api/users/1");
  });
  
  // Snapshot testing
  test("renders correctly", () => {
    const { asFragment } = render(<UserCard name="Alice" />);
    expect(asFragment()).toMatchSnapshot();
  });
});
```

#### React Testing Library

```bash
npm install -D @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

```tsx
// UserCard.test.tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { UserCard } from "./UserCard";

test("displays user name and handles click", async () => {
  const handleClick = jest.fn();
  render(<UserCard name="Alice" onClick={handleClick} />);
  
  // Queries (prefer user-facing)
  expect(screen.getByText("Alice")).toBeInTheDocument();
  expect(screen.getByRole("button", { name: /view profile/i })).toBeInTheDocument();
  
  // User interactions
  await userEvent.click(screen.getByRole("button"));
  expect(handleClick).toHaveBeenCalledTimes(1);
});
```

---

### Vitest — Vite-Native, Jest-Compatible

```bash
npm install -D vitest @vitest/ui
```

```typescript
// vitest.config.ts
import { defineConfig } from "vitest/config";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  test: {
    environment: "jsdom",
    setupFiles: ["./vitest.setup.ts"],
    include: ["src/**/*.test.{ts,tsx}"],
    coverage: {
      provider: "v8",
      reporter: ["text", "json", "html"]
    },
    // UI: npx vitest --ui
  }
});
```

- **Same API as Jest** — drop-in replacement
- **Native ESM** — no config needed for Vite projects
- **Watch mode** — incredibly fast

---

### Mocha — Flexible, Minimal

```bash
npm install -D mocha @types/mocha chai @types/chai ts-node
```

#### `.mocharc.json`

```json
{
  "spec": "src/**/*.test.ts",
  "require": ["ts-node/register"],
  "timeout": 5000,
  "slow": 100,
  "reporter": "spec",
  "ui": "bdd",
  "watch": false,
  "bail": false
}
```

#### Writing Tests

```typescript
// math.test.ts
import { expect } from "chai";
import { add, fetchUser } from "./math";

describe("Math utilities", () => {
  describe("add()", () => {
    it("adds two positive numbers", () => {
      expect(add(2, 3)).to.equal(5);
    });
    
    it("handles negative numbers", () => {
      expect(add(-1, 1)).to.equal(0);
    });
  });
  
  describe("fetchUser()", () => {
    it("resolves with user data", async () => {
      const user = await fetchUser(1);
      expect(user).to.deep.equal({ id: 1, name: "Alice" });
    });
  });
});
```

#### Mocha + Chai + Sinon (Mocking)

```bash
npm install -D sinon @types/sinon
```

```typescript
import { expect } from "chai";
import sinon from "sinon";
import { UserService } from "./UserService";

describe("UserService", () => {
  let sandbox: sinon.SinonSandbox;
  
  beforeEach(() => { sandbox = sinon.createSandbox(); });
  afterEach(() => { sandbox.restore(); });
  
  it("fetches user from API", async () => {
    const mockFetch = sandbox.stub(global, "fetch").resolves({
      json: () => Promise.resolve({ id: 1, name: "Alice" })
    } as Response);
    
    const service = new UserService();
    const user = await service.getUser(1);
    
    expect(user.name).to.equal("Alice");
    expect(mockFetch.calledOnceWith("/api/users/1")).to.be.true;
  });
});
```

---

### Cypress / Playwright — E2E Testing

```bash
# Cypress
npm install -D cypress
npx cypress open

# Playwright (recommended)
npm install -D @playwright/test
npx playwright install
```

```typescript
// playwright.config.ts
import { defineConfig, devices } from "@playwright/test";

export default defineConfig({
  testDir: "./e2e",
  fullyParallel: true,
  retries: 2,
  workers: "50%",
  reporter: "html",
  use: {
    baseURL: "http://localhost:3000",
    trace: "on-first-retry",
    screenshot: "only-on-failure"
  },
  projects: [
    { name: "chromium", use: { ...devices["Desktop Chrome"] } },
    { name: "firefox", use: { ...devices["Desktop Firefox"] } },
    { name: "webkit", use: { ...devices["Desktop Safari"] } },
    { name: "Mobile Chrome", use: { ...devices["Pixel 5"] } },
    { name: "Mobile Safari", use: { ...devices["iPhone 12"] } }
  ],
  webServer: {
    command: "npm run dev",
    url: "http://localhost:3000",
    reuseExistingServer: !process.env.CI
  }
});
```

```typescript
// e2e/login.test.ts
import { test, expect } from "@playwright/test";

test("user can login", async ({ page }) => {
  await page.goto("/login");
  await page.fill("#email", "user@example.com");
  await page.fill("#password", "secret");
  await page.click('button[type="submit"]');
  await expect(page).toHaveURL("/dashboard");
  await expect(page.locator("h1")).toContainText("Welcome");
});
```

---

### Testing Strategy (Testing Trophy)

```
        ┌─────────────┐
        │   E2E       │  ← Few, critical paths (Cypress/Playwright)
        │  (Cypress)  │
        ├─────────────┤
        │ Integration │  ← API, component integration (Vitest/Jest)
        ├─────────────┤
        │   Unit      │  ← Many, pure functions, hooks (Vitest/Jest)
        └─────────────┘
```

| Type | Tools | Speed | Scope |
|---|---|---|---|
| **Unit** | Vitest, Jest | ⚡⚡⚡ | Pure functions, hooks, utils |
| **Integration** | Vitest, Jest | ⚡⚡ | Component trees, API routes |
| **E2E** | Playwright, Cypress | ⚡ | Real user flows, cross-browser |

---

## 📦 Additional Essential Tools

### Linting & Formatting

```bash
# ESLint
npm install -D eslint @eslint/js typescript-eslint eslint-plugin-react eslint-plugin-react-hooks

# Prettier
npm install -D prettier eslint-config-prettier eslint-plugin-prettier
```

```javascript
// eslint.config.js (flat config, ESLint 9+)
import js from "@eslint/js";
import tseslint from "typescript-eslint";
import reactPlugin from "eslint-plugin-react";
import prettierConfig from "eslint-config-prettier";

export default tseslint.config(
  js.configs.recommended,
  ...tseslint.configs.recommended,
  reactPlugin.configs.recommended,
  prettierConfig,
  {
    rules: {
      "react/react-in-jsx-scope": "off",
      "@typescript-eslint/no-unused-vars": ["warn", { argsIgnorePattern: "^_" }]
    }
  }
);
```

```json
// .prettierrc
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 100,
  "bracketSpacing": true,
  "arrowParens": "avoid"
}
```

### Git Hooks

```bash
# Husky + lint-staged
npm install -D husky lint-staged
npx husky install
npx husky add .husky/pre-commit "npx lint-staged"
```

```json
// package.json
{
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": ["eslint --fix", "prettier --write"],
    "*.{json,md,css}": ["prettier --write"]
  }
}
```

---

## 📚 Summary Cheatsheet

| Category | Tools | Best For |
|---|---|---|
| **Package Manager** | npm, Yarn, pnpm | pnpm (speed/disk), Yarn (monorepo), npm (default) |
| **Bundler** | Vite, Webpack, Rollup, esbuild | Vite (apps), Rollup (libs), esbuild (speed) |
| **Transpiler** | Babel, SWC, TypeScript, oxc | SWC/oxc (speed), tsc (type-check) |
| **Unit/Integration** | Vitest, Jest | Vitest (Vite), Jest (legacy) |
| **E2E** | Playwright, Cypress | Playwright (modern, cross-browser) |
| **Lint/Format** | ESLint + Prettier | Standard combo |

---

## 🔗 Resources

- [npm Docs](https://docs.npmjs.com/)
- [Yarn Docs](https://yarnpkg.com/)
- [pnpm Docs](https://pnpm.io/)
- [Webpack Docs](https://webpack.js.org/)
- [Vite Docs](https://vitejs.dev/)
- [Rollup Docs](https://rollupjs.org/)
- [esbuild Docs](https://esbuild.github.io/)
- [Babel Docs](https://babeljs.io/)
- [TypeScript Docs](https://www.typescriptlang.org/)
- [Vitest Docs](https://vitest.dev/)
- [Jest Docs](https://jestjs.io/)
- [Playwright Docs](https://playwright.dev/)
- [Testing Library](https://testing-library.com/)