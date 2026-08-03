# Contributing to PackkitJS

Thanks for helping improve the Packkit ecosystem! These are the shared conventions —
individual repositories may add their own `CONTRIBUTING.md` with project-specific setup,
which takes precedence.

## General flow

1. For anything non-trivial, open an issue first so we can agree on the approach.
2. Branch or fork, keep the change focused, and open a PR against `main`.
3. Make sure the project's checks pass (`lint`, `test`, `build`) before requesting review.

## Ecosystem conventions

- **Provider-neutral core.** `create-packkit` stays a generation engine. Provider
  packages (`@packkit/provider-*`) depend on it — never the reverse — and consume only
  the public deployment contract, not Packkit internals.
- **Pure planning, isolated side effects.** In providers, keep `supports`/`prepare`/`plan`
  pure and deterministic; confine network and I/O to `apply`, through a host-injected client.
- **Small, reviewable PRs.** One concern per PR; avoid drive-by reformatting.
- **Tests alongside behavior.** New behavior ships with a test.

By participating you agree to abide by our [Code of Conduct](CODE_OF_CONDUCT.md).
