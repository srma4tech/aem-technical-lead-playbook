# Enterprise Java Engineering: Design, Concurrency, and Resilience

## Purpose and audience

Enterprise Java engineering builds software that remains understandable, correct, secure, and operable as codebases, teams, traffic, and dependencies grow. The best design is not the one with the most abstractions; it is the simplest design that preserves required behavior and quality attributes under realistic conditions.

This chapter is for senior Java engineers and technical leads, including developers building AEM services and integrations. Verify the language level, APIs, and runtime constraints supported by the target project.

## Learning outcomes

After completing this chapter, you should be able to:

- design cohesive services with explicit contracts and controlled dependencies;
- choose appropriate collections and concurrency primitives;
- reason about asynchronous work, thread pools, timeouts, and cancellation;
- handle errors without hiding failures or leaking sensitive details;
- make integrations resilient to slow or unavailable dependencies;
- test behavior and failure modes at the right level;
- connect Java implementation choices to AEM runtime and operational constraints.

## 1. Start with behavior and boundaries

Before choosing a pattern, define the behavior the system must guarantee. Identify the business operation, data ownership, expected load, failure modes, security boundary, and operational response.

### Cohesion and coupling

A class or service should have a clear reason to change. Keep business rules separate from transport concerns, persistence details, and vendor-specific API shapes where the separation makes the system easier to test or evolve.

Ask whether the class owns one coherent policy, whether external API types leak through the codebase, whether business behavior can be tested without starting the full platform, and whether dependencies are explicit and replaceable.

Avoid both extremes: a large service that owns every concern and an over-engineered network of tiny wrappers with no meaningful contract.

### Dependency direction

A useful default is for business policy to depend on interfaces that describe required capabilities, while adapters implement those interfaces for HTTP, persistence, or platform APIs. This can make tests and future replacements easier. Do not introduce interfaces automatically for every class; introduce a boundary when it reduces meaningful coupling or enables a required substitution.

## 2. Java collections and data modeling

Select data structures based on the operations and correctness guarantees the application needs.

- Use List when order and duplicates matter.
- Use Set when uniqueness is part of the contract.
- Use Map for keyed lookup and explicit key/value relationships.
- Choose immutable values for data that should not change after construction.
- Document whether iteration order is meaningful.
- Avoid exposing mutable internal collections directly.
- Use appropriate equality and hashing for domain keys.

A linear scan may be simpler and faster for a small collection. A map may be better for repeated keyed lookups. Analyze expected data size and operation frequency rather than optimizing based on the collection's name alone.

For untrusted or unexpectedly large inputs, consider memory growth, maximum payload sizes, and algorithmic complexity. A nested loop or unbounded accumulation can become a production availability issue.

### Null and optional values

Avoid ambiguous null semantics. Define whether a missing value is invalid, optional, unknown, or intentionally empty. Optional is useful for expressing a possibly absent return value in many APIs, but it is not a universal replacement for null and is generally not needed for every field or method parameter.

Validate required values at boundaries and fail with clear, actionable errors.

## 3. Exceptions and error contracts

Exceptions should preserve the distinction between an expected business outcome and an unexpected failure.

- Use validation errors for invalid caller input.
- Use explicit result types or domain exceptions when a business operation has meaningful alternative outcomes.
- Preserve the original cause when translating an exception at a boundary.
- Do not catch broad exceptions just to log and continue.
- Avoid logging the same failure at every layer.
- Do not return success-shaped data after a failed operation unless that fallback is explicit.
- Keep stack traces, credentials, and sensitive payloads out of client-visible errors.

An HTTP adapter may translate a downstream timeout into an integration-specific exception. The application layer can then decide whether to retry, degrade gracefully, or fail the request. Keep the cause and useful context while preventing transport details from becoming the entire domain contract.

A useful error should answer: what operation failed, what safe context identifies it, whether retry is appropriate, and what the caller or operator should do next.

## 4. Concurrency fundamentals

Concurrency can improve throughput or responsiveness, but introduces coordination, lifecycle, and resource-management costs. First determine whether the task is CPU-bound, I/O-bound, latency-sensitive, or naturally sequential.

### Thread safety and shared state

A class is not thread-safe merely because it uses a concurrent collection. Multiple operations can still form a race when their combined effect must be atomic.

- Prefer immutable objects and local variables where possible.
- Minimize shared mutable state.
- Define the synchronization boundary explicitly.
- Use atomic operations for simple state transitions.
- Use locks only when the invariant requires coordinated access.
- Avoid holding locks while performing network or other slow I/O.
- Document lifecycle assumptions for caches and shared registries.

Concurrent collections protect their documented operations, not arbitrary multi-step business invariants. A thread-safe map does not make a check, remote call, and insert sequence atomic.

### ExecutorService and bounded work

Use an executor whose lifecycle and resource limits are explicit. Unbounded thread creation or unbounded task queues can turn a dependency slowdown into memory exhaustion and request pileups.

- Define concurrency limits based on workload and downstream capacity.
- Choose queue behavior deliberately.
- Handle task rejection as an operational signal.
- Use named threads and appropriate uncaught-exception handling where relevant.
- Shut down executors that the application owns.
- Do not create a new pool for every request.
- Avoid blocking a constrained worker while waiting for work scheduled onto the same exhausted pool.

In managed platforms such as AEM, prefer platform-supported execution mechanisms and respect the runtime's thread and lifecycle model. Do not introduce unmanaged background threads without verifying platform constraints and cleanup behavior.

### CompletableFuture and asynchronous composition

CompletableFuture can compose independent asynchronous operations, but it does not make blocking work non-blocking by itself.

- Use asynchronous APIs where available.
- Choose the executor for blocking work rather than accidentally consuming a shared common pool.
- Apply timeouts and define cancellation behavior.
- Handle exceptional completion explicitly.
- Compose stages with clear names and boundaries.
- Do not join or block on a future from a thread whose capacity is needed to complete that future.
- Propagate request context deliberately; do not assume thread-local context automatically crosses executor boundaries.

Parallelize only work that is genuinely independent and whose latency and resource cost justify the added complexity.

## 5. Timeouts, retries, and resilience

Every remote call needs a defined time budget. Without a timeout, a dependency can hold application resources indefinitely. A timeout is not merely a client-library setting; it should fit within the end-to-end latency budget.

If an inbound request has a total budget of two seconds, do not allow three sequential downstream calls to each wait for two seconds. Allocate time across operations and leave room for application work and response delivery.

Use separate connect and response/read timeouts where supported. Bound response sizes and parsing work as well.

### Retry carefully

Retries can improve success rates for transient failures, but they also amplify load and duplicate side effects.

- Retry only failures likely to be transient.
- Use bounded attempts and exponential backoff with jitter where appropriate.
- Respect downstream retry guidance and rate limits.
- Avoid retrying non-idempotent mutations unless an idempotency mechanism exists.
- Ensure total retry time fits the caller's budget.
- Record retry counts and exhaustion.
- Avoid multiple layers independently retrying the same request without a shared budget.

A circuit breaker or concurrency limiter may help protect a failing dependency, but it needs observable states, safe fallback behavior, and recovery tests. Do not add resilience patterns mechanically when a timeout and clear error contract are enough.

### Graceful degradation

A fallback is valid only when it preserves an acceptable business meaning. Returning stale catalog data may be acceptable for a browse page; fabricating a payment authorization is not. Define fallback behavior with product stakeholders and document freshness and user impact.

## 6. API and integration design

An integration adapter should make the external system's contract explicit and keep vendor-specific details from spreading through the application.

- Define request and response schemas.
- Validate remote responses as untrusted input.
- Set connection, read, and overall timeouts.
- Bound payload size and pagination.
- Use TLS and approved authentication mechanisms.
- Redact secrets and sensitive data from logs.
- Include correlation identifiers where supported.
- Map remote errors to stable internal error categories.
- Make retries and idempotency explicit.
- Define versioning and compatibility expectations.

Avoid treating HTTP 200 as proof that the business operation succeeded; validate the response contract and domain outcome. Similarly, do not assume every non-200 response is safely retryable.

An operation is idempotent when repeating it produces the same intended effect as performing it once. Idempotency matters for retries, webhook delivery, asynchronous jobs, and uncertain network outcomes. For a mutation, consider an idempotency key or durable operation identifier, deduplication window, conflict response, and retention policy.

## 7. Observability and production diagnosis

Logs alone are not a complete observability strategy. A production service needs signals that help answer whether the operation is healthy, where time is spent, and which dependency or change explains a regression.

Useful signals include:

- **Latency:** percentiles such as p50, p95, and p99 where volume supports them.
- **Traffic:** requests or operations per time interval.
- **Errors:** distinguish validation, authorization, dependency, timeout, and internal failures.
- **Saturation:** queue depth, pool usage, rejected tasks, memory pressure, and connection usage where observable.
- **Dependency health:** duration, error rate, retry count, and timeout count.
- **Release context:** deployed version, change identifier, environment, and rollout time.

Use structured logs with stable fields and correlation identifiers. Do not put secrets, access tokens, or unnecessary personal data into logs. Keep metric labels bounded; user IDs, raw URLs, and arbitrary request values can create high-cardinality metrics and data exposure.

For a slow operation, identify the affected operation and time window, separate application time from downstream waiting, inspect percentiles and errors, look for queueing or repeated work, correlate symptoms with a release or dependency event, test a hypothesis, and measure the result. Avoid speculative JVM or thread-count tuning before identifying the bottleneck, especially when the runtime is managed.

## 8. Testing strategy

Use multiple test levels because no single test style provides sufficient confidence.

- **Unit tests:** business rules, mapping, validation, state transitions, and error translation.
- **Component tests:** wiring, serialization, adapters, and meaningful collaboration.
- **Integration tests:** real protocols, authentication, schemas, persistence, and dependency behavior in a controlled environment.
- **Contract tests:** compatibility expectations between consumers and providers.
- **Performance tests:** representative concurrency, data volume, tail latency, and dependency conditions.
- **Failure tests:** timeouts, malformed responses, rate limits, partial failure, duplicate delivery, and recovery.
- **Security tests:** authorization boundaries, input handling, and sensitive-data exposure.

Prefer deterministic tests with controlled clocks, injected dependencies, and bounded waiting. Avoid tests that pass only because of arbitrary sleeps or depend on unspecified thread scheduling.

For asynchronous code, test completion, exception propagation, timeout behavior, cancellation, and resource cleanup. Ensure tests do not leave executors or background tasks running after completion.

## 9. Java engineering in AEM

AEM applications use Java inside a managed application framework. General Java principles apply, but lifecycle, dependency injection, repository sessions, and platform-supported execution mechanisms matter.

- Use OSGi Declarative Services for supported service lifecycle and dependency wiring.
- Keep request-specific state out of singleton service fields.
- Do not retain request-bound ResourceResolver, session, or request objects beyond their valid lifecycle.
- Close resolvers and other resources when the code owns them; use the correct service-user or request-bound identity for repository access.
- Use least-privilege service users and subservice mappings for background/system operations.
- Bound repository queries and avoid traversal-heavy request paths.
- Configure outbound HTTP clients with timeouts, bounded connections, and clear lifecycle ownership.
- Use platform-supported job or scheduler mechanisms rather than assuming a custom thread is appropriate.
- Keep environment-specific configuration outside hard-coded constants and secrets out of OSGi configuration committed to source.
- Verify Java API availability and supported behavior for the target AEM version and runtime.

For request-bound repository work, prefer the identity associated with the request when appropriate. For system operations, use a dedicated service identity with only the required permissions. Do not use an administrative resolver as a shortcut.

## 10. Worked scenario: a slow downstream service

A product page calls an external pricing API synchronously. During a downstream slowdown, request latency rises, worker threads accumulate, and unrelated pages begin timing out.

A structured response:

1. Measure end-to-end latency and identify time spent waiting on pricing.
2. Confirm whether connection and response timeouts are configured and fit the page's latency budget.
3. Check concurrency, connection-pool usage, queue depth, and retry behavior.
4. Determine whether multiple application layers are retrying the same call.
5. Decide with product stakeholders whether a safe cached or degraded response is acceptable.
6. Apply bounded timeouts and a limited retry policy only for appropriate transient failures.
7. Add concurrency protection or circuit breaking if the dependency can otherwise consume excessive resources.
8. Test slow responses, connection failures, rate limiting, malformed payloads, and recovery.
9. Monitor tail latency, error rates, retries, and queue saturation after deployment.
10. Document the fallback contract and operational owner.

The root issue is not solved merely by adding more threads. The goal is to prevent one dependency from consuming the resources required to keep the wider application healthy.

## 11. Design review checklist

- [ ] The service has a clear responsibility and explicit dependencies.
- [ ] Data structures and mutability match the required semantics.
- [ ] Errors distinguish caller mistakes, business outcomes, and unexpected failures.
- [ ] Shared state and concurrency invariants are documented.
- [ ] Thread pools and queues are bounded and have a clear lifecycle.
- [ ] Asynchronous work has explicit executor, timeout, cancellation, and error behavior.
- [ ] External calls have timeouts, bounded payloads, and appropriate retry/idempotency behavior.
- [ ] Logs and metrics are actionable, bounded, and free of secrets.
- [ ] Unit, integration, contract, failure, and performance tests cover relevant risks.
- [ ] AEM-specific lifecycle, repository access, service identity, and runtime constraints are respected.
- [ ] Operational ownership, alerts, and recovery behavior are defined.

## Authoritative references

- [Java SE documentation](https://docs.oracle.com/en/java/javase/)
- [Java concurrency tutorial](https://docs.oracle.com/javase/tutorial/essential/concurrency/)
- [Java ExecutorService API](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/ExecutorService.html)
- [Java CompletableFuture API](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/CompletableFuture.html)
- [AEM as a Cloud Service development guidelines](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines)
- [AEM 6.5 JCR integration best practices](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/bestpractices/jcr-integration)
- [AEM security development guidance](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/introduction/security)

## Completion criteria

This chapter is ready to be marked published only after technical/editorial review, link validation, strict documentation build, required CI checks, and live-site verification. Repository presence and a successful build alone are not release evidence.
