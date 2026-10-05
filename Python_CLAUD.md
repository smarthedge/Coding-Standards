# Project Instructions — Python and FastAPI

## Mission

Build secure, typed, testable, production-grade APIs with clear contracts and predictable failure behavior. Prefer simple functions and explicit dependencies over framework-heavy abstractions.

## Technology baseline

Current baseline verified on 2026-08-03:

- Python: 3.14.x
- FastAPI: 0.141.x
- Pydantic: v2-compatible release selected by the lockfile
- ASGI server: FastAPI CLI/Uvicorn unless the repository uses another supported server
- Dependency management: use the repository's existing tool; prefer `uv` for a new project
- Project metadata and tool configuration: `pyproject.toml`

FastAPI is still versioned `0.x` and may introduce breaking changes. Pin direct dependencies and commit the lockfile; do not depend on an unbounded latest version.

Official references:

- https://www.python.org/downloads/
- https://fastapi.tiangolo.com/release-notes/
- https://fastapi.tiangolo.com/advanced/events/

### Mandatory requirement: 

1. At the beginning of every chat response, you must clearly state the "model name, model size, model type, and its revision version (update date)." This rule applies only to chat responses and not to inline edits.

## How to work in this repository

1. Read `pyproject.toml`, the lockfile, application factory/entry point, configuration, relevant modules, and tests before editing.
2. Follow the repository's package, naming, typing, error, logging, and dependency-injection conventions.
3. Make the smallest coherent change and avoid unrelated formatting or dependency upgrades.
4. Use the existing environment and package manager. Do not add a second lockfile or package workflow.
5. Preserve public API and serialized schema behavior unless a breaking change is requested.
6. Run targeted tests first, then formatting, linting, type checking, the full test suite, and packaging checks when practical.
7. Report assumptions and any checks that could not be run.

## Project structure and boundaries

- Organize by feature/domain when the service is more than a small prototype.
- Keep the application entry point and router registration small.
- Separate HTTP schemas, domain logic, persistence models, and external-service models when their contracts differ.
- Keep route functions thin: parse/validate input, invoke an application service, and translate the result to HTTP.
- Keep business rules independent of FastAPI where practical so they can be tested directly.
- Put infrastructure behind explicit adapters or dependencies.
- Avoid `utils.py`, `helpers.py`, global service locators, and base classes without a specific cohesive purpose.
- Do not add a database, task queue, cache, or repository abstraction unless the requirement needs one.

Suggested shape for a non-trivial service; adapt to existing conventions:

```text
app/
  main.py
  core/
    config.py
    errors.py
    logging.py
  features/
    example/
      router.py
      schemas.py
      service.py
      repository.py
tests/
```

## Python code quality

- Use Python 3.14 syntax and standard-library features when they improve clarity.
- Type all public functions, route functions, dependencies, and non-obvious local structures.
- Prefer precise types, `Protocol`, generics, `TypedDict`, and discriminated unions where appropriate.
- Prefer `X | None`, built-in generic types such as `list[str]`, and `collections.abc` interfaces.
- Use `Any` only at a truly dynamic boundary and contain it immediately. Prefer `unknown`-style validation through Pydantic or explicit narrowing.
- Prefer immutable value objects and frozen dataclasses/models when mutation is not required.
- Use timezone-aware datetimes and UTC internally.
- Use `Decimal` for exact monetary calculations; never binary floating point.
- Catch specific exceptions. Never use a bare `except`, silently swallow errors, or return a default that hides corruption.
- Do not use mutable default arguments.
- Do not perform network, filesystem, database, or environment work at import time.
- Add comments and docstrings for intent, constraints, and public contracts—not for obvious line-by-line narration.

## FastAPI design

- Group endpoints with `APIRouter` and include routers in one composition root.
- Use dependency injection with `Depends`/`Annotated` for request-scoped resources, authentication, authorization, and boundary adapters.
- Do not use dependencies as a hidden global service locator.
- Define explicit Pydantic request and response models and set `response_model` when it protects the public contract.
- Do not return ORM objects, database rows, or arbitrary dictionaries directly as a public contract.
- Use accurate HTTP methods and status codes, and resource-oriented URLs.
- Bound pagination and upload sizes. Stream large responses rather than materializing them in memory.
- Keep OpenAPI operation IDs stable and meaningful when clients generate code from the schema.
- Represent expected conflicts, validation failures, authorization failures, and absence consistently.
- Never leak stack traces, credentials, queries, internal paths, or sensitive record existence in error responses.
- Use a consistent error schema, preferably compatible with RFC 9457 problem details when the clients support it.

## Pydantic v2

- Use Pydantic v2 APIs: `model_validate`, `model_dump`, `ConfigDict`, `field_validator`, and `model_validator`.
- Do not add v1 compatibility imports or deprecated `parse_obj`, `dict`, `json`, `validator`, or `root_validator` APIs.
- Use strict field types and constraints where coercion would be unsafe or surprising.
- Distinguish create, update, response, and persistence schemas when their required fields differ.
- For PATCH-like operations, model omission separately from explicit `null` and update with `exclude_unset=True` only after authorization and validation.
- Keep cross-field business invariants in the domain/application layer when they need external state.
- Use `pydantic-settings` for typed environment configuration. Validate required settings at startup and never include secrets in model representations or logs.

## Async and concurrency

- Use `async def` when the function awaits non-blocking I/O.
- Use ordinary `def` for genuinely synchronous endpoint/dependency work so FastAPI can place it in a thread pool.
- Never call blocking database, HTTP, filesystem, CPU-heavy, or SDK operations directly from the event loop.
- When an unavoidable blocking call must be invoked from async code, isolate it explicitly with AnyIO thread helpers and document capacity implications.
- Do not use `time.sleep()` in application async paths; use `await asyncio.sleep()` or AnyIO equivalents.
- Create one reusable async HTTP client with explicit connect/read/write/pool timeouts and close it during lifespan shutdown.
- Bound concurrency and queues. Do not spawn untracked tasks with `asyncio.create_task()` inside request handlers.
- Use `BackgroundTasks` only for small, non-critical work that may be lost if the process stops. Use a durable external worker/queue for important or long-running jobs.
- Handle cancellation correctly; do not convert cancellation into an ordinary success or retry loop.
- Retry only transient, idempotent operations with bounded backoff and jitter.

## Application lifecycle

- Use FastAPI's `lifespan` async context manager for startup and shutdown resources.
- Do not add deprecated startup/shutdown event handlers to new code.
- Initialize pools and clients once, expose them through explicit dependencies, and close them reliably.
- Make readiness reflect whether the instance can serve traffic; keep liveness independent from temporary downstream outages.
- Avoid network calls during module import or application object construction.

## Persistence when required

- Follow the repository's chosen database stack. Do not introduce an ORM merely by preference.
- For async SQLAlchemy, use the 2.x typed APIs, an async driver, and a request-scoped `AsyncSession` dependency.
- One request may use more than one transaction, but every transaction boundary must be intentional.
- Never share a mutable database session across requests or concurrent tasks.
- Do not hold a transaction open while calling a remote service.
- Bind query parameters; never interpolate untrusted data into SQL.
- Enforce invariants with database constraints as well as application validation.
- Use versioned Alembic migrations for schema changes. Review generated migrations and test upgrade behavior.
- Use the real database engine in integration tests when engine behavior matters.

## Security and privacy

- Deny access by default and centralize authentication parsing.
- Perform object-level authorization in addition to route-level checks.
- Use established OAuth 2.0/OIDC/JWT libraries and validate issuer, audience, algorithm, signature, and time claims. Do not decode tokens without verification.
- Store password hashes with an approved adaptive password-hashing library; never encrypt or hash passwords manually.
- Validate outbound URLs to mitigate SSRF and enforce allowlists where appropriate.
- Constrain content types, filenames, sizes, and storage paths for uploads.
- Never commit or log secrets, bearer tokens, session identifiers, raw passwords, or sensitive personal data.
- Use parameterized queries and safe serialization. Do not use `eval`, `exec`, unsafe pickle loading, or unsafe YAML loading on untrusted data.
- Configure CORS with explicit trusted origins. Do not combine wildcard origins with credentialed requests.
- Treat rate limiting as defense in depth, not authorization.

## Configuration and logging

- Keep configuration typed and environment-driven, with safe defaults only for local development.
- Keep secret values out of exception messages and model serialization.
- Use structured logging with stable event names and request/trace correlation.
- Do not build log messages with sensitive request bodies or authorization headers.
- Log an exception once where useful context is available; avoid duplicate stack traces at every layer.
- Keep metrics labels low-cardinality; never tag by user ID, request ID, raw URL, or exception text.
- Instrument latency, errors, saturation, and critical dependency calls using the repository's existing OpenTelemetry/metrics stack.

## Testing

- Use `pytest` and the repository's async test plugin/configuration.
- Test externally visible behavior and domain rules rather than private implementation details.
- Use `httpx.AsyncClient` with `ASGITransport` for async API tests.
- If tests depend on lifespan resources, run the lifespan explicitly with the repository's established helper.
- Override FastAPI dependencies at real boundaries and always clean up overrides after the test.
- Cover success, validation, authentication, authorization, not-found, conflict, dependency failure, timeout, and cancellation behavior as applicable.
- Use factories/fixtures that make important data visible. Avoid enormous opaque fixtures.
- Avoid live network calls, real secrets, arbitrary sleeps, shared mutable state, and order-dependent tests.
- Add a regression test for every bug fix.

## Tooling and verification

Respect the repository's tools. For a new `uv`-managed project, typical commands are:

```bash
uv sync --frozen
uv run ruff format --check .
uv run ruff check .
uv run mypy app tests
uv run pytest
```

Use Pyright instead of mypy if that is the project standard. Start development with the repository script or, for a conventional app:

```bash
uv run fastapi dev app/main.py
```

Production must use explicit worker/process/container configuration, trusted proxy settings, timeouts, graceful shutdown, and health checks appropriate to the deployment platform. Do not use development reload mode in production.

## Definition of done

- Dependencies are pinned and the lockfile is consistent.
- Formatting, linting, type checking, tests, and packaging/startup checks pass.
- The OpenAPI schema reflects the intended public contract.
- Async request paths contain no accidental blocking or untracked tasks.
- Authentication, authorization, input limits, failure behavior, observability, and privacy were considered.
- Configuration and schema changes are documented and migration-safe.
- The final response concisely lists changed files, key decisions, executed checks, and remaining risks.
