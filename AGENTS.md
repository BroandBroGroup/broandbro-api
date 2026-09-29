# Base44 Dev Environment

## Project Overview
Cloudflare Worker API built with Hono + Chanfana (OpenAPI 3.1) + D1 (SQLite). Runs locally via `wrangler dev` using workerd runtime with a local D1 database — no external credentials required.

## Running the App
```bash
docker compose -f docker-compose.base44.yml up -d
```
- Service listens on host port **3000** (mapped from container port 3000).
- On startup: installs npm deps (if missing), applies D1 migrations locally, then starts `wrangler dev`.
- Wrangler dev watches `src/` and reloads on file changes (live reload works for source edits).
- Local D1 state lives in `.wrangler/state/v3/d1/` (persisted via bind mount).

## Health Check
- `GET /` serves the OpenAPI docs page (HTML, 200 OK).
- `GET /openapi.json` returns the generated OpenAPI schema.

## API Endpoints
- `GET /` — OpenAPI docs
- `GET /openapi.json` — OpenAPI schema
- `GET /tasks` — list tasks
- `POST /tasks` — create task (body: `name`, `slug`, `description`, `completed` (boolean), `due_date` (ISO datetime))
- `GET /tasks/:id` — read task
- `PUT /tasks/:id` — update task
- `DELETE /tasks/:id` — delete task
- `POST /dummy/:slug` — example endpoint

## No External Secrets
All infrastructure (D1 database) runs locally inside the container. No external service credentials are needed to boot or run the app.

## Tests
```bash
npm run test
```
Uses Vitest with `@cloudflare/vitest-pool-workers`. Tests are in `tests/`.
