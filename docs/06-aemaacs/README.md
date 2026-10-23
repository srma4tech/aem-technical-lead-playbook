# AEM as a Cloud Service: Architecture, Delivery, and Operations

## Purpose and audience

AEM as a Cloud Service (AEMaaCS) changes how teams build, deploy, operate, and troubleshoot AEM applications. It is not simply AEM 6.5 hosted on different infrastructure. The service provides a managed, continuously updated platform and cloud-native operational model; project teams still own application quality, content and access design, integrations, delivery configuration, and the operational readiness of their solution.

This chapter is for AEM developers, technical leads, architects, and engineers planning or operating an AEMaaCS implementation. It focuses on engineering consequences and decision-making. Confirm implementation details against the current Adobe documentation and the actual program/environment configuration.

## Learning outcomes

After completing this chapter, you should be able to:

- explain the major AEMaaCS tiers and managed-service boundaries;
- distinguish supported cloud development patterns from assumptions inherited from AEM 6.5;
- design code, configuration, and content packages for Cloud Manager delivery;
- reason about scaling, caching, indexing, background work, and external integrations;
- investigate incidents using cloud-appropriate evidence and deployment history;
- plan migration and release validation without treating a successful build as proof of production readiness.

## 1. Understand the operating model

AEMaaCS combines a managed platform with project-specific code, content, configuration, and integrations. Adobe manages much of the underlying infrastructure and platform lifecycle; the project team remains accountable for the behavior and safety of its application.

### High-level service tiers

A typical Sites delivery architecture distinguishes:

- **Author** — authors and administrators create, review, and manage content.
- **Publish** — serves published content and application requests.
- **Preview** — supports review of content before it is published, where configured for the program.
- **Delivery edge and CDN** — caches and routes content according to the service and project configuration.
- **Cloud Manager** — provides the supported CI/CD and environment-management workflow.

Exact topology and capabilities depend on the AEMaaCS offering and program configuration. Do not assume every environment or deployment has identical features.

### What changes from traditional AEM

Traditional AEM operations may involve direct host access, manual service restarts, direct filesystem edits, and operational scripts tied to individual instances. In AEMaaCS, the supported approach is to deliver application code and configuration through the approved pipeline and use the available service tooling and logs.

- **Deployment:** Replace direct server/package copying with supported Cloud Manager pipelines and package structure.
- **Runtime state:** Do not rely on instance-local filesystem or mutable state; design for replaceable runtime instances and supported persistence.
- **Configuration:** Version configuration and deploy it through supported mechanisms instead of making manual instance changes.
- **Scaling:** Design for changing capacity and verify service-specific scaling behavior rather than tuning a fixed number of instances.
- **Troubleshooting:** Use pipeline history, available logs, metrics, environment tools, and reproducible evidence instead of assuming direct host access.
- **Platform updates:** Validate regularly against the managed update cadence rather than treating every platform upgrade as a project-managed server maintenance task.

These are architectural directions, not substitutes for checking current Adobe product documentation.

## 2. Establish ownership and boundaries

A cloud program works best when responsibility is explicit. A useful responsibility map distinguishes the platform provider, project engineering, security, operations, and business content owners.

### Adobe-managed responsibilities

The managed service takes responsibility for defined infrastructure and platform operations, service updates, and platform capabilities described by Adobe. The exact service boundary is documented by Adobe and may vary by product, contract, or deployment.

### Project-team responsibilities

The project team typically remains responsible for:

- application code, custom endpoints, components, Sling Models, workflows, and integrations;
- repository permissions and content exposure decisions;
- Dispatcher configuration and delivery behavior under project control;
- dependency selection, code quality, and secure use of credentials;
- pipeline configuration, release approvals, and application validation;
- capacity-sensitive behavior such as expensive queries, unbounded jobs, and downstream call patterns;
- observability, incident escalation, and evidence collection for application-level failures.

Create an ownership matrix before go-live. For each control, record the owner, evidence source, escalation route, and review cadence. Avoid vague statements such as “Adobe handles operations” or “the application team owns everything.”

## 3. Build and deploy through Cloud Manager

Cloud Manager is the supported CI/CD and environment-management path for AEMaaCS. Treat the pipeline as a controlled release system, not just a convenient way to upload a package.

### Pipeline design

A robust delivery flow generally includes:

1. change review and source control;
2. build and automated tests;
3. code quality and applicable security analysis;
4. package and configuration validation;
5. deployment to the target environment through the configured pipeline;
6. smoke, functional, and integration validation;
7. production promotion with an explicit release decision;
8. post-deployment observation and rollback or mitigation readiness.

The exact pipeline types, gates, and supported steps depend on the program. Keep production promotion governed by evidence and approvals appropriate to the risk.

### Package structure and configuration

Keep application code, repository structure, and environment-specific configuration organized according to Adobe's current package and deployment guidance. Avoid relying on manual changes that are not represented in source control: they create configuration drift and make later deployments unpredictable.

Before introducing a package or configuration change, ask:

- Is this path and package type supported in the target deployment?
- Could it overwrite product-managed content or configuration?
- Is the change reproducible from source control?
- Can it be tested in a lower environment?
- What is the expected behavior on the next deployment?
- How will failure be detected and mitigated?

### Release evidence

A green build is necessary but not sufficient. Release evidence should include the expected commit and pipeline result, test outcomes, configuration validation, key functional journeys, integration health, and a post-deployment observation window. For high-risk changes, define a rollback or mitigation plan before deployment.

## 4. Configuration and runtime assumptions

Cloud-ready code avoids dependence on mutable, instance-local state and unsupported administrative access.

- Keep configuration versioned and deployed through the supported mechanisms.
- Use environment-appropriate configuration without embedding secrets in source code or content packages.
- Do not rely on local filesystem writes for durable business data.
- Avoid custom runtime operations that assume direct access to the underlying host.
- Verify the supported lifecycle for schedulers, jobs, listeners, and background tasks.
- Make operations safe if an instance is replaced or a task is retried.
- Prefer explicit timeouts and bounded resource use.

When migrating an existing solution, search for assumptions about local paths, instance names, manual console edits, fixed topology, direct service restarts, and mutable state. Each finding needs an explicit supported replacement or a documented reason it remains valid.

## 5. Content and repository design

Cloud deployment does not remove the need for sound JCR and content architecture. Expensive queries, unbounded traversals, excessive writes, and permission mistakes can still create performance or security incidents.

- Model content around access patterns and authoring needs.
- Keep queries bounded and validate query plans and indexes against representative data.
- Use supported indexing configuration and deployment practices.
- Avoid broad repository traversal in request-serving code.
- Make writes idempotent where retries are possible.
- Apply least privilege to service identities and content paths.
- Test content migrations and package filters in a representative lower environment.
- Keep generated or operational state out of locations that should be treated as deployable source.

Indexing and repository behavior can be version- and service-sensitive. Consult current Adobe guidance before porting a traditional AEM configuration directly into AEMaaCS.

## 6. Delivery, Dispatcher, and caching

The delivery path may include CDN behavior, Dispatcher, publish, and application logic. Debugging must distinguish a cached response from an origin response and identify which layer owns the observed behavior.

### Design questions

- Which paths and methods should be publicly reachable?
- Which responses are cacheable, and what keys or headers affect cache behavior?
- Can personalized or authenticated content enter a shared cache?
- What invalidates content after activation or deployment?
- How do redirects, errors, selectors, extensions, and query parameters behave through the delivery chain?
- Are origin and edge routes restricted to the intended access path?

Do not assume that a successful publish activation proves every edge cache has the expected content. Verify the real public request path and cache headers using supported tools. Use the [Dispatcher chapter](../03-dispatcher/README.md) for filter, cache eligibility, and troubleshooting fundamentals.

## 7. Scaling and performance

The service can scale according to its operating model, but application design still determines how efficiently that capacity is used. Scaling should not be the first response to a slow query, request amplification, or a blocked external dependency.

### Engineering priorities

- Measure end-user latency and error rates, not only server-side averages.
- Track tail latency, throughput, saturation, and dependency timing.
- Use bounded queries, appropriate indexes, and efficient rendering.
- Avoid unbounded concurrency and thread creation.
- Use connection and request timeouts for external systems.
- Apply retries only where safe, bounded, and appropriate to the failure.
- Make background processing observable and safe under retries or duplicate execution.
- Test representative content volume, traffic shape, and integration behavior.

AEMaaCS resource and scaling behavior is service-specific. Do not import fixed-instance tuning advice from a self-managed installation without validating whether the relevant control exists and is supported.

## 8. Integrations and asynchronous work

External systems can become the limiting factor for an otherwise healthy AEM deployment. A synchronous integration in a request path should have a clear latency budget, timeout, error contract, and fallback behavior.

For each integration, document:

- caller and target identities;
- authentication and credential ownership;
- data sensitivity and allowed scope;
- connect/read timeouts and response-size limits;
- retry and backoff policy;
- idempotency behavior for mutations;
- circuit-breaking or graceful-degradation behavior where appropriate;
- logs, correlation identifiers, and escalation ownership.

Do not assume an in-process scheduler is a globally unique distributed lock. Validate the execution semantics of the chosen mechanism in the target service. Background work should be bounded, observable, restart-safe, and designed to tolerate retry or duplicate delivery when the platform or integration can produce it.

## 9. Security and compliance

Managed infrastructure reduces some operational responsibilities, but it does not make unsafe application code, excessive permissions, exposed endpoints, or leaked secrets acceptable.

- Apply least privilege to users, service identities, and repository paths.
- Validate authorization at every protected operation.
- Validate untrusted input and encode output for its context.
- Keep secrets out of code, packages, browser assets, and logs.
- Review dependency vulnerabilities and pipeline permissions.
- Verify Dispatcher/CDN exposure and cache privacy.
- Use current Adobe documentation to understand the service's security controls and the customer's responsibilities.
- Keep privacy, retention, data residency, and compliance decisions aligned with the organization's requirements and actual service configuration.

See the [Security chapter](../05-security/README.md) for the application security workflow and review checklist.

## 10. Migration from AEM 6.5 or another hosting model

A migration is a change in runtime assumptions, deployment model, operations, and sometimes architecture—not just a package conversion.

### Discovery checklist

Inventory:

- custom bundles, OSGi configurations, service users, and repository permissions;
- custom workflows, schedulers, listeners, and background processing;
- local filesystem dependencies and instance-specific state;
- Dispatcher filters, rewrites, virtual hosts, cache rules, and invalidation;
- indexes, queries, large content traversals, and repository-intensive jobs;
- integrations, credentials, network allowlists, certificates, and timeouts;
- package filters, overlays, customizations, and dependencies on product internals;
- operational runbooks, monitoring, support routes, and release approvals.

### Migration sequence

1. Establish the target service model, environment topology, constraints, and ownership.
2. Classify each discovered dependency as supported, replaceable, or requiring redesign.
3. Refactor unsupported runtime and deployment assumptions before moving large volumes of content.
4. Validate package structure, configuration, indexes, and Dispatcher transformation using supported tooling.
5. Test representative content and key user journeys in a lower environment.
6. Test integrations under realistic latency, failure, and credential-rotation conditions.
7. Validate security, performance, accessibility, and cache behavior.
8. Rehearse release and incident procedures with the team that will operate the service.
9. Capture residual risks, owners, mitigations, and a go/no-go decision.

Avoid a “lift and hope” migration. A green pipeline does not prove that content permissions, cache rules, background jobs, and downstream dependencies behave correctly.

## 11. Troubleshooting and incident response

Start with a specific symptom, timeframe, environment, and affected journey. Collect evidence before making changes.

- **Deployment failed:** Inspect the exact pipeline step, build output, validation message, package/configuration change, and commit.
- **Works in lower environment but fails in production:** Compare deployed commit, environment configuration, permissions, external credentials, routing, and data differences.
- **Stale public content:** Check activation outcome, cache headers, CDN/Dispatcher behavior, invalidation, and whether the response is from cache or origin.
- **Slow requests:** Separate edge/cache misses from publish rendering, repository queries, and downstream dependency latency.
- **Background task is duplicated or missing:** Inspect scheduling/execution semantics, idempotency, logs, retry behavior, and any assumptions about a single instance.
- **Integration errors:** Check timeout, TLS/authentication, credential validity, response shape, rate limits, retry amplification, and dependency health.
- **Access denied:** Verify the caller identity, effective repository permissions, endpoint authorization, and delivery-layer filters independently.

Use available Cloud Manager information, supported logs and environment tools, request identifiers, timestamps, and deployed commit history. The exact diagnostic facilities differ by environment and service configuration. Do not invent access to host-level tools that the service does not expose.

For incidents, define severity and escalation using the organization's process. Preserve evidence, contain exposure when needed, mitigate user impact, validate recovery, and document the root cause and preventive actions.

## 12. Worked scenario: a release passes CI but breaks delivery

A release introduces a Dispatcher configuration change. The pipeline succeeds, but a public product-detail route returns 403 after deployment.

A disciplined investigation:

1. Capture the exact public URL, method, response headers, timestamp, and affected environment.
2. Establish whether the response is generated by the edge, Dispatcher, publish, or application.
3. Compare the deployed configuration and commit with the previous known-good release.
4. Review filter ordering and the exact path, selector, extension, and method involved.
5. Check whether the intended route depends on a new selector or extension that the filter does not allow.
6. Test a safe, representative request in a lower environment and inspect the resulting delivery behavior.
7. Make the smallest supported configuration correction, review it, and deploy through the approved pipeline.
8. Verify both the intended route and nearby forbidden routes to ensure the fix does not broaden public access.
9. Confirm cache behavior and observe error rates after deployment.
10. Record the cause, test gap, and new regression test.

The goal is not merely to restore one URL. It is to prove that the route works and the security boundary remains intact.

## 13. Go-live readiness checklist

- [ ] Service topology, environment purpose, and responsibility boundaries are documented.
- [ ] Code and configuration are delivered through supported, reproducible pipelines.
- [ ] Pipeline gates, release approvals, and post-deployment verification are defined.
- [ ] Unsupported filesystem, host, and mutable-runtime assumptions have been removed or resolved.
- [ ] Repository access, service identities, and protected endpoints follow least privilege.
- [ ] Dispatcher/CDN routes, caching, and invalidation have been tested end to end.
- [ ] Indexes and queries have been validated against representative content.
- [ ] Integrations have explicit timeouts, bounded retries, credential ownership, and failure behavior.
- [ ] Background work is bounded, observable, and safe under retries or duplicate execution.
- [ ] Security, functional, performance, and accessibility tests cover critical journeys.
- [ ] Monitoring, incident escalation, support ownership, and recovery procedures are ready.
- [ ] Migration risks and exceptions have owners and review dates.
- [ ] Adobe's current documentation has been checked for service-specific constraints.

## Authoritative references

Use current Adobe documentation as the source of truth for supported capabilities and service-specific constraints.

- [AEM as a Cloud Service documentation](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service)
- [Introduction to AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/introduction)
- [AEM as a Cloud Service architecture](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture)
- [AEM as a Cloud Service development guidelines](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines)
- [Cloud Manager introduction](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-manager/content/introduction)
- [AEM as a Cloud Service content delivery overview](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/overview)
- [Dispatcher overview for AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview)
- [Migration journey to AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/migration-journey/overview)
- [Security overview for AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/cloud-service-security-overview)

## Completion criteria

This chapter should be marked published only after technical and editorial review, link validation, strict documentation build, required CI checks, and live-site verification. Repository presence and a successful build alone are not release evidence.
