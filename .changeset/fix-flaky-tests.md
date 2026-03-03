---
"pnpm": patch
---

Fixed flaky tests in CI.

- Upgraded `@pnpm/registry-mock` from `5.2.1` to `5.2.2`, which adds retry with exponential backoff to `getIntegrity()`. This fixes intermittent `ENOENT` failures when Verdaccio proxies a package from the upstream registry and responds to HTTP before finishing the write of the package metadata to its storage directory.
- Added `waitForLines(count, timeout?)` method to `TestIpcServer` that waits for at least `count` newline-terminated messages to be received using an event-driven approach, rather than a fixed-duration `setTimeout`. This removes the fragile timing-based waits from the `TestIpcServer` message handling tests.
