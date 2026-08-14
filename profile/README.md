# Packkit 📦

A **provider-neutral, multi-language project platform**. One protocol and one
lifecycle vocabulary; many language generators, many interfaces, many compatible
deploy providers. Scaffold a modern project — in JavaScript/TypeScript **or
Python** — from a [web configurator](https://packkit-web.pages.dev/), an
[MCP server](https://www.npmjs.com/package/packkit-mcp) for AI agents, or a CLI,
with a stable embedded API and safe, baseline-aware upgrades.

## Repositories

| Repo | What it is |
| --- | --- |
| **[@packkit/core](https://github.com/PackkitJS/packkit-core)** | The universal protocol + lifecycle primitives — generator contract, deployment contracts, digest, extension, upgrade engine, conformance suites. Browser-safe by default; language-agnostic. |
| **[create-packkit](https://github.com/PackkitJS/create-packkit)** | The **JavaScript/TypeScript** generator — libraries, CLIs, services, SPAs, monorepos. |
| **[create-packkit-py](https://github.com/PackkitJS/create-packkit-py)** | The **Python** generator — pyproject/uv, ruff, pytest, mypy, src layout. |
| **[packkit-web](https://github.com/PackkitJS/packkit-web)** | The browser configurator — pick a language, configure, download a zip. [Live](https://packkit-web.pages.dev/). |
| **[packkit-mcp](https://github.com/PackkitJS/packkit-mcp)** | The Model Context Protocol server — AI agents scaffold & upgrade projects in any supported language as a native tool. |
| **[packkit-actions](https://github.com/PackkitJS/packkit-actions)** | Shared, reusable GitHub Actions — CI, integration, security, dependency-freshness. |
| **[@packkit/provider-netlify](https://github.com/PackkitJS/provider-netlify)** | A deployment provider — consumes a project's deployment **contract** to plan and apply a Netlify site. |

## How it fits together

`@packkit/core` defines the **`PackkitGenerator`** protocol and the shared
lifecycle: generate, extend, digest, export/replay a definition, plan an upgrade,
and describe how to deploy in a provider-neutral **deployment contract**. Each
language generator implements that protocol; interfaces (web, MCP) and providers
consume it — and never a generator's internals.

```text
                     ┌──────────────► packkit-web   (browser interface)
@packkit/core  ◄──── generators ────► packkit-mcp   (agent interface)
 (protocol +         (create-packkit, │
  lifecycle)          create-packkit-py)
        ▲                             └──────────────► providers  (consume DeploymentContract)
        └──── packkit-actions (shared lifecycle automation)
```

The dependency direction is one-way and acyclic. A **provider** consumes a
deployment **contract**, not a language — support is `provider × DeploymentContract`,
so a static site from any generator deploys the same way. Adding a language is one
generator implementing the protocol; the interfaces and providers don't change.

## Three ways to integrate programmatically

- **Universal platform** — `@packkit/core` + `PackkitGenerator` + a generator
  registry. A developer portal, agent host, or API drives JavaScript, Python, and
  future generators through **one** lifecycle, selecting by generator id.
- **Language-specific** — `create-packkit`'s embedded API (or the Python
  equivalent) when a caller deliberately wants a generator's richer, language-aware
  features (e.g. `package.json` merging).
- **Filesystem** — the Node-safe writer (`@packkit/core/node`) when a host wants to
  write a generated project to disk. Generation and upgrade planning stay
  side-effect free.

Explicit registration only — no npm scanning, dynamic download, or arbitrary
plugin execution.
