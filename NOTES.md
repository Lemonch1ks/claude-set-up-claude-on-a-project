# NOTES

## What's in CLAUDE.md, and what I left out

I kept four short sections: a one-line description, the three commands I actually run (`dev`, `test`, `lint`, plus the fact that CI runs lint + tests so both must pass), conventions, and a short architecture map. The conventions are the rules Claude would otherwise guess wrong on: CommonJS instead of ES modules, `node:test` + `supertest` instead of Jest, all data access through `db/store.js`, the `{ error }` JSON shape for errors, converting `:id` with `Number()`, and not adding dependencies without asking.

I deliberately left out: a file-by-file walkthrough (Claude can read the code), the list of endpoints (it changes and is obvious from `routes/`), setup steps like `npm install` and the course instructions from the README (one-off, not needed every session), and anything from `.env` (secrets never go into a committed file). Shorter means every line actually gets followed.

## Permission rules

- **allow:** `npm test`, `npm run lint`, and read-only git (`status`, `diff`, `log`) — safe, run constantly, no reason to be prompted.
- **ask:** `git push`, `git commit`, `npm install` — they change shared history or the dependency tree, so I want to confirm each time.
- **deny:** reading/editing `.env` (and `cat .env`), `git push --force` / `-f`, `git reset --hard`, `rm -rf`.

Without the deny rules Claude could read `.env` while "investigating config" and put real secrets into the conversation, a log or a commit; a force-push could overwrite teammates' work on the remote, and `reset --hard` / `rm -rf` can destroy uncommitted work with no undo.

## Other fix

The original `.gitignore` had leading spaces before `.env` and `.claude/settings.local.json`, and git treats leading spaces as part of the pattern — so neither file was actually ignored (checked with `git check-ignore`). I removed the indentation so secrets and personal settings can't be committed by accident.

## Verification

- `claude --version` → `2.1.309 (Claude Code)`.
- In a fresh session `/memory` lists the project `CLAUDE.md` as loaded, and `/permissions` shows the allow / ask / deny rules from `.claude/settings.json`.
- Asking "How do I run the tests here?" gets `npm test` (node:test runner) straight from `CLAUDE.md`.
