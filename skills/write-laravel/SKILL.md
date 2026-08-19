---
name: write-laravel
description: Use when writing or reviewing code for a Laravel application or package, including tests. Do not use for planning, explaining, or diagnosing unless the task also changes the implementation.
---

# Write Laravel

Inspect the project's own guidance and neighboring code before changing or judging it. Follow established project conventions for architecture, test organization, and verification commands.

## Class Conventions

Apply these defaults when introducing a class or intentionally changing its purpose, structure, or name. Project guidance or a class family used consistently across the relevant area overrides them; one existing class does not establish a convention. Do not widen the task to retrofit unrelated classes.

- **Controllers.** Use invokable controllers named `VerbNounController`, such as `StorePostController`. Avoid noun-first names such as `PostController`.
- **Events.** Tense conveys timing. Use progressive before (`RequestSending`) and past tense after (`Registered`).
- **Facades.** Use singular nouns without a suffix, such as `Inventory` or `Geocoder`.
- **Jobs.** Use an action followed by the `Job` suffix, such as `CreateUserJob` or `PerformDatabaseCleanupJob`.
- **Listeners.** Use an action followed by the `Listener` suffix, such as `SendInvitationMailListener`.
- **Mailables.** Use a noun followed by the `Mail` suffix, such as `OrderConfirmationMail`.
- **Notifications.** Use past tense followed by the `Notification` suffix, such as `EmployeeAccountCreatedNotification`.

For application commands:

- **Commands.** Mirror the `app:` Artisan signature, using kebab case for multiple words, such as `app:inventory:flush-records` → `Inventory\FlushRecordsCommand`.

For package commands:

- **Commands.** Mirror the Artisan signature, using kebab case for multiple words, such as `package:flush-records` → `FlushRecordsCommand`.

## Collections

- Favor Laravel collection methods when they are at least as clear as the equivalent loop.

## Database

- Use Eloquent by default when working with database records. Use the query builder or raw SQL when Eloquent does not express the required query well.
- Let Eloquent infer table names by convention unless there is strong reason to be explicit.
- Prefer query methods on the Eloquent model over repositories that only wrap model calls.

## Configuration

- Access configuration where it is interpreted, with explicit defaults. Avoid wrappers that only relay values.
- In multi-section configuration files, use blank lines within the outer array and between top-level groups or multi-line sibling configurations.
- Use Laravel-style section comments only when a top-level group needs a heading or explanation. Document behavior or constraints that are not evident from the keys and defaults.
- Use inline `env()` calls for common deployment-specific values, with defaults when safe and meaningful. Leave uncommon opt-in values at literal defaults so applications choose how to source them.
- Make every published configuration setting's effective default explicit where it is declared. Express inherited environment defaults as complete nested `env()` fallback chains ending in a literal value.
- For complex configuration blocks, follow how similar Laravel framework or first-party package components read and interpret their configuration.
- Treat configuration as trusted application input. Add validation only when supported behavior requires it, rather than merely to fail earlier or produce a friendlier error.

## Framework Boundaries

- Express form request validation rules as arrays rather than pipe-delimited strings.
- Route requests through controllers rather than inline route callbacks.
- Seed only initial data required for the application to function; keep appending, upserting, and complex logic out of seeders.
- Define a morph map for every polymorphic Eloquent association in a service provider's `boot()` method.
- Follow the project's existing mass-assignment strategy, whether it uses `Model::unguard()`, `$guarded = []`, or a `$fillable` allowlist.
- Regardless of strategy, pass only validated data or explicitly selected attributes to mass-assignment methods.

## Exceptions

- Let exceptions bubble to the framework or host-application boundary by default.
- Catch an exception only when the boundary meaningfully changes behavior through recovery, rollback, retry, fallback, or translation.
- Use `finally` only for guaranteed cleanup of resources such as locks, temporary files, and external handles.
- Design exception contracts around actions callers can take. Related failures may share a type only when callers can handle them alike or distinguish them through structured state; messages are diagnostic, not control flow.
- Translate lower-level exceptions only at a boundary that owns a more stable failure contract, and preserve the original exception as the cause.
- Let exceptions from caller-supplied callbacks and delegated application work propagate unchanged unless the current boundary owns their recovery semantics.
- Choose an exception's base class according to the catch boundary callers need, preferring the closest suitable SPL or framework exception.
- Let domain exception classes own their meaningful messages. Use named constructors when they clarify distinct failure cases.
- In a package, make package-defined exceptions implement a shared marker interface so callers have a package-wide catch boundary independent of base class.

## Database Migrations

Read [references/database-migrations.md](references/database-migrations.md) whenever creating, editing, or reviewing a migration.
