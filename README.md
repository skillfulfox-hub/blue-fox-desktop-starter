# 🦊 Blue Fox: Ultimate Desktop Starter Kit

> **Note:** This is a **Public Documentation Repository**. The full source code, pre-configured Nx workspace, and production-ready sidecar orchestration are available exclusively through a commercial license.
> 
> [👉 **Get Instant Access to Full Source Code**](https://skillfulfox.lemonsqueezy.com/)

---

Blue Fox: Ultimate Starter Kit
Commercial full-stack starter kit for building desktop-first applications with NestJS + React + Tauri.

This repository is an Nx monorepo that ships:

a React desktop client (Mantine UI),
a NestJS API sidecar (JWT auth with bearer + cookie fallback),
a shared types package (@blue-fox/api-interfaces),
ready-to-extend auth/users/dashboard flows,
i18n support (en, de, ua).
Why This Kit
Desktop-first architecture with Tauri sidecar process management.
Fast product bootstrap: auth, user management, API docs, app metadata endpoint.
Flexible persistence: SQLite, PostgreSQL, or MongoDB through TypeORM config switching.
Commercial license model for product teams and client delivery.
Tech Stack
Monorepo: Nx
Frontend: React 19, Vite, Mantine, React Router
Desktop shell: Tauri v2 (Rust)
Backend: NestJS 11, Passport JWT, Swagger
Data layer: TypeORM with SQLite/PostgreSQL/MongoDB options
Tests: Vitest, Playwright, Jest
Repository Structure
apps/
  client/        React desktop UI
  server/        NestJS API sidecar
  client-e2e/    Playwright smoke tests
  server-e2e/    Jest e2e scaffold
libs/
  api-interfaces Shared contracts and DTO interfaces
src-tauri/
  src/main.rs    Tauri bootstrap and sidecar orchestration
  tauri.conf.json
LICENSE.md       Commercial license agreement
PRODUCTION_CHECKLIST.md
Requirements
Node.js >=20.19.0
npm >=10
Rust toolchain (for Tauri)
Tauri system prerequisites for your OS
Windows Notes
This repository is currently configured for Windows sidecar packaging (node18-win-x64 in server:pkg).
WebView2 runtime is required for Tauri desktop execution on Windows.
Quick Start (Recommended Desktop Flow)
Install dependencies:
npm install
Build the API sidecar and run desktop app in dev mode:
npm run fox:dev
What this does:

builds NestJS server bundle,
packages it into src-tauri/binaries/server-x86_64-pc-windows-msvc.exe,
starts Tauri dev mode with the React frontend.
Default Demo Credentials
Username: admin
Password: admin123
The admin user is auto-seeded if missing.

Authentication Model
POST /api/auth/login returns { access_token, user }.
The desktop client stores access_token in local storage and sends Authorization: Bearer <token> on API requests.
The server also sets HTTP-only access_token cookie as a compatibility fallback for browser-style flows.
GET /api/auth/profile accepts both Bearer token and cookie.
Environment Configuration
No .env template is included by default. The server reads these variables:

Variable	Default	Description
PORT	3000	API server port (for standalone server mode)
SWAGGER_ENABLED	true	Set to false to disable Swagger
DB_TYPE	sqlite	sqlite, postgres, or mongodb
DB_PATH	blue-fox-db.sqlite	SQLite file path
DB_HOST	localhost	PostgreSQL host
DB_PORT	5432	PostgreSQL port
DB_USERNAME	postgres	PostgreSQL username
DB_PASSWORD	password	PostgreSQL password
DB_NAME	blue_fox_db	PostgreSQL database name
MONGO_URI	mongodb://localhost/blue_fox_db	MongoDB connection URI
NODE_ENV	development	Affects cookie security behavior
Database Mode Examples
SQLite (default):

set DB_TYPE=sqlite
set DB_PATH=blue-fox-db.sqlite
PostgreSQL:

set DB_TYPE=postgres
set DB_HOST=localhost
set DB_PORT=5432
set DB_USERNAME=postgres
set DB_PASSWORD=your_password
set DB_NAME=blue_fox_db
MongoDB:

set DB_TYPE=mongodb
set MONGO_URI=mongodb://localhost/blue_fox_db
Development Commands
npm scripts
Command	Purpose
npm run fox:dev	Full desktop dev flow (build/package sidecar + tauri dev)
npm run start	Run tauri dev only (expects sidecar binary already built)
npm run server:build	Build NestJS server bundle (dist/apps/server)
npm run server:pkg	Package server sidecar executable with pkg
npm run server:build:pkg	Build + package sidecar
Nx commands (examples)
Command	Purpose
npx nx serve server	Run backend server in node mode
npx nx build client	Build frontend bundle
npx nx serve client	Run client dev server (used by Tauri beforeDevCommand)
npx nx test server	Run server unit tests
npx nx test client	Run client unit tests (Vitest)
npx nx e2e client-e2e	Run Playwright smoke tests
npx nx run server-e2e:e2e	Run server e2e suite
To inspect available targets in your environment:

npx nx show project <project-name> --web
API Snapshot
Base URL in desktop runtime is dynamic (http://localhost:<sidecar-port>/api). In Tauri mode, sidecar port is selected automatically from 3010..3099.

Core endpoints:

GET /api - sidecar info and stack versions
POST /api/auth/register
POST /api/auth/login - returns JWT + user payload
POST /api/auth/logout
GET /api/auth/profile
GET /api/users
PATCH /api/users/:id
DELETE /api/users/:id
Swagger (when enabled):

/api/docs
/api/docs-json
Build and Release
Desktop release build
Build desktop bundles:
npx tauri build
beforeBuildCommand already rebuilds and repackages the sidecar (npm run server:build:pkg) before client bundling.

Expected Tauri artifacts are generated under src-tauri/target/release/bundle/.

Productization and Production Hardening
Before shipping to real customers, review and complete:

PRODUCTION_CHECKLIST.md
Critical defaults you must change:

Replace demo JWT secret values in apps/server/src/app/auth/auth.module.ts and apps/server/src/app/auth/jwt.strategy.service.ts.
Disable TypeORM synchronize in production and introduce migrations.
Rotate/remove demo credentials and tighten CORS/cookie settings for your deployment.
Update Tauri identity metadata in src-tauri/tauri.conf.json (identifier, productName, window title) for your brand.
Troubleshooting
pkg is not recognized during npm run server:pkg: install it in the workspace (npm i -D pkg) or make sure a global pkg binary is available.
Tauri starts but shows API connection error: rebuild sidecar (npm run server:build:pkg) and restart npm run fox:dev.
Login succeeds but UI returns to login: rebuild/reinstall latest desktop bundle, then clear blue_fox_access_token in app local storage and retry.
White/blank desktop window on Windows: verify WebView2 Runtime is installed and up to date.
Swagger missing: ensure SWAGGER_ENABLED is not set to false.
Localization
Client translations are stored in:

apps/client/public/i18/en.json
apps/client/public/i18/de.json
apps/client/public/i18/ua.json
License
This project is distributed under a commercial license.

Read full terms in LICENSE.md.

Key restrictions include:

no source redistribution as a starter kit/template,
no public source repository hosting of licensed source,
no competitive resale as a developer-facing starter/template product.
Support
Licensor: Skillful Fox Studio Contact: skillfulfoxstudio[at]proton.me
