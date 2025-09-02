## Scope

Internal reference of intentionally risky behaviors to test scanners/agents. Do not ship or publish this file.

## Authentication & Authorization

- Weak Defaults: Admin login accepts environment fallback credentials (`ADMIN_USER`/`ADMIN_PASS`) with dev defaults in `.env`.
- Unsigned JWT Trust: Middleware decodes JWT with `jwt.decode` (no signature verification). Endpoints using claim-based checks accept forged tokens.
  - Impact: Privilege escalation by crafting a token with `{ role: 'admin' }`.
- IDOR via Claims: `/user/:id` authorizes access if `req.user.id === :id` or `role==='admin'`, but claims come from an unverified token.

## Cryptography

- Weak Password Hashing: `legacyHash` is non-cryptographic for user passwords (predictable/brute-forceable).

## Injection

- SQL Injection: `/search?username=` builds SQL via string interpolation:
  - `SELECT id, username, role FROM users WHERE username = '${username}'`
  - Impact: Boolean-based or union-based SQLi.

## XSS

- Stored XSS: `/comments` stores raw `text` and `/comments` view renders `text` without escaping (only `author` is escaped).

## Sensitive Data Exposure

- Debug Meta: `/api/meta` returns `process.env`, `tokenSecret`, and `serviceToken` when either `DEBUG_EXPOSE=1` (default) or request header `X-Debug: 1` is present.

## Access Control

- Header-Gated Internal API: `/inventory/all` allows access when header `X-Client-Tier: internal` is set by the client.

## SSRF

- Remote Fetch: `/media/thumbnail?src=` fetches arbitrary `http/https` URLs server-side. No allowlist; internal resources may be reachable. Timeout present but no host validation.

## Race Condition

- Non-Atomic Transfer: `/payments/transfer` updates shared balances with a randomized delay (read/modify/write race).

## File Uploads

- Insecure Uploads: `/api/files` accepts any file and serves via `/uploads` statically. No MIME/extension validation.

## Notes

- Startup: Service loads `.env` via `dotenv`. Defaults in `.env` are intentionally weak for testing.
- Keep this file private (e.g., exclude from artifacts) to avoid tipping off automated agents.

