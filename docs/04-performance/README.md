# Performance Engineering for AEM

## Purpose and audience

Performance engineering is a continuous process of measuring user-visible behavior, identifying the dominant constraint, changing one thing at a time, and validating the result under representative conditions. For AEM, the relevant system often includes the browser, CDN, Dispatcher, Publish, repository, integrations, and authoring or deployment processes.

This chapter is for AEM developers, technical leads, and architects who need to diagnose latency, protect capacity, and prevent regressions. It does not treat a single response-time number or a local benchmark as proof of production readiness.

## Learning outcomes

After completing this chapter, you should be able to:

- define user- and business-relevant performance objectives;
- design repeatable tests and interpret percentiles, throughput, errors, and saturation together;
- distinguish browser, edge/cache, AEM, repository, and downstream-service bottlenecks;
- choose optimizations based on evidence and validate their trade-offs;
- create regression checks and operational signals that keep performance from degrading.

## 1. Start with a measurable question

Avoid starting with “the site is slow.” Convert the report into a question that can be tested.

Examples:

- Did p95 time to first byte increase for public product pages after a release?
- Does the page become slow only on a cold cache, or also on a warm cache?
- Does authoring become slower as asset volume grows?
- Does a specific integration dominate end-to-end latency when its response time rises?
- Does throughput plateau while CPU, thread pools, connection pools, or repository operations saturate?

For each investigation, record the environment, release/build, time window, exact URLs or operation, traffic profile, cache state, and the metric that represents the user impact.

### Build a performance objective

A useful objective describes the user journey and operating conditions, not only a server metric. Include:

- **Scope:** page type, API, authoring workflow, or business transaction.
- **Load:** expected concurrent users, request rate, payload size, and traffic mix.
- **Latency:** target percentile such as p95 or p99, with a clearly defined measurement boundary.
- **Reliability:** acceptable error and timeout rate.
- **Capacity:** expected headroom at forecast peak.
- **Environment:** deployment tier, region, cache state, data volume, and dependency assumptions.

Targets must come from product needs and observed baselines. Do not invent universal thresholds for every AEM deployment.

## 2. Use a measurement model, not one number

### Latency percentiles

An average can hide a poor experience for a subset of requests. Track median and tail percentiles (commonly p95 and p99) alongside throughput and errors. A p95 value means 95% of measured requests completed at or below that duration; it does not mean every user experienced that duration.

Compare like with like: same endpoint class, region, cache condition, payload, and load profile. A change in the traffic mix can make two aggregate numbers misleading.

### Throughput, errors, and saturation

Measure request rate or completed transactions together with latency. A system may show lower latency only because it is rejecting or timing out work. Track error rate and saturation indicators such as CPU, memory pressure, queue depth, thread utilization, connection-pool waits, and downstream limits where those metrics are available.

### End-to-end and component timing

A browser journey can include DNS, connection setup, TLS, edge routing, cache lookup, origin processing, client-side JavaScript, images, fonts, and third-party scripts. Server-side time-to-first-byte does not represent total page usability.

Use distributed traces, request IDs, browser measurements, access logs, and application metrics where available to isolate the expensive segment. Confirm how each timer is defined before comparing values.

## 3. Design repeatable performance tests

A benchmark is useful only if its conditions and limitations are understood.

1. **State the hypothesis.** Example: “The page's p95 origin response is dominated by an uncached product API call.”
2. **Choose representative journeys.** Include high-volume paths, expensive paths, and critical business flows—not just the homepage.
3. **Prepare realistic data.** Content size, indexes, assets, permissions, and query cardinality can change results.
4. **Define cache conditions.** Test cold and warm behavior separately when both matter; record CDN and Dispatcher state.
5. **Ramp load deliberately.** Include steady state, expected peak, and a controlled stress test where permitted.
6. **Capture the whole system.** Record latency percentiles, throughput, errors, resource utilization, and downstream timings.
7. **Repeat the run.** Avoid drawing conclusions from a single noisy sample.
8. **Change one variable at a time.** Record the exact code/configuration change and compare with the same workload.
9. **Check correctness.** A faster response that omits content, returns stale data, or suppresses failures is not an optimization.
10. **Publish the result.** Store the test script/configuration, environment, baseline, result, limitations, and decision.

Run load tests only in authorized environments with an agreed traffic limit and rollback plan. Never stress production casually.

## 4. Find the bottleneck by layer

### Browser and frontend

Inspect network waterfalls, payload sizes, render-blocking resources, JavaScript execution, layout shifts, image formats/sizes, and third-party scripts. Server-side improvements may not fix a client-side bottleneck.

### CDN and Dispatcher

Check cache-hit behavior, TTLs, invalidation, URL variation, request routing, and response headers. High-cardinality query strings or unsafe cache variation can create repeated origin requests. Coordinate with the [Dispatcher chapter](../03-dispatcher/README.md) and review every cache layer before attributing a result to AEM.

### AEM application

Inspect request traces and logs, expensive Sling Models or servlets, repeated repository access, synchronous external calls, excessive rendering work, and thread contention. Separate CPU-bound work from waiting on I/O. Avoid adding caches or asynchronous execution without understanding correctness, invalidation, ordering, and failure behavior.

### JCR/Oak and queries

Use query plans and supported repository diagnostics to understand expensive queries. Check whether the query shape and index design match the actual predicates and ordering. Avoid broad traversal queries and repeated per-item repository lookups in rendering paths. Do not add indexes speculatively: validate the query plan and measure the effect on reads, writes, index build/update cost, and repository resource usage.

### External dependencies

Measure each downstream call and its timeout, retry, and connection-pool behavior. A dependency with long tail latency can dominate an otherwise fast page. Set explicit timeouts appropriate to the user journey; uncontrolled retries can amplify load during an outage.

## 5. Optimize with explicit trade-offs

### Caching

Cache stable public output at the appropriate layer when correctness permits. Define cache identity, TTL, invalidation, personalization rules, and acceptable staleness. A cache can reduce origin load, but it also adds invalidation complexity and can create privacy defects if user-specific output is shared.

### Repository access

Prefer bounded, purposeful reads and queries over repeated lookups or traversal. Fetch only what the use case needs, and avoid hidden N+1 patterns in list rendering. Measure before and after under representative content volume.

### Concurrency and asynchronous work

Concurrency can improve throughput when work spends time waiting, but it can also increase memory usage, contention, queueing, and downstream pressure. Use bounded executors and explicit overload behavior. Avoid unbounded thread creation, unbounded queues, and fire-and-forget tasks whose failures are lost.

### Payload and rendering

Reduce unnecessary markup, payload size, and duplicate work. Consider responsive assets and efficient client-side loading. Validate visual and functional correctness alongside the performance result.

### Keep the optimization honest

For each change, document:

- the measured baseline and hypothesis;
- the exact change and expected mechanism;
- latency, throughput, error, and resource results;
- correctness and security implications;
- any new operational complexity;
- the decision to keep, revise, or revert the change.

## 6. Performance on AEM as a Cloud Service

AEM as a Cloud Service has managed infrastructure and supported deployment/diagnostic workflows that differ from self-managed installations. Do not assume direct host access, arbitrary JVM tuning, filesystem inspection, or traditional operational procedures are available.

Use the current Adobe guidance for the target environment, supported SDK/tooling, Cloud Manager deployment process, and available observability. Keep project-level responsibilities clear: application code, query behavior, cache configuration, content structure, integration behavior, and frontend payload can still create bottlenecks even when the platform is managed.

Compare local SDK results with cloud-environment evidence carefully. Differences in data volume, topology, cache state, integrations, and runtime characteristics can make local timings non-representative.

## 7. Production diagnosis workflow

When a performance alert fires:

1. **Confirm impact:** identify affected journeys, environments, regions, and users; check errors as well as latency.
2. **Establish a baseline:** compare with a known-good period and identify when the change began.
3. **Correlate changes:** deployments, content activations, index changes, cache policy changes, traffic shifts, and dependency incidents.
4. **Localize the layer:** use browser data, edge/cache signals, access logs, traces, application metrics, and dependency timing.
5. **Form one testable hypothesis:** avoid changing several unrelated settings simultaneously.
6. **Mitigate safely:** use an approved rollback, feature flag, scoped cache adjustment, or traffic-control mechanism when evidence supports it.
7. **Validate recovery:** confirm user impact, latency percentiles, errors, and resource pressure return to expected levels.
8. **Prevent recurrence:** add a regression test, alert, capacity check, or documented operational action.

### Common patterns

**High p95, normal median:** inspect tail latency, slow dependencies, lock/queue waits, cache misses, and outlier queries.

**High latency with low CPU:** investigate waiting, connection pools, downstream services, thread starvation, network behavior, and repository contention before adding CPU capacity.

**Latency and errors rise together under load:** inspect saturation, timeouts, queue growth, and retry amplification. More concurrency may worsen the incident.

**Warm cache is fast, cold cache is slow:** investigate origin work, cache population, invalidation, and expected cold-start behavior. Do not assume all traffic will remain warm.

**Only one page family is slow:** compare its Sling Models, queries, payload, personalization, and dependencies with a similar healthy page.

**Performance degrades after content growth:** test with representative repository volume, asset counts, query cardinality, and index state. A small local dataset may conceal a scaling problem.

## 8. Regression prevention and release gates

Performance needs an ongoing feedback loop.

- Keep a repeatable smoke benchmark for critical journeys.
- Run deeper tests for material changes to queries, rendering, caching, integrations, or infrastructure configuration.
- Compare results against a stable baseline and explain variance rather than blindly failing on noisy single-run numbers.
- Track errors, percentiles, throughput, and resource use together.
- Define who reviews regressions and what evidence is required to accept a trade-off.
- Include realistic content and cache scenarios in pre-release validation.
- Keep performance dashboards and alerts actionable, with a clear owner and response procedure.
- Revisit objectives when traffic, content, architecture, or business requirements change.

A performance gate should use project-specific thresholds supported by baseline data. This chapter intentionally does not prescribe one universal p95 or throughput threshold for all AEM projects.

## 9. Worked scenario: a product page slows down

After a release, the p95 response time for public product pages increases. The average changes only slightly, and Publish CPU remains moderate.

A disciplined investigation:

1. Compare the same URL family, region, traffic mix, and cache state before and after the release.
2. Separate warm-cache hits from origin requests and check whether the hit ratio changed.
3. Trace slow origin requests and compare time spent in repository queries, Sling Model work, and downstream calls.
4. Check whether a new model performs repeated per-item lookups or calls an external service synchronously.
5. Reproduce with representative content volume and the same cache conditions.
6. Test a focused change and compare p50/p95/p99, throughput, errors, and resource use.
7. Validate the returned content and behavior, then add a regression check for the identified cause.

The outcome should be a causal explanation backed by measurements—not “we added a cache and it feels faster.”

## 10. Review checklist

- [ ] User journey, environment, workload, and success metric are explicit.
- [ ] Baseline includes latency percentiles, throughput, errors, and relevant saturation signals.
- [ ] Test data, cache state, and run conditions are recorded.
- [ ] The dominant bottleneck is supported by trace, log, or measurement evidence.
- [ ] Changes are isolated enough to attribute the result.
- [ ] Correctness, privacy, security, and freshness remain intact.
- [ ] Downstream timeouts, retries, and concurrency are bounded and understood.
- [ ] Results include trade-offs and limitations, not only the best number.
- [ ] Production mitigation and rollback are safe and authorized.
- [ ] A regression test, alert, or follow-up action prevents recurrence.

## Authoritative references

Use documentation matching the AEM version and environment. These references are starting points; verify current applicability and supported diagnostic methods before implementation.

- [AEM 6.5 platform overview](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/deploying/introduction/platform)
- [AEM as a Cloud Service architecture](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture)
- [AEM development best practices: JCR integration](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/bestpractices/jcr-integration)
- [AEM as a Cloud Service development guidelines](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines)
- [Dispatcher in AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview)
- [Cloud Manager overview](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-manager/content/introduction)

## Completion criteria

This chapter is ready to be marked published only after technical/editorial review is complete, references and internal links are checked, the strict documentation build and required CI checks pass, and the published page is verified. Repository presence alone is not release evidence.
