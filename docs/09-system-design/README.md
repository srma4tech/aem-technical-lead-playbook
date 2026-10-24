# System Design: Requirements, Architecture, and Trade-offs

## Purpose and audience

System design turns business needs into an architecture that can meet its functional requirements and quality attributes under real operating conditions. It is not a contest to name the most technologies. A strong design explains what must happen, what can fail, which constraints matter, and why the chosen boundaries are appropriate.

This chapter is for senior engineers, technical leads, and engineers preparing for solution-architecture responsibilities. Examples use enterprise web platforms and AEM integrations, but the method applies to many distributed systems.

## Learning outcomes

By the end of this chapter, you should be able to:

- clarify functional requirements, non-functional requirements, assumptions, and constraints;
- estimate traffic, storage, concurrency, and latency at a useful level;
- define service boundaries and data ownership;
- compare synchronous, asynchronous, cache, and persistence choices;
- design for failure, security, observability, and operational ownership;
- communicate trade-offs with diagrams, decision records, and explicit acceptance criteria.

## 1. Start with the problem statement

A vague requirement such as “make the website scalable” is not yet a design requirement. Ask what user or business outcome must improve, under which workload, and how success will be measured.

### Functional requirements

Describe the observable behaviors the system must provide. Prefer concrete statements such as:

- A visitor can browse a product page and see current-enough price and availability.
- An authorized editor can publish content, and public delivery reflects it within an agreed propagation window.
- A downstream timeout does not cause unrelated pages to exhaust their request capacity.
- An integration event can be safely processed more than once without duplicating a business action.

Keep the initial list short and prioritize the critical user journeys. Requirements that conflict should be made visible instead of silently resolved by the architect.

### Non-functional requirements

Quality attributes need measurable targets or at least explicit decision criteria.

- **Latency:** target response times and tail-latency expectations.
- **Throughput:** requests or events per second, including peaks.
- **Availability:** acceptable outage window and failure scope.
- **Durability:** which data must not be lost.
- **Consistency:** how quickly readers must see a write and whether stale reads are acceptable.
- **Security and privacy:** identities, data classes, trust boundaries, and retention.
- **Recoverability:** recovery time and recovery point expectations.
- **Operability:** metrics, alerts, support ownership, and safe rollback.
- **Cost:** infrastructure, licensing, data transfer, and ongoing maintenance.

Do not promise a numerical service level without knowing the dependencies, operating model, and measurement boundary.

### Constraints and assumptions

Record platform versions, supported APIs, data residency, existing contracts, delivery deadlines, team skills, budget, and organizational policies. Separate verified facts from assumptions. Give each high-impact assumption an owner and a way to validate it.

## 2. Establish the workload model

A design should be plausible for the expected workload, not only for a happy-path demo.

Estimate:

- normal and peak request rates;
- burst duration and likely causes;
- read/write ratio;
- payload sizes and response sizes;
- number of users, tenants, locales, and content items;
- background-job volume;
- external dependency limits;
- data growth and retention period.

Use simple estimates to reveal order-of-magnitude concerns. For example, if an endpoint receives 300 requests per second and average service time is 200 ms, Little's Law suggests roughly 60 requests are in service on average under steady-state assumptions. This is not a thread-pool sizing formula by itself; real concurrency also depends on queueing, tail latency, blocking, and resource limits.

Avoid false precision. Record the assumptions and test the design with representative load.

## 3. Draw the system boundary before the boxes

Start with users, external actors, and the system's responsibility. Then identify the major capabilities and the contracts between them.

A typical enterprise web flow might include:

1. Browser or client.
2. CDN or edge delivery.
3. Dispatcher or reverse proxy.
4. AEM publish tier and application services.
5. Repository or other durable data store.
6. External APIs such as pricing, inventory, identity, or payment.
7. Telemetry and operational tooling.

Do not assume every box must become a separate microservice. A box can be a logical module inside one deployable unit. A network boundary introduces latency, partial failure, authentication, versioning, deployment coordination, and operational burden.

### Define responsibility and ownership

For each component, write down:

- the capability it owns;
- the data it is authoritative for;
- the APIs or events it exposes;
- the identities allowed to call it;
- its failure behavior and timeout budget;
- its scaling and operational owner.

If two services both believe they own the same business fact, consistency and update behavior will become difficult. Prefer clear ownership with explicit read models or replicated projections where needed.

## 4. Choose communication patterns deliberately

### Synchronous request/response

Use synchronous calls when the caller needs the result immediately and the dependency can meet the latency and availability budget.

Benefits include a straightforward request flow and immediate feedback. Costs include runtime coupling: a slow dependency adds latency and can consume caller resources.

Set timeouts, bound response sizes, validate the response contract, and define which failures can be retried. Propagate a correlation identifier across the call chain where supported.

### Asynchronous messaging or jobs

Use asynchronous processing when work can complete later, needs buffering, or should be isolated from the caller's request lifecycle.

Benefits include decoupling in time and the ability to absorb bursts. Costs include eventual consistency, duplicate delivery, ordering questions, dead-letter handling, schema evolution, and more complex debugging.

Design for at-least-once delivery unless the platform contract explicitly guarantees otherwise. Make handlers idempotent, define retry and poison-message behavior, and provide a way to replay or reconcile failed work.

### Events are not a universal fix

Do not convert a simple operation into a distributed workflow just to reduce direct dependencies. Events are most useful when the business process genuinely benefits from independent consumers, delayed work, or durable buffering.

## 5. Data, consistency, and caching

### Choose an authoritative source

For each business entity, identify the authoritative writer. Decide whether other components read the source directly, call an API, or use a derived read model. Avoid uncontrolled dual writes to multiple systems without a reconciliation strategy.

### Consistency trade-offs

A system may tolerate slightly stale product descriptions but not a stale payment authorization result. State the required freshness per data type rather than choosing “strong consistency” or “eventual consistency” for the whole platform.

When asynchronous propagation is used, define the expected lag, how lag is measured, and what happens when the lag exceeds the target.

### Cache intentionally

Caching can reduce origin load and improve latency, but it changes freshness and invalidation behavior.

For each cache, document:

- what is cached and under which key;
- whether the response is public or user-specific;
- freshness or TTL expectations;
- invalidation and purge triggers;
- behavior during origin failure;
- how hit rate, stale responses, and origin load are observed.

Never let a shared cache mix personalized or authorization-sensitive responses across users. A cache key that omits a meaningful variation dimension can become a data-exposure vulnerability.

In AEM, reason about browser, CDN, Dispatcher, and application-level caches as separate layers. A purge at one layer does not prove every downstream cache has refreshed.

## 6. Resilience and failure isolation

Distributed systems fail partially. A dependency may be slow rather than completely unavailable; DNS, TLS, authentication, rate limits, and schema changes can fail independently.

Use a failure matrix to make behavior explicit:

- **Dependency timeout:** Is there a bounded fallback, or should the request fail?
- **Rate limit:** Should the caller queue, back off, or return a controlled response?
- **Invalid response:** Reject and alert, or can a safe partial result be used?
- **Duplicate event:** How is idempotency enforced?
- **Partial write:** What is the compensation or reconciliation path?
- **Cache purge failure:** How is stale content detected and corrected?
- **Region or service outage:** What recovery objective and failover model apply?

Use timeouts, bounded retries, backoff with jitter, concurrency limits, and circuit breakers where they address an evidenced failure mode. Retries can amplify an outage. Avoid retrying non-idempotent operations without a reliable deduplication mechanism.

Define graceful degradation with product stakeholders. A stale catalogue may be acceptable; inventing a successful payment or permission decision is not.

## 7. Security is part of the architecture

Identify trust boundaries and data flows before implementation. For every boundary, ask who authenticates, what authorization is checked, how secrets are managed, and what input is trusted.

- Apply least privilege to service identities and repository access.
- Validate and normalize untrusted input at the boundary.
- Use TLS and approved authentication for service-to-service calls.
- Keep credentials out of source control, logs, URLs, and error messages.
- Separate public cached content from personalized or confidential data.
- Define data retention, audit requirements, and incident response.
- Review dependency and supply-chain risks.
- Test unauthorized access as well as authorized happy paths.

In AEM, include Dispatcher filters, publish-tier exposure, Sling servlet registration, service-user mappings, repository ACLs, and cloud configuration boundaries in the threat model.

## 8. Capacity, scaling, and overload control

Scale the constrained resource, not the most visible component. CPU, heap, repository query cost, connection pools, downstream quotas, and queue depth can each become the bottleneck.

Ask:

- Can work be cached or eliminated before adding capacity?
- Is the workload CPU-bound or waiting on I/O?
- Are concurrency and queues bounded?
- Can one tenant or dependency monopolize shared resources?
- Does horizontal scaling increase pressure on a shared database or external API?
- What signal triggers scale-out, and how quickly can it respond?
- What happens when the maximum capacity is reached?

overload control means the system has an explicit response when it cannot safely accept more work. Options include rejecting requests early, queueing within a limit, applying per-client quotas, shedding noncritical work, or returning a controlled retryable response. An unbounded queue is not resilience; it often postpones failure until resources are exhausted.

## 9. Observability and operability

A design is incomplete if operators cannot tell whether it is healthy or how to recover it.

Define:

- **Metrics:** request rate, latency percentiles, error rate, saturation, queue depth, cache hit ratio, dependency latency, and retry counts.
- **Logs:** structured events with safe context and correlation IDs.
- **Traces:** end-to-end spans across meaningful service boundaries when tracing is available.
- **Alerts:** symptoms tied to user impact or a clear operational action.
- **Runbooks:** diagnosis, mitigation, rollback, and escalation steps.
- **Ownership:** team accountable for the service and its dependencies.

Avoid high-cardinality metrics and sensitive log fields. Set alert thresholds from service objectives and observed baselines rather than choosing arbitrary values.

## 10. Deployment, migration, and rollback

Architecture includes how change reaches production.

- Prefer backward-compatible API and schema changes.
- Use expand-and-contract migrations when producers and consumers roll out independently.
- Separate deployment from feature activation when a feature flag is appropriate.
- Define canary or staged rollout signals where supported.
- Make rollback possible without corrupting data or leaving incompatible schema changes.
- Test configuration as well as application code.
- Define how to reconcile events or writes created during a failed rollout.

For AEM as a Cloud Service, align deployment and configuration with Cloud Manager and the supported runtime model. Do not assume that mutable infrastructure, local filesystem state, or unrestricted runtime configuration behaves as it would on a self-managed AEM 6.5 installation.

## 11. Worked scenario: global product content and inventory

A retailer serves multiple locales through AEM. Product content is authored in AEM, while price and inventory are owned by external systems. The page must be fast, public content can be cached, and inventory freshness has a stricter requirement than descriptive content.

A defensible design process:

1. Separate ownership: AEM owns authored product content; pricing and inventory systems own their respective business facts.
2. Define freshness targets for each data type and agree on which values may be stale.
3. Cache public descriptive content at CDN/Dispatcher layers using safe variation keys.
4. Avoid putting personalized or authorization-dependent responses in a shared cache.
5. Decide whether price and inventory are rendered synchronously, obtained through a bounded client-side request, or supplied through a suitable read model.
6. Allocate an end-to-end latency budget and configure timeouts for each dependency.
7. Define failure behavior: which fields can be omitted or marked unavailable, and which business actions must fail closed.
8. Protect downstream services with concurrency limits, bounded retries, and rate-limit handling.
9. Instrument cache hits, dependency latency, stale-data signals, errors, and request saturation.
10. Test peak traffic, dependency slowdown, malformed responses, cache purge delays, and recovery.
11. Roll out gradually and define measurable rollback conditions.
12. Record unresolved assumptions and owners in an Architecture Decision Record.

The important result is not a specific technology stack. It is an explicit contract between content, delivery, and transactional systems, with freshness, security, failure behavior, and ownership made testable.

## 12. Architecture Decision Records

Use an ADR when a decision has meaningful long-term consequences, multiple credible options, or future teams will need to understand the rationale.

A useful ADR includes:

- context and problem;
- requirements and constraints;
- options considered;
- decision and rationale;
- trade-offs and risks;
- security and operational implications;
- validation plan and revisit triggers.

Record the decision, not a sales pitch. An ADR should explain why alternatives were not selected and what new evidence would justify revisiting the choice.

## 13. System design review checklist

- [ ] Functional requirements and critical user journeys are concrete.
- [ ] Quality attributes have measurable targets or explicit criteria.
- [ ] Workload estimates include peak, burst, payload, and data growth.
- [ ] Component boundaries and authoritative data ownership are clear.
- [ ] Network boundaries have contracts, authentication, timeouts, and failure behavior.
- [ ] Consistency, freshness, cache keys, and invalidation are defined.
- [ ] Retry, idempotency, overload control, and partial failure are addressed.
- [ ] Threat boundaries and least-privilege access are reviewed.
- [ ] Metrics, logs, traces, alerts, runbooks, and service ownership are defined.
- [ ] Deployment, compatibility, migration, and rollback are safe.
- [ ] Decisions and assumptions are documented with owners and revisit triggers.
- [ ] Risks are tested through representative load and failure scenarios.

## Authoritative references

- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
- [Google Cloud Architecture Framework](https://cloud.google.com/architecture/framework)
- [Microsoft Azure Well-Architected Framework](https://learn.microsoft.com/en-us/azure/well-architected/)
- [AEM as a Cloud Service architecture](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture)
- [AEM as a Cloud Service development guidelines](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines)
- [AEM Dispatcher overview](https://experienceleague.adobe.com/en/docs/experience-manager-dispatcher/using/dispatcher)

## Completion criteria

This chapter is ready to be marked published only after technical/editorial review, link validation, strict documentation build, required CI checks, and live-site verification. Repository presence and a successful build alone are not release evidence.
