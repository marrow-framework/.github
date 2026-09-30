<div align="center">

<img src="https://raw.githubusercontent.com/marrow-framework/.github/main/logo.png" alt="Marrow" width="120">

# Marrow

**A modular, HMVC-first PHP framework — explicit dependencies over global resolution, service injection over magic.**

[![CI](https://img.shields.io/github/actions/workflow/status/marrow-framework/core/ci.yml?branch=main&style=flat-square&label=CI)](https://github.com/marrow-framework/core/actions/workflows/ci.yml)
[![PHP 8.2+](https://img.shields.io/badge/PHP-8.2%2B-777BB4?style=flat-square&logo=php&logoColor=white)](https://php.net)
[![License MIT](https://img.shields.io/badge/license-MIT-22c55e?style=flat-square)](https://github.com/marrow-framework/core/blob/main/LICENSE)
[![Version](https://img.shields.io/badge/version-2.3.0-f97316?style=flat-square)](https://github.com/marrow-framework/core/releases)

[Documentation](https://github.com/marrow-framework/core/tree/main/docs) · [Getting started](https://github.com/marrow-framework/core/blob/main/docs/getting-started.md) · [Changelog](https://github.com/marrow-framework/core/blob/main/CHANGELOG.md)

</div>

---

## What Marrow is

Marrow is a PHP 8.2+ framework for building **modular, testable, HMVC applications**. Instead of a single `app/` folder and a global service locator, an application is a set of self-contained **modules** — each with its own routes, views, migrations, and an explicit `imports`/`exports` contract enforced by the container at boot time.

```php
#[Module(
    name: 'blog',
    imports: [AuthModule::class],
    providers: [PostService::class],
    exports: [PostService::class],
)]
class BlogModule extends BaseModule {}
```

A module doesn't even have to live in the app — `BaseModule::path()` resolves its views, routes, and migrations by reflecting on the module class's own file location, which means a module ships just as well from `vendor/` as a plain Composer package. That one mechanism is what turns the framework into an ecosystem: install a package, and its module registers itself.

**Core building blocks:** IoC container with reflection-based resolution, HTTP + console kernels, a fluent router (groups, named routes, `#[Route]` attributes), a model-oriented ORM (relationships, migrations, soft deletes, eager loading), auth & a typed security middleware pipeline, Twig, events/jobs/queues/scheduling, and pluggable `/health` checks.

## Get started

```bash
composer create-project marrow/skeleton my-app
cd my-app
cp .env.example .env && php forge key:generate
php forge migrate --seed
php forge serve --watch-css
```

## Repositories

| Repository | What it is |
|---|---|
| **[core](https://github.com/marrow-framework/core)** | The framework itself — container, modules, router, ORM, auth, CLI (`forge`). Distributed as `marrow/framework`. |
| **[skeleton](https://github.com/marrow-framework/skeleton)** | The starter application project — `composer create-project marrow/skeleton` and you have a running app. |
| **[form-builder](https://github.com/marrow-framework/form-builder)** | Django-style declarative forms: define fields as attributes on the backend, validate with Marrow's own rule engine, render semantic HTML — no fluent HTML builder, no duplicated frontend rules. |
| **[anvil](https://github.com/marrow-framework/anvil)** | Local Docker Compose dev environment — Laravel Sail's role, under Marrow's own name. `php forge anvil:install --services=mysql,redis,node`. |
| **[compass](https://github.com/marrow-framework/compass)** | Generates `AGENTS.md` — a live map of an app's modules, routes, and config, plus Marrow-specific gotchas, for AI coding agents and new contributors. |

Every companion package is an independent, self-documented Composer package (own `README`/`LICENSE`) that registers itself automatically on `composer require` via package auto-discovery — no manual step in `config/modules.php`.

```bash
composer require marrow/form-builder
composer require --dev marrow/anvil marrow/compass
```

## Design principles

- **Explicit over implicit.** Module dependencies are declared (`imports`/`exports`) and validated at boot — missing dependencies and dependency cycles fail loudly, not silently.
- **Injection over global state.** Services are resolved through the container and constructor-injected; no facades, no service locator reached for from anywhere.
- **Convention where it helps, none where it doesn't.** There's deliberately no `app/` directory in the skeleton — a domain concern like authentication lives in its own `Account` module, not a generic bucket.
- **Package ecosystem as a first-class idea, not an afterthought.** The same reflection-based path resolution that lets a local module find its own views is what lets a `vendor/`-installed package register a full module — forms, dev tooling, and AI-agent context are all shipped this way rather than bolted onto the core.

## Contributing

Each repository documents its own contribution workflow. In general: PHP 8.2+, strict typing, PSR-12, tests for functional changes (`composer test`), static analysis (`composer analyse`), and a clear PR description. See [core's CONTRIBUTING.md](https://github.com/marrow-framework/core/blob/main/CONTRIBUTING.md) for the full guide.

## License

All Marrow repositories are MIT licensed.
