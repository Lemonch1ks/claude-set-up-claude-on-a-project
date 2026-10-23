# CLAUDE.md

Small Express REST API (users + health check) with an in-memory store, used as the course starter project.

## Commands

- `npm run dev` — start the API with auto-reload on http://localhost:3000
- `npm test` — run all tests (Node's built-in `node:test` runner, files in `tests/`)
- `npm run lint` — ESLint; CI runs lint then tests on Node 22, so both must pass

## Conventions

- CommonJS only: use `require` / `module.exports`, not `import` / `export`.
- Tests use `node:test` + `node:assert` + `supertest`, not Jest or Mocha. Import `app` from `../server` and never call `app.listen` in tests.
- Route handlers never touch the `users` array directly — go through functions exported from `db/store.js`; add a new store function if one is missing.
- Errors are returned as JSON `{ "error": "<message>" }` with the right status (400 bad input, 404 not found).
- Convert `req.params.id` with `Number()` before looking it up — store ids are numbers.
- Do not add new dependencies without asking.

## Architecture

- `server.js` — creates the Express app, mounts routers, exports `app`; only listens when run directly.
- `routes/` — one file per resource (`users.js`, `health.js`), each exports an `express.Router()` mounted in `server.js` at `/<resource>`.
- `db/store.js` — the only data-access layer; in-memory, resets on restart.
- Config comes from environment variables (`PORT`); see `.env.example`. Never read or edit `.env`.
