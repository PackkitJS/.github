# Packkit 📦

A **provider-neutral, multi-language project platform**. One protocol and one
lifecycle vocabulary; many language generators, many interfaces, many compatible
deploy providers. Scaffold a modern project — in JavaScript/TypeScript, **Python**,
or **Go** — from a [web configurator](https://packkit-web.pages.dev/), an
[MCP server](https://www.npmjs.com/package/packkit-mcp) for AI agents, or a CLI,
with a stable embedded API and safe, baseline-aware upgrades.

## Repositories

| Repo | What it is |
| --- | --- |
| **[@packkit/core](https://github.com/PackkitLabs/packkit-core)** | The universal protocol + lifecycle primitives — generator contract, deployment contracts, digest, extension, upgrade engine, conformance suites. Browser-safe by default; language-agnostic. |
| **[create-packkit-js](https://github.com/PackkitLabs/create-packkit-js)** | The **JavaScript/TypeScript** generator — libraries, CLIs, services, SPAs, monorepos. Published as `create-packkit` on npm (`npx create-packkit`). |
| **[create-packkit-py](https://github.com/PackkitLabs/create-packkit-py)** | The **Python** generator — pyproject/uv, ruff, pytest, mypy, src layout. |
| **[create-packkit-go](https://github.com/PackkitLabs/create-packkit-go)** | The **Go** generator — go.mod modules, idiomatic layout, table tests; library/CLI/worker/HTTP service. |
| **[packkit-web](https://github.com/PackkitLabs/packkit-web)** | The browser configurator — pick a language, configure, download a zip. [Live](https://packkit-web.pages.dev/). |
| **[packkit-mcp](https://github.com/PackkitLabs/packkit-mcp)** | The Model Context Protocol server — AI agents scaffold & upgrade projects in any supported language as a native tool. |
| **[packkit-actions](https://github.com/PackkitLabs/packkit-actions)** | Shared, reusable GitHub Actions — CI, integration, security, dependency-freshness. |
| **[@packkit/provider-netlify](https://github.com/PackkitLabs/provider-netlify)** | A deployment provider — consumes a project's deployment **contract** to plan and apply a Netlify site. |
| **[@packkit/provider-aws](https://github.com/PackkitLabs/provider-aws)** | A deployment provider — turns a project's deployment **contract** into OpenTofu/Terraform + a GitHub-OIDC pipeline (static → S3/CloudFront, service → App Runner, worker → ECS Fargate). |

## How it fits together

`@packkit/core` defines the **`PackkitGenerator`** protocol and the shared
lifecycle: generate, extend, digest, export/replay a definition, plan an upgrade,
and describe how to deploy in a provider-neutral **deployment contract**. Each
language generator implements that protocol; interfaces (web, MCP) and providers
consume it — and never a generator's internals.

```text
                      ┌─────────────► packkit-web   (browser interface)
@packkit/core  ◄──── generators ────► packkit-mcp   (agent interface)
 (protocol +        (create-packkit-js, │
  lifecycle)         create-packkit-py, │
        ▲            create-packkit-go)  └─────────► providers  (consume DeploymentContract:
        │                                              provider-netlify, provider-aws)
        └──── packkit-actions (shared lifecycle automation)
```

The dependency direction is one-way and acyclic. A **provider** consumes a
deployment **contract**, not a language — support is `provider × DeploymentContract`,
so a `service` from the JavaScript, Python, or Go generator deploys the same way.
Adding a language is one generator implementing the protocol; the interfaces and
providers don't change.

## Three ways to integrate programmatically

- **Universal platform** — `@packkit/core` + `PackkitGenerator` + a generator
  registry. A developer portal, agent host, or API drives JavaScript, Python, Go,
  and future generators through **one** lifecycle, selecting by generator id.
- **Language-specific** — a generator's embedded API when a caller deliberately
  wants its richer, language-aware features (e.g. `package.json` merging in
  `create-packkit`, `go.mod` diffing in `create-packkit-go`).
- **Filesystem** — the Node-safe writer (`@packkit/core/node`) when a host wants to
  write a generated project to disk. Generation and upgrade planning stay
  side-effect free.

Explicit registration only — no npm scanning, dynamic download, or arbitrary
plugin execution.
