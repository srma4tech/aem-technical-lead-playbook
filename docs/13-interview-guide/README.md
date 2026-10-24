# Interview Guide: AEM, Java, Architecture, and Technical Leadership

## Purpose

This guide helps experienced engineers prepare for interviews that test more than memorized definitions. Strong senior-level answers connect concepts to real systems, explain trade-offs, identify failure modes, and show how the candidate validates assumptions. Use it to structure practice, not to memorize scripts; expectations vary by role and company.

## A repeatable answer framework

For technical questions, use this sequence:

1. **Clarify:** ask about versions, scale, constraints, and the desired outcome.
2. **Explain:** state the core concept in plain language.
3. **Apply:** connect it to a realistic implementation or production scenario.
4. **Trade off:** explain alternatives and why you would choose one.
5. **Validate:** describe tests, metrics, logs, or evidence that would confirm the solution.
6. **Caveat:** name relevant version, security, lifecycle, or operational constraints.

For incidents, use **impact → evidence → hypothesis → safe mitigation → verification → prevention**.

For leadership questions, describe a real situation, your specific contribution, the decision, the outcome, and what you learned. Be precise about your contribution versus the wider team's work.

## 1. Prepare by interview stage

### Recruiter or hiring-manager screen

Explain your role, strengths, target responsibilities, and why the role fits. Prepare a concise professional summary and two or three examples that demonstrate scope, collaboration, and measurable outcomes. Avoid listing every technology you have touched; select the experience most relevant to the role and be honest about depth.

### Technical deep dive

Expect follow-up questions about implementation details, failure modes, debugging, performance, security, and trade-offs. If you do not know an answer, state what you know, what you would verify, and how you would investigate.

### System design

Clarify requirements before drawing architecture. Discuss workload, boundaries, data ownership, API contracts, caching, consistency, security, resilience, observability, deployment, and cost. Explain the critical trade-offs rather than naming technologies without justification.

### Leadership and behavioral round

Prepare examples of disagreement, incident response, mentoring, delivery risk, design review, and a decision you changed after new evidence. Demonstrate accountability without claiming sole ownership of work delivered by a team.

## 2. AEM interview domain map

### AEM architecture and request lifecycle

Explain the path from client request through CDN/Dispatcher, publish tier, Sling resource and servlet resolution, repository access, rendering, and response caching. Explain where a 403, 404, stale response, slow query, or rendering error can originate.

Practice questions:

- How does a request resolve to a Sling resource and servlet?
- What is the difference between a resource type and a resource supertype?
- How do selectors, extensions, methods, and resource types influence servlet resolution?
- How do Sling Models adapt data, and what belongs in a model versus an OSGi service?
- How do Declarative Services activation and service references affect runtime behavior?

A strong answer includes version and configuration assumptions and explains how you would verify the behavior in the target environment.

### JCR, Oak, and content modelling

Review nodes and properties, resource resolvers, sessions, query design, indexes, permissions, and lifecycle. Explain why broad traversal or unbounded result sets can harm production performance.

Practice questions:

- How would you diagnose a slow repository query?
- When would you use a service resolver, and how would you constrain its permissions?
- Why should request-bound resolvers not be stored in singleton fields?
- How would you model content that varies by locale or site?
- What evidence would justify a new Oak index?

Start with query shape, expected cardinality, existing indexes, and measured behavior before proposing an index or content-model rewrite.

### Dispatcher and caching

Understand filter allow/deny behavior, rewrite rules, cache eligibility, invalidation, cache keys, TTLs, and the interaction between CDN and Dispatcher.

Practice questions:

- A public page returns 403 only through Dispatcher. How do you isolate the cause?
- Why might activated content remain stale?
- How can a query string affect cacheability?
- How would you prevent personalized responses from entering a shared cache?
- What changes when moving Dispatcher configuration to AEM as a Cloud Service?

Do not solve access issues by allowing broad path patterns. Explain the narrow rule, negative tests, and verification from the external request path.

### AEM as a Cloud Service

Explain cloud architecture, Cloud Manager pipelines, supported configuration, immutable deployment assumptions, environment variables and secrets, CDN/Dispatcher behavior, and operational boundaries.

Practice questions:

- How would you migrate a legacy Dispatcher configuration?
- Which local development assumptions are unsafe in Cloud Service?
- How do you handle environment-specific integration endpoints?
- How would you validate a deployment before and after rollout?
- What would you investigate if a pipeline succeeds but runtime behavior differs?

Tie answers to official documentation and the supported target version.

### Security and production engineering

Prepare service-user least privilege, ACLs, public endpoint exposure, input validation, output encoding, CSRF considerations, dependency security, safe logging, timeout policy, and incident response.

Practice questions:

- How would you secure a custom Sling servlet?
- What is the risk of an administrative resolver?
- How do you investigate a sudden increase in 403 responses?
- How would you prevent a slow downstream API from exhausting request capacity?
- Which signals distinguish a cache issue from an application bottleneck?

## 3. Java and enterprise engineering domain map

### Core Java and collections

Be ready to reason about equality and hashing, immutability, collection choice, generics, exception contracts, and complexity. Explain why a data structure fits the workload rather than reciting its definition.

Practice questions:

- When is a HashMap unsuitable for a requirement?
- What makes a class immutable in practice?
- How do equals and hashCode affect collection behavior?
- How do you avoid exposing mutable internal state?
- How would you handle a large or untrusted input safely?

### Concurrency and asynchronous work

Review thread safety, atomicity, locks, executor lifecycle, bounded queues, CompletableFuture composition, cancellation, timeouts, and memory visibility.

Practice questions:

- Why does a concurrent map not make a multi-step operation atomic?
- How can an unbounded queue cause an outage?
- When should blocking work use a dedicated executor?
- How do you avoid deadlocks or thread starvation?
- What must be tested for asynchronous failure and cancellation?

Explain the invariant being protected and the resource limit being enforced.

### Integration and resilience

Prepare HTTP contracts, TLS, authentication, timeout budgets, retries with jitter, idempotency, circuit breakers, rate limits, response validation, and observability.

Practice questions:

- Which failures are safe to retry?
- How do you prevent duplicate side effects after a timeout?
- How do you allocate an end-to-end latency budget?
- When is stale data an acceptable fallback?
- How do you test a provider that is intermittently unavailable?

### Testing and delivery

Discuss unit, component, integration, contract, performance, and failure tests. Explain how you keep asynchronous tests deterministic and how CI checks protect the release.

Practice questions:

- What belongs in a unit test versus an integration test?
- How would you reproduce a production-only failure?
- How do you prevent a flaky test from undermining trust in CI?
- How would you test compatibility across API versions?
- What evidence should be required before a release?

## 4. System design interview structure

A 45-minute design interview can use this sequence, adapting to the interviewer:

1. **Requirements:** critical use cases, non-goals, constraints.
2. **Quality attributes:** latency, availability, consistency, security, cost.
3. **Workload:** peak traffic, read/write ratio, data size, growth.
4. **High-level design:** components, responsibilities, and data ownership.
5. **Contracts:** API shape, event flow, authentication, versioning.
6. **Data and caching:** source of truth, freshness, invalidation.
7. **Failure handling:** timeouts, retries, idempotency, partial failure.
8. **Operations:** metrics, alerts, deployment, rollback, recovery.
9. **Trade-offs:** alternatives, risks, and assumptions to validate.

Do not over-design before requirements are clear. If the interviewer adds a constraint, adapt the design and explain what changes.

## 5. Scenario practice bank

Use these prompts for timed practice. First answer aloud in five minutes, then expand with a diagram or written review.

### Scenario A: Dispatcher returns 403

A page works on publish but fails through Dispatcher after a deployment. Cover request-path evidence, effective filters, rewrites, intended public access, narrow configuration changes, negative tests, and external verification.

### Scenario B: Slow AEM page

A product page has rising p95 latency while average latency looks normal. Cover percentiles, cache hit ratio, request classes, repository query evidence, dependency timing, thread/resource saturation, and controlled validation.

### Scenario C: Third-party API outage

A downstream service begins timing out and unrelated requests queue behind it. Cover end-to-end timeout budgets, bounded concurrency, retry amplification, safe degradation, dependency metrics, and recovery tests.

### Scenario D: Cloud migration

A legacy AEM deployment relies on local filesystem changes and environment-specific configuration. Cover supported cloud configuration, content/code separation, pipeline validation, secrets, Dispatcher transformation, integration testing, staged rollout, and rollback constraints.

### Scenario E: Conflicting architecture opinions

Two engineers disagree about introducing an asynchronous event flow. Clarify requirements, compare synchronous and asynchronous failure semantics, quantify operational complexity, validate assumptions, record the decision, and define when it should be revisited.

### Scenario F: Incident ownership

A production incident occurs during a release, and several teams propose competing fixes. Establish impact, assign an incident coordinator, preserve evidence, separate mitigation from root-cause work, communicate facts and uncertainty, and verify recovery before closing.

## 6. Behavioral answer framework

Use a concise story structure:

- **Situation:** relevant context and stakes.
- **Task:** the outcome or responsibility you had.
- **Action:** specific decisions and work you contributed.
- **Result:** measurable outcome or observable change.
- **Reflection:** what you learned and what you would improve.

Avoid stories that only describe the team or use “we” to hide your contribution. Also avoid claiming independent ownership of a shared result. Strong answers make collaboration and personal accountability both clear.

Prepare examples for a difficult production issue, technical disagreement, design decision with trade-offs, mentoring or unblocking another engineer, delivery risk raised early, assumption corrected by evidence, and process improvement that reduced recurring work.

## 7. Self-assessment rubric

Score each practice answer from 1 to 4.

- **1 — Recall only:** definitions with little application or no clear structure.
- **2 — Applied:** plausible implementation, but gaps in constraints or failure handling remain.
- **3 — Senior-level:** explains trade-offs, tests, failure modes, and operational impact.
- **4 — Lead/architect-level:** frames the problem, identifies assumptions, aligns stakeholders, compares alternatives, defines measurable validation, and explains long-term ownership.

Do not score yourself based on confidence alone. Keep notes on missing evidence and revisit the weakest topics with a practical exercise.

## 8. A two-week practice cycle

- **Days 1–2:** AEM request lifecycle, Sling resolution, JCR/Oak, and content modelling.
- **Days 3–4:** Dispatcher filters, cache behavior, and security boundaries.
- **Days 5–6:** Java collections, exceptions, concurrency, and asynchronous work.
- **Day 7:** Mock technical deep dive; review weak areas.
- **Days 8–9:** Integration resilience, testing, and production diagnostics.
- **Days 10–11:** System design with workload and failure analysis.
- **Day 12:** Behavioral stories and decision communication.
- **Day 13:** Full mock interview across technical and leadership topics.
- **Day 14:** Review notes, close knowledge gaps, and repeat the lowest-scoring scenario.

Adjust the plan to the target role. A backend-heavy AEM role needs more implementation depth; an architect role needs stronger system boundaries, business trade-offs, governance, and communication.

## Authoritative references

- [AEM as a Cloud Service documentation](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service)
- [AEM 6.5 documentation](https://experienceleague.adobe.com/en/docs/experience-manager-65)
- [AEM Dispatcher documentation](https://experienceleague.adobe.com/en/docs/experience-manager-dispatcher/using/dispatcher)
- [Java SE documentation](https://docs.oracle.com/en/java/javase/)
- [Java concurrency tutorial](https://docs.oracle.com/javase/tutorial/essential/concurrency/)
- [Google Engineering Practices: code review](https://google.github.io/eng-practices/review/)
- [Google Site Reliability Engineering book](https://sre.google/sre-book/table-of-contents/)

## Completion criteria

Use this guide as a practice framework and verify version-sensitive facts against authoritative documentation. It is ready to be marked published only after editorial review, link validation, strict documentation build, required CI checks, and live-site verification.
