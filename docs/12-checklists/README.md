# Engineering Checklists: Practical Quality Gates

## Purpose

Checklists reduce avoidable omissions in repeatable, high-consequence work. They do not replace expertise, testing, or critical thinking. Use the smallest checklist that catches meaningful risk, and remove items that no longer help the decision.

Existing repository checklists remain useful and are linked below. This chapter organizes them by engineering activity and adds practical prompts for AEM and enterprise delivery.

## How to use a checklist

- Use it at the point where it can change a decision, not as paperwork after the work is done.
- Mark an item complete only when there is evidence.
- Record exceptions and accepted risks rather than silently skipping controls.
- Tailor the list to the change's blast radius and reversibility.
- Review checklists after incidents, near misses, or repeated review findings.
- Keep ownership clear and avoid duplicate lists that drift apart.

## 1. Change readiness

- [ ] The intended outcome and acceptance criteria are clear.
- [ ] Scope and non-goals are documented.
- [ ] Dependencies, owners, and rollout sequence are identified.
- [ ] Relevant assumptions have been validated or assigned an owner.
- [ ] Security, performance, compatibility, and operational impact are considered.
- [ ] Configuration and secrets are handled through approved mechanisms.
- [ ] A rollback or mitigation plan exists for high-impact changes.
- [ ] Validation evidence is defined before implementation is complete.

## 2. Pull request review

- [ ] The change solves the stated problem and stays within scope.
- [ ] Public contracts and compatibility implications are understood.
- [ ] Input validation, authorization, and sensitive-data handling are correct.
- [ ] Errors, timeouts, retries, and resource cleanup are explicit.
- [ ] Shared state and concurrency behavior are safe.
- [ ] Performance implications are reasonable for expected workload.
- [ ] Tests cover important behavior and failure modes.
- [ ] Tests are deterministic and do not depend on arbitrary sleeps.
- [ ] Logging and metrics are useful without leaking secrets or creating unbounded cardinality.
- [ ] Documentation, configuration, and operational updates are included.
- [ ] Review comments distinguish blockers from suggestions.

Related checklist: [Architecture Review Checklist](../checklists/architecture-review.md).

## 3. AEM implementation review

- [ ] Sling Models expose data and avoid becoming hidden orchestration layers.
- [ ] Reusable business logic belongs in appropriately scoped OSGi services.
- [ ] OSGi component lifecycle and service references are understood.
- [ ] Request-specific state is not stored in singleton service fields.
- [ ] ResourceResolver and session lifetimes are bounded and correctly closed when owned.
- [ ] Repository access uses the correct user identity and least-privilege permissions.
- [ ] Queries are bounded and supported by appropriate indexes.
- [ ] Custom servlets use deliberate registration, method, selector, and resource-type choices.
- [ ] Input is validated and output is safely encoded or rendered.
- [ ] Outbound integrations have timeouts, bounded connections, and clear lifecycle ownership.
- [ ] Environment-specific settings and secrets are not hard-coded.
- [ ] The target AEM version and runtime support the selected APIs and behavior.

## 4. Dispatcher and delivery review

- [ ] Filter rules preserve deny-by-default behavior.
- [ ] Only intended public paths and methods are allowed.
- [ ] Rewrite rules and request normalization are understood.
- [ ] Cache eligibility and cache keys reflect response variation.
- [ ] Personalized or authorization-sensitive content cannot leak through shared caches.
- [ ] TTL and invalidation/purge behavior are documented.
- [ ] CDN and Dispatcher responsibilities are considered separately.
- [ ] Allowed and denied paths are tested, including relevant query and encoding variants.
- [ ] AEM 6.5 versus AEM as a Cloud Service configuration differences are reviewed.
- [ ] A safe rollback exists for delivery configuration changes.

Related chapter: [Dispatcher](../03-dispatcher/README.md).

## 5. Performance review

- [ ] The target user journey and baseline are defined.
- [ ] Measurements include tail latency and error rate, not only averages.
- [ ] Representative data volume, concurrency, and traffic peaks are tested.
- [ ] Browser, CDN, Dispatcher, AEM, repository, and dependency layers are separated during diagnosis.
- [ ] Query shape, result size, rendering cost, and repeated work are reviewed.
- [ ] Connection pools, thread pools, queues, and retries are bounded.
- [ ] The proposed optimization has a measurable hypothesis.
- [ ] Regression tests or monitoring detect recurrence.
- [ ] No speculative JVM or thread-count tuning is used as a substitute for diagnosis.

Related chapter: [Performance](../04-performance/README.md).

## 6. Security review

- [ ] Trust boundaries and data sensitivity are identified.
- [ ] Authentication and authorization are enforced at the correct boundary.
- [ ] Service identities have only the required permissions.
- [ ] Public endpoints and Dispatcher exposure are reviewed.
- [ ] Input validation, output encoding, and request-forgery controls are appropriate.
- [ ] Secrets are stored and rotated through approved mechanisms.
- [ ] Logs and error responses do not disclose credentials or sensitive payloads.
- [ ] Dependencies and packages are reviewed for known risk.
- [ ] Shared caches cannot expose private or personalized data.
- [ ] Security-relevant events and incident escalation are understood.

Related chapter: [Security](../05-security/README.md).

## 7. Integration review

- [ ] The API contract, schema, authentication, and versioning are documented.
- [ ] Connect/read/overall timeout behavior fits the end-to-end latency budget.
- [ ] Response size and pagination are bounded.
- [ ] Remote responses are validated as untrusted input.
- [ ] Retry policy is bounded and limited to suitable failure types.
- [ ] Mutations have an idempotency or deduplication strategy where required.
- [ ] Rate limits, partial failures, and fallback semantics are explicit.
- [ ] Correlation identifiers and safe observability are available.
- [ ] Contract and failure tests cover timeouts, malformed responses, and authorization errors.
- [ ] Dependency ownership and escalation path are known.

## 8. Release and deployment review

- [ ] Required CI checks are green on the exact commit intended for merge.
- [ ] Strict documentation or application build passes.
- [ ] Configuration changes are validated for the target environment.
- [ ] Migration and backward compatibility are considered.
- [ ] Rollout, health signals, rollback triggers, and responsible owners are defined.
- [ ] Release notes and changelog reflect actual changes.
- [ ] Version numbers and tags are supported by repository release evidence.
- [ ] The deployment workflow completes successfully.
- [ ] The live user-facing result is verified.
- [ ] Dashboards and inventory status are updated only after evidence is available.

Related guidance: [Release Strategy](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/RELEASES.md).

## 9. Incident response checklist

- [ ] Impact, severity, affected user journeys, and incident owner are established.
- [ ] A shared timeline records facts, hypotheses, actions, and results.
- [ ] Relevant logs, metrics, traces, and recent changes are preserved.
- [ ] Mitigation has an explicit blast radius and rollback path.
- [ ] Responders have clear task ownership and report-back expectations.
- [ ] Stakeholders receive timely updates that distinguish facts from uncertainty.
- [ ] Recovery is verified using user-facing signals.
- [ ] Data integrity, queues, and residual security risks are checked.
- [ ] Follow-up actions have owners and measurable completion criteria.

Related chapter: [Production Support](../11-production-support/README.md).

## 10. Documentation review

- [ ] Audience, prerequisites, and learning outcomes are clear.
- [ ] The page teaches the topic rather than only listing planned sections.
- [ ] Examples and diagrams are accurate and add value.
- [ ] Version-sensitive claims cite authoritative sources.
- [ ] Internal links and external references work.
- [ ] Security, performance, and operational caveats are included where relevant.
- [ ] Markdown formatting, spell check, and link validation pass.
- [ ] Strict MkDocs build passes.
- [ ] The deployed page is inspected at the live URL.
- [ ] Inventory and roadmap reflect the evidence, not intention.

Use the existing [Documentation Review Checklist](../checklists/documentation-review.md) as the canonical contributor review aid.

## 11. Architecture decision review

- [ ] The problem and constraints are explicit.
- [ ] Requirements and quality attributes are prioritized.
- [ ] More than one credible option was considered where appropriate.
- [ ] Data ownership and component boundaries are clear.
- [ ] Failure, security, performance, cost, and operability trade-offs are documented.
- [ ] Migration and rollback constraints are addressed.
- [ ] Decision owner and affected stakeholders are identified.
- [ ] Validation plan and revisit triggers are recorded.
- [ ] The decision is documented as an ADR when its long-term impact warrants it.

## 12. Checklist governance

A checklist should have a clear purpose, owner, trigger, and evidence expectation. Review it when an incident or recurring defect exposes a gap. If an item is consistently marked complete without useful evidence, rewrite it or remove it. Avoid turning every recommendation into a mandatory checkbox; overlong checklists create checkbox fatigue and weaken attention to the few controls that matter most.

## Related resources

- [Architecture Review Checklist](../checklists/architecture-review.md)
- [Documentation Review Checklist](../checklists/documentation-review.md)
- [Chapter Inventory and Definition of Done](../chapter-inventory.md)
- [Release Strategy](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/RELEASES.md)
- [Roadmap](../ROADMAP.md)

## Completion criteria

This chapter is ready to be marked published only after link validation, strict documentation build, required CI checks, editorial review, and live-site verification.
