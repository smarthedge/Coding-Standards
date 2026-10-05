# Project Instructions — Java, Spring Boot, Spring Cloud, PostgreSQL, R2DBC

## Mission

Build secure, observable, production-grade reactive services. Prefer clear, boring code over clever abstractions. Preserve the repository's established conventions unless a change is required and justified.

## Technology baseline

- Java: JDK 25
- Spring Boot target: 4.1.x
- Spring Cloud target: 2025.1.2
- Database: PostgreSQL
- Data access: Spring Data R2DBC with `r2dbc-postgresql`
- Reactive runtime: Spring WebFlux and Reactor
- Build: use the existing Maven Wrapper or Gradle Wrapper; do not introduce a second build tool

### Mandatory requirement: 

1. At the beginning of every chat response, you must clearly state the "model name, model size, model type, and its revision version (update date)." This rule applies only to chat responses and not to inline edits.

### Mandatory compatibility guard

As verified on 2026-08-03, Spring Cloud 2025.1.2 officially supports Spring Boot 4.0.7, not Spring Boot 4.1.x.

- If Spring Cloud 2025.1.2 is required, use Spring Boot 4.0.7.
- If Spring Boot 4.1.x is required, do not import Spring Cloud 2025.1.2. Wait for or select an officially compatible Spring Cloud release train.
- Never suppress the Spring Cloud compatibility verifier or force this combination merely to make the build pass.
- Before changing either version, confirm the current compatibility matrix in the official Spring documentation and explain the choice.

Official references:

- https://docs.spring.io/spring-boot/system-requirements.html
- https://docs.spring.io/spring-cloud-release/reference/index.html

## How to work in this repository

1. Read the relevant build file, configuration, nearby production code, and tests before editing.
2. Make the smallest coherent change that satisfies the requirement.
3. Do not upgrade frameworks, plugins, or unrelated dependencies unless explicitly requested.
4. Use dependency management from Spring Boot and the compatible Spring Cloud BOM. Do not add versions to managed dependencies.
5. Preserve public API behavior unless the task explicitly authorizes a breaking change.
6. After editing, run the narrowest relevant tests first, then the full verification task when practical.
7. Report assumptions, compatibility risks, migrations, and any checks that could not be run.

## Architecture and code organization

- Organize by business feature or bounded context, not only by technical layer.
- Keep HTTP DTOs, domain concepts, persistence models, and external-client models distinct when their contracts differ.
- Keep controllers thin: validate and translate HTTP concerns, then delegate to an application service.
- Keep business rules out of controllers, repositories, configuration classes, and entity callbacks.
- Use constructor injection. Do not use field injection.
- Prefer immutable types, records for value/transport types, sealed types where they clarify a closed domain, and exhaustive pattern matching.
- Use interfaces at real boundaries; do not create an interface for every class.
- Avoid generic `util`, `helper`, or `common` dumping grounds.
- Do not introduce a new framework or abstraction when the JDK or Spring already provides a clear solution.

## Reactive programming rules

- Keep the request path non-blocking end to end.
- Never call `block()`, `blockFirst()`, `blockLast()`, or `subscribe()` in application request processing.
- Do not add Spring MVC, JPA, JDBC repositories, blocking HTTP clients, or blocking SDK calls to a reactive path.
- Use `WebClient` for reactive HTTP calls. Do not use `RestTemplate`, `RestClient`, or OpenFeign in a reactive execution path.
- Compose publishers with Reactor operators; return `Mono` or `Flux` to the framework.
- Use `map` for synchronous transformations and `flatMap` only for asynchronous composition.
- Avoid nested reactive chains. Extract named functions when a pipeline becomes difficult to read.
- Set timeouts at remote boundaries. Retry only transient, idempotent operations, with bounded exponential backoff and jitter.
- Never retry validation, authorization, or non-idempotent failures blindly.
- Preserve cancellation and backpressure. Do not collect an unbounded `Flux` into memory.
- Make concurrency explicit and bounded. Do not change schedulers casually.
- If an unavoidable blocking library is already required, isolate it behind a clearly named adapter, run it on `Schedulers.boundedElastic()`, and document the capacity and operational risk.
- Use Reactor Context for request-scoped metadata that must cross reactive boundaries; do not rely on ordinary `ThreadLocal` state.

## PostgreSQL and R2DBC

- Use `spring-boot-starter-data-r2dbc`, `r2dbc-postgresql`, and the Boot-managed connection pool unless the repository has a verified alternative.
- Do not add JDBC for runtime data access. A JDBC driver may be used only for a clearly isolated schema-migration tool when required.
- Use Spring Data repositories for straightforward aggregate access. Use `R2dbcEntityTemplate` or `DatabaseClient` for explicit SQL and complex queries.
- Bind all SQL parameters. Never concatenate untrusted values into SQL.
- Select only required columns and prevent N+1 query patterns.
- Use database constraints for invariants: primary keys, foreign keys, unique constraints, checks, and nullability.
- Prefer `uuid`, `timestamptz`, and appropriate native PostgreSQL types. Use `jsonb` only when the data is genuinely document-shaped and query/index needs are understood.
- Store timestamps in UTC and expose offsets explicitly at API boundaries.
- Add indexes based on actual predicates, joins, ordering, and uniqueness requirements. Explain non-obvious indexes.
- Prefer keyset pagination for large or frequently changing datasets; use offset pagination only when its trade-offs are acceptable.
- Do not hold a database connection or transaction open while calling a remote service.
- Use reactive transactions through `TransactionalOperator` or `@Transactional` on a method that returns a reactive type. Verify that the entire transactional pipeline is assembled inside the boundary.
- Never call `subscribe()` inside a transaction.
- Every schema change requires a versioned, forward migration. Do not depend on automatic schema generation in production.
- Use PostgreSQL/Testcontainers for database integration tests; do not substitute H2 for PostgreSQL behavior.

## HTTP API design

- Use resource-oriented URLs, correct HTTP methods, and accurate status codes.
- Define explicit request and response DTOs. Do not expose persistence objects directly.
- Validate input with Jakarta Bean Validation at the boundary and enforce domain invariants in the domain/application layer.
- Return a consistent error contract, preferably Spring `ProblemDetail` aligned with RFC 9457.
- Do not leak stack traces, SQL, internal class names, credentials, or sensitive record existence through errors.
- Keep API versioning consistent with the existing service. Do not introduce versioning without a concrete compatibility need.
- Document public operations and schemas through the repository's OpenAPI mechanism when one exists.
- Make pagination limits bounded and deterministic.
- Require an explicit idempotency strategy for retried create/payment/workflow commands.

## Spring Cloud usage

- Add Spring Cloud components only for a demonstrated distributed-systems need.
- Prefer platform-native service discovery and configuration when the deployment platform already provides them.
- For Gateway, use the reactive Spring Cloud Gateway stack and keep filters non-blocking.
- Use Spring Cloud LoadBalancer rather than legacy Ribbon.
- Use Spring Cloud CircuitBreaker with bounded timeouts and observable fallback behavior; never hide systemic failure behind an empty fallback.
- Treat distributed configuration as production code: validate it, secure it, version it, and define behavior when it is unavailable.
- Do not add Eureka, Config Server, Bus, Stream, or OpenFeign speculatively.

## Configuration and secrets

- Use typed `@ConfigurationProperties` with validation for application configuration.
- Keep environment-specific values outside the artifact.
- Never commit passwords, tokens, private keys, connection strings with credentials, or real customer data.
- Do not log secrets, authorization headers, session identifiers, or sensitive personal data.
- Fail fast on missing required configuration while allowing explicit, safe local defaults where appropriate.

## Security

- Deny access by default and make authorization rules explicit.
- Prefer OAuth 2.0/OIDC resource-server support for bearer-token APIs.
- Perform object-level authorization when access depends on the requested resource, not only the route.
- Validate outbound URLs and redirects; guard against SSRF and open redirects.
- Use parameter binding and output encoding; do not invent custom cryptography.
- Apply least privilege to application identities and database roles.
- Treat dependency or static-analysis warnings as findings to assess, not messages to disable globally.

## Observability and reliability

- Use structured logging with stable event names and useful, non-sensitive context.
- Propagate trace and correlation context across HTTP and messaging boundaries.
- Use Micrometer metrics and tracing through the repository's existing observability stack.
- Keep metric labels low-cardinality; never use user IDs, request IDs, raw URLs, or exception messages as tags.
- Expose health checks that distinguish liveness from readiness. Do not make liveness depend on remote services.
- Add timeouts, concurrency limits, and backpressure at external boundaries.
- Log an error once at the layer that can add useful context; avoid duplicate stack traces through every layer.

## Testing

- Use JUnit Jupiter, AssertJ, Reactor Test, Spring Boot Test, and Testcontainers as applicable.
- Test behavior and externally visible contracts, not private implementation details.
- Use `StepVerifier` for publisher behavior, including completion and error paths.
- Use focused tests such as `@WebFluxTest` and `@DataR2dbcTest` when they provide value; use full-context tests only for cross-cutting integration.
- Use `WebTestClient` for HTTP tests.
- Cover validation, authorization, absence/not-found behavior, conflicts, timeouts, and database constraints.
- Avoid sleeps, shared mutable fixtures, and order-dependent tests.
- For bug fixes, add a regression test that fails without the fix.

## Build and verification

Use repository wrappers. Typical commands are:

```bash
./mvnw test
./mvnw verify
```

or:

```bash
./gradlew test
./gradlew check
```

Also run any configured formatter, linter, architecture test, static analyzer, and migration validation. Do not claim success for checks that were not executed.

## Definition of done

- The chosen Spring Boot/Spring Cloud versions are officially compatible.
- Production code compiles on JDK 25 without preview features unless preview use is explicitly approved.
- Reactive paths contain no accidental blocking or manual subscriptions.
- Schema and configuration changes are documented and migration-safe.
- Tests cover the change and pass.
- Formatting, static checks, and packaging pass.
- Security, observability, and failure behavior were considered.
- The final response concisely lists changed files, key decisions, executed checks, and remaining risks.
