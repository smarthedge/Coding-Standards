# Project Instructions — Angular and PrimeNG

## Mission

Build accessible, fast, maintainable Angular applications with a consistent PrimeNG design system. Prefer Angular's current first-party APIs and simple local state over unnecessary frameworks or abstractions.

## Technology baseline

Current baseline verified on 2026-08-03:

- Angular and Angular CLI: 22.x
- PrimeNG: 22.x
- Node.js: 24.15.0 or newer within the Angular-supported Node 24 range
- TypeScript: `>=6.0.0 <6.1.0`
- RxJS: 7.4 or newer within the Angular-supported 7.x range
- Package manager: use the repository's existing lockfile and package manager

Official references:

- https://angular.dev/reference/versions
- https://primeng.dev/installation

Do not silently upgrade major versions. For existing repositories, treat `package.json` and the lockfile as authoritative, verify peer dependencies, and use the Angular update guide for migrations.

### Mandatory requirement: 

1. At the beginning of every chat response, you must clearly state the "model name, model size, model type, and its revision version (update date)." This rule applies only to chat responses and not to inline edits.

## How to work in this repository

1. Read `package.json`, `angular.json`, TypeScript configs, application configuration, nearby components, and tests before editing.
2. Follow the repository's naming, folder, styling, state-management, and testing conventions.
3. Make the smallest coherent change and avoid unrelated reformatting or dependency churn.
4. Use the existing package manager and committed lockfile. Never mix npm, pnpm, yarn, or Bun lockfiles.
5. Preserve public component inputs, outputs, routes, and API contracts unless a breaking change is requested.
6. Run targeted tests first, then lint, the full test suite, and a production build when practical.
7. Report assumptions and any checks that could not be run.

## Angular architecture

- Use standalone components, directives, and pipes. Do not add NgModules unless integrating a legacy boundary that requires them.
- Organize by feature. Keep feature pages, feature UI, data access, models, and utilities close to the feature that owns them.
- Keep route-level components focused on orchestration. Extract reusable presentational components when they have a clear contract.
- Use lazy-loaded routes for substantial features.
- Prefer dependency injection with `inject()` where it improves clarity and matches surrounding code.
- Use `ChangeDetectionStrategy.OnPush` when the component is not already operating in the current zoneless default/convention; do not add Zone.js merely to solve local design problems.
- Do not introduce a global state library for local or URL-derived state. Use a store only when state is genuinely shared and complex, and follow the repository's existing choice.
- Keep domain decisions outside generic UI components.
- Avoid `SharedModule`, `CommonModule` aggregation, barrel files that create cycles, and generic `utils` dumping grounds.

## Components and templates

- Prefer signals for local and derived state: `signal`, `computed`, and read-only exposed signals.
- Use `effect` only for imperative side effects, not for state derivation. Avoid effects that write to other signals unless the lifecycle is explicit and justified.
- Use signal-based `input`, `output`, `model`, and query APIs when consistent with the repository.
- Use built-in control flow: `@if`, `@for`, `@switch`, and `@defer` where appropriate.
- Every `@for` must use a stable, meaningful `track` expression.
- Keep templates declarative. Do not call expensive or state-mutating methods during rendering.
- Prefer class and style bindings to imperative DOM manipulation.
- Use `async` pipe or signal interop instead of manual subscriptions in components.
- If a manual subscription is unavoidable, use `takeUntilDestroyed()` and explain why.
- Do not use `setTimeout` to paper over change-detection or lifecycle issues.
- Keep components small enough that state, events, and accessibility remain understandable.

## PrimeNG rules

- Import each PrimeNG component directly and only where used to preserve tree shaking.
- Configure PrimeNG once with `providePrimeNG` in application configuration.
- Use the current styled-mode preset and design-token system. Do not add legacy `primeng/resources` theme CSS.
- Prefer PrimeNG component APIs, templates, design tokens, and Pass Through APIs over brittle selectors or DOM queries.
- Do not use `::ng-deep` except as a documented, temporary last resort for a verified library limitation.
- Centralize intentional theme customizations. Do not scatter hard-coded colors, spacing, or z-index values across components.
- Use semantic tokens rather than coupling application styles to one preset such as Aura.
- Verify behavior in light/dark modes if the application supports both.
- Preserve keyboard interaction, focus visibility, labels, descriptions, validation associations, and screen-reader behavior.
- PrimeNG is not a substitute for semantic structure; use the correct heading, landmark, button, link, table, and form semantics.
- For large tables, explicitly design pagination/virtual scrolling, loading, empty, error, and responsive states.
- Confirm a component's current v22 API before using inputs or outputs remembered from older PrimeNG versions.

## State and RxJS

- Use signals for synchronous UI state and `computed` for derivation.
- Use RxJS for asynchronous streams, cancellation, event composition, and HTTP flows.
- Convert between signals and observables only at clear boundaries; avoid repeated back-and-forth conversion.
- Never nest `subscribe` calls.
- Choose flattening operators deliberately: `switchMap` for replaceable work, `concatMap` for ordered work, `exhaustMap` for ignored re-entry, and `mergeMap` for intentional concurrency.
- Handle errors at the layer that can recover or present them meaningfully. Do not silently convert failures to empty data.
- Avoid `shareReplay` without understanding cache lifetime, invalidation, and ref-count behavior.
- Keep subscriptions finite or lifecycle-bound.

## Forms

- Use strictly typed reactive forms for non-trivial forms.
- Keep validation rules explicit and reusable where they represent shared business rules.
- Show validation messages accessibly and at a useful time; do not rely on color alone.
- Distinguish create, edit, and API response types when their optionality differs.
- Map form values to API commands explicitly rather than sending raw form objects blindly.
- Prevent duplicate submissions and make pending/success/error states visible.
- Do not mutate input objects to make a form work.

## Data access and API contracts

- Keep HTTP access in dedicated data-access services or generated clients, not directly in presentational components.
- Use typed request and response models; avoid `any` and unchecked casts.
- Prefer `unknown` plus validation/narrowing at untrusted boundaries.
- Use functional interceptors for cross-cutting HTTP behavior when consistent with the repository.
- Do not hide feature-specific behavior in a global interceptor.
- Preserve server error detail needed for user-safe messages and diagnostics without exposing sensitive information.
- Represent loading, empty, success, stale, and error states explicitly.
- Use route parameters and query parameters for shareable navigation state.

## TypeScript quality

- Keep strict compiler settings enabled. Do not weaken strictness to make a change compile.
- Prefer precise types, discriminated unions, readonly data, and exhaustive switches.
- Avoid enums when a literal union or `as const` object is clearer and interoperable.
- Do not use non-null assertions unless the invariant is locally obvious and cannot be encoded in the type.
- Do not use type assertions to bypass a design problem.
- Use descriptive names; avoid unexplained abbreviations and boolean parameters.
- Add comments for intent and constraints, not narration of obvious code.

## Accessibility and UX

- Target WCAG 2.2 AA.
- All interactive behavior must be keyboard operable with logical focus order.
- Provide visible focus indicators and restore/move focus intentionally after dialogs, navigation, and destructive actions.
- Give form controls programmatic labels and associate errors/help text.
- Use live regions sparingly for important dynamic status.
- Confirm color contrast and do not communicate status through color alone.
- Respect reduced-motion preferences.
- Provide responsive behavior at narrow widths, zoom, and large text sizes.
- Use skeletons or progress indicators only when they improve clarity; avoid layout shift.

## Security

- Treat all server and user-provided content as untrusted.
- Do not bypass Angular sanitization or use `innerHTML` unless content is sanitized by a reviewed policy and the need is documented.
- Never place secrets in frontend source, environment files shipped to browsers, or logs.
- Do not store sensitive bearer tokens in `localStorage` unless the existing architecture explicitly accepts and mitigates that risk.
- Enforce authorization on the server; route guards are user-experience controls, not security boundaries.
- Protect external links and redirects against unsafe destinations and opener attacks.
- Do not log personal, authentication, or payment data.

## Performance

- Lazy-load meaningful route boundaries and use `@defer` only where loading behavior remains predictable and accessible.
- Keep initial bundles within configured budgets.
- Avoid importing entire icon, utility, date, or component libraries for one feature.
- Use responsive images and explicit dimensions.
- Virtualize or paginate large collections; never render an unbounded data set.
- Measure before adding memoization or complex caching.
- Keep high-frequency events throttled/debounced only where product behavior supports it.

## Testing

- Use the repository's configured Angular test runner; do not introduce a second runner.
- Test user-visible behavior, accessibility, inputs/outputs, routing, forms, and error states rather than private methods.
- Use Angular testing utilities and component harnesses where available.
- Mock at real boundaries such as HTTP or browser APIs, not every collaborator.
- Use HTTP testing providers for request assertions.
- Add regression coverage for bug fixes.
- Keep tests deterministic; avoid arbitrary sleeps and brittle selectors based on internal PrimeNG markup.
- Add or update end-to-end coverage for critical user journeys when the repository has an E2E suite.

## Build and verification

Read `package.json` and use its scripts. Typical commands are:

```bash
npm ci
npm run lint
npm test -- --watch=false
npm run build
```

Substitute the repository's package manager and script names. Do not run an install that rewrites the lockfile unless dependency changes are part of the task.

## Definition of done

- Code uses Angular 22 and PrimeNG 22 APIs compatible with the locked dependency versions.
- Strict TypeScript compilation, linting, tests, and production build pass.
- New UI covers loading, empty, error, responsive, keyboard, and screen-reader behavior as applicable.
- No manual-subscription leak, unsafe HTML bypass, secret, or unnecessary dependency was introduced.
- PrimeNG customization uses supported APIs and design tokens.
- The final response concisely lists changed files, key decisions, executed checks, and remaining risks.
