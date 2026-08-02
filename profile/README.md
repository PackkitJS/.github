# Packkit 📦

Provider-neutral project generation for modern JavaScript — and a growing set of
deploy providers that launch what it generates.

## Packages

| Package | What it is |
| --- | --- |
| **[create-packkit](https://www.npmjs.com/package/create-packkit)** | The generator. Scaffold a modern npm package, CLI, service, or app from a CLI **or** a [web configurator](https://danmat.github.io/create-packkit/) — with safe, baseline-aware upgrades and a stable embedded API. |
| **[@packkit/provider-netlify](https://github.com/PackkitJS/provider-netlify)** | Netlify deployment provider. Consumes a generated project's deployment contract to plan and apply a Netlify site. |

## How it fits together

`create-packkit` stays a provider-neutral **generation engine**: it resolves
config, generates files deterministically, and describes how to deploy a project
in a provider-neutral **deployment contract**. Separate `@packkit/provider-*`
packages consume that contract and launch it. The dependency direction is
one-way — providers depend on `create-packkit`, never the reverse.

```text
create-packkit  ──►  GeneratedProject.deploymentContract  ──►  @packkit/provider-*
```

Providers keep pure planning (`supports` / `prepare` / `plan`) separate from
side effects: all network access is confined to `apply()`, through a client the
host injects. A provider never sources credentials itself.
