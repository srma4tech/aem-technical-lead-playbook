# Engineering Case Studies: Evidence, Trade-offs, and Lessons

## Purpose and source integrity

Case studies help engineers connect principles to decisions under real constraints. They should show evidence, alternatives, consequences, and learning—not merely describe a sequence of activities.

**The scenarios in this chapter are illustrative composites, not claims about a specific customer, employer, or production incident.** They do not represent verified outcomes from a real organization. Use them to practice analysis. Any future real-world case study must be authorized, anonymized, evidence-backed, and reviewed for confidential information.

## A case-study analysis framework

For each case, capture:

1. **Context:** system boundaries, users, and business outcome.
2. **Symptoms:** what was observed and how impact was measured.
3. **Evidence:** logs, metrics, traces, test results, and change history.
4. **Hypotheses:** plausible explanations and how they were tested.
5. **Options:** feasible mitigations and their trade-offs.
6. **Decision:** what was selected and why.
7. **Validation:** evidence that the change worked and did not create a new problem.
8. **Learning:** changes to design, tests, monitoring, documentation, and ownership.

Separate confirmed facts from assumptions. Avoid false precision, invented metrics, or implying causation from timing alone.

## Case 1: Stale content after publication

### Context

An illustrative multi-locale AEM site publishes updated product descriptions. Editors report that some visitors continue to see old content after activation, while other visitors see the new version.

### Initial observations

The inconsistent behavior suggests that the source content may be correct while one or more delivery caches retain an older response. It does not yet prove which cache layer is responsible.

### Investigation plan

1. Confirm the content version and activation status in the authoring workflow.
2. Compare responses across affected and unaffected locales, URLs, and query parameters.
3. Capture response headers and cache status from an external client.
4. Determine whether the stale response originates from CDN, Dispatcher, or an application-level cache.
5. Inspect cache keys, TTLs, invalidation triggers, and purge evidence.
6. Check whether a recent configuration or URL-normalization change altered cache eligibility.

### Options and trade-offs

- **Manual purge:** useful for immediate mitigation, but insufficient if invalidation is broken.
- **Shorter TTL:** may reduce stale duration, but increases origin traffic and does not solve every invalidation defect.
- **Correct invalidation flow:** addresses the cause but requires testing all affected cache layers.
- **Change cache key or eligibility:** can fix variation errors, but an overly broad change may reduce cache hit rate or expose personalized responses.

### Recommended response

Use an approved purge to reduce immediate impact once the affected layer is identified. Then correct the invalidation or cache-key behavior, add tests for relevant locale and URL variations, and verify externally after deployment. Do not mark the incident resolved based only on a successful author-side activation.

### Lessons

- Cache behavior is a system of layers, not a single switch.
- Content correctness and delivery freshness are separate properties.
- Verification must include the actual user-facing request path.
- Repeated manual purges indicate a process or configuration defect that needs follow-up.

## Case 2: A downstream API causes request pileup

### Context

An illustrative product page calls a third-party inventory API synchronously. During a provider slowdown, page latency increases and unrelated requests begin to wait for available workers.

### Initial observations

The impact is wider than the inventory field itself. A dependency may be consuming application capacity through slow requests, excessive retries, or unbounded concurrency.

### Investigation plan

1. Establish affected user journeys and the start time.
2. Compare end-to-end latency with inventory API latency.
3. Inspect timeout configuration, connection-pool use, concurrent calls, queue depth, and retry counts.
4. Check whether multiple layers retry the same request.
5. Review whether the business permits stale inventory or a clearly marked unavailable state.
6. Correlate symptoms with a release or provider status, without treating correlation as proof.

### Options and trade-offs

- **Increase worker capacity:** may help if application capacity is genuinely insufficient, but can amplify load on the dependency.
- **Retry more aggressively:** may recover some transient errors but can worsen a provider outage.
- **Bounded timeout and concurrency:** limits resource consumption but may increase visible errors during provider failure.
- **Approved fallback:** protects the page experience but requires clear freshness and business rules.
- **Asynchronous read model:** can reduce request coupling but introduces propagation lag and reconciliation needs.

### Recommended response

Apply an approved mitigation that limits waiting and retry amplification. If the business contract allows it, degrade only the inventory portion rather than failing unrelated page content. Add bounded timeouts, explicit retry rules, dependency metrics, and tests for slow, malformed, and unavailable responses.

### Lessons

- A timeout is a capacity-control mechanism as well as a latency setting.
- Retrying without a total time budget can amplify failure.
- Fallbacks must preserve business meaning.
- Recovery includes verifying that queues drain and normal traffic resumes.

## Case 3: Moving a legacy AEM deployment to AEM as a Cloud Service

### Context

An illustrative organization has a legacy AEM deployment with environment-specific configuration, custom Dispatcher rules, and operational assumptions that rely on direct server access. The target is AEM as a Cloud Service.

### Risks to identify

- unsupported runtime or filesystem assumptions;
- differences in configuration ownership and deployment;
- Dispatcher configuration that does not match cloud requirements;
- hard-coded endpoints or secrets;
- repository query or content-model behavior that scales poorly;
- undocumented external integrations;
- insufficient rollback and post-deployment verification.

### Investigation and planning

1. Inventory code, bundles, content, integrations, Dispatcher rules, custom run modes, and operational scripts.
2. Classify each item as reusable, refactor-required, replace, or retire.
3. Map configuration to supported Cloud Manager and environment-specific mechanisms.
4. Validate external dependencies, authentication, timeout policy, and secret handling.
5. Test repository queries, content migration, cache behavior, and key user journeys.
6. Use official migration tooling and guidance where applicable, validating generated configuration rather than assuming it is correct.
7. Plan staged rollout, monitoring, rollback limits, and data reconciliation.

### Options and trade-offs

- **Lift-and-shift assumptions:** may appear faster but carries high compatibility and operational risk.
- **Refactor every component before migration:** can reduce legacy complexity but expands scope and delays learning.
- **Risk-led migration:** validates high-risk components early, preserves working behavior where supported, and sequences refactoring based on evidence.

### Recommended response

Use a risk-led migration. Establish a supported baseline, test the highest-risk integrations and Dispatcher rules early, and document decisions and unresolved gaps. Treat a successful build as one validation signal, not proof that runtime behavior, content freshness, security, and recovery are correct.

### Lessons

- Cloud migration changes operating assumptions, not only deployment syntax.
- Configuration, secrets, observability, and ownership need explicit treatment.
- Compatibility should be verified against the target runtime and current Adobe documentation.
- Rollback plans must account for data and content changes, not only application binaries.

## Case 4: Architecture disagreement over asynchronous processing

### Context

A team proposes replacing a synchronous integration with event-driven processing. One engineer argues that it will improve resilience; another worries about operational complexity and delayed consistency.

### A useful decision process

1. Clarify the actual requirement: latency, throughput, durability, burst handling, or failure isolation.
2. Define which result must be available to the caller immediately.
3. Compare synchronous and asynchronous failure modes.
4. Estimate event volume, delivery guarantees, duplicate handling, ordering, and operational ownership.
5. Prototype the highest-risk assumption if the evidence is weak.
6. Choose the simplest design that meets the requirement.
7. Record the decision, accepted trade-offs, validation plan, and conditions for revisiting it.

### Lessons

- A pattern is not a goal in itself.
- Asynchronous processing introduces consistency and operations work that must be owned.
- Disagreement becomes productive when options are evaluated against shared requirements.
- An ADR preserves rationale but does not replace stakeholder alignment.

## 5. What a strong case study should not do

- Invent production metrics or claim outcomes without evidence.
- Reveal customer names, internal URLs, credentials, identifiers, or proprietary architecture.
- Confuse a hypothesis with a confirmed root cause.
- Hide failed attempts that materially changed the diagnosis.
- Present a technology choice as universally correct.
- Claim sole ownership of a team result without evidence.
- Close with generic lessons that cannot be converted into an action.

## 6. Case-study review checklist

- [ ] The scenario is clearly labeled as real, anonymized, or illustrative.
- [ ] Confidential and identifying details have been removed or authorized.
- [ ] The business and technical context is sufficient to understand the decision.
- [ ] Evidence, assumptions, and hypotheses are distinguished.
- [ ] Options and trade-offs are represented fairly.
- [ ] The selected action is connected to stated requirements.
- [ ] Validation and residual risks are described.
- [ ] Lessons translate into specific tests, design changes, or operational improvements.
- [ ] Technical references support version-sensitive claims.
- [ ] A reviewer has checked factual accuracy and confidentiality.

## Authoritative references

- [AEM as a Cloud Service architecture](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture)
- [AEM Dispatcher documentation](https://experienceleague.adobe.com/en/docs/experience-manager-dispatcher/using/dispatcher)
- [AEM as a Cloud Service development guidelines](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines)
- [Google Site Reliability Engineering book](https://sre.google/sre-book/table-of-contents/)
- [Architecture Decision Records](https://adr.github.io/)

## Completion criteria

These examples are illustrative and should not be represented as verified customer outcomes. The chapter is ready to be marked published only after editorial review, link validation, strict documentation build, required CI checks, and live-site verification.
