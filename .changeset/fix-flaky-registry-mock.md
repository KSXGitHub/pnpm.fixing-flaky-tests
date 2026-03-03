---
"pnpm": patch
---

Upgraded `@pnpm/registry-mock` to 5.2.2 to fix flaky tests caused by a race condition in `getIntegrity()`. Verdaccio may respond to HTTP requests before finishing writing package metadata to its storage directory; the new version retries with exponential backoff on `ENOENT` or truncated JSON errors.
