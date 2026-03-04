---
"pnpm": patch
---

Fixed flaky tests in CI.

- Upgraded `@pnpm/registry-mock` from `5.2.1` to `5.2.2`, which adds retry with exponential backoff to `getIntegrity()`. This fixes intermittent `ENOENT` failures when Verdaccio proxies a package from the upstream registry and responds to HTTP before finishing the write of the package metadata to its storage directory.
- Added `waitForLines(count, timeout?)` method to `TestIpcServer` that waits for at least `count` newline-terminated messages to be received using an event-driven approach, rather than a fixed-duration `setTimeout`. This removes the fragile timing-based waits from the `TestIpcServer` message handling tests.
- Increased the `execPnpm` timeout from the default 3 minutes to 5 minutes for the `dlx creates cache and store prune cleans cache` test. This test downloads a git-hosted package (`shelljs/shx#61aca968...`) that requires cloning a repository, which can exceed the 3-minute limit on a slow CI runner.
- Fixed race condition in `worker/src/start.ts`: `writeIndexFile()` now uses `${process.pid}${threadId}` for temp filenames so concurrent worker threads in the same process don't write to the same temp file, preventing package manifest corruption in the store.
- Replaced `rimraf@2.5.1` with `is-positive@1.0.0` in the `install --lockfile-only` test. `rimraf@2.5.1` depends on `glob@6.0.4 → minimatch@2.x`, which is not pre-seeded in the Verdaccio registry mock; on CI runners where the Verdaccio proxy to npm is unavailable, pnpm gets `ERR_PNPM_FETCH_404` for `minimatch`. `is-positive@1.0.0` is pre-seeded in the mock and has no transitive npm dependencies.
- Added a 10-minute explicit timeout to the `installation on a workspace with many complex circular dependencies does not fail when auto install peers is on` test in `@pnpm/core`. This test resolves real packages from `https://registry.npmmirror.com` (Angular 14, Ionic 6, tsparticles stack) and routinely runs for over 4 minutes on slow Windows CI runners, exceeding Jest's default `testTimeout` of 4 minutes.
