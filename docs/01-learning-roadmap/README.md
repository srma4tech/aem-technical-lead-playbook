# Learning Roadmap

This roadmap helps you move from AEM platform fundamentals toward production ownership, technical leadership, and architecture judgment. It reflects the material currently in the playbook; planned areas are clearly identified rather than presented as completed courses.

## Choose your learning path

|Track|Start here|Focus|Evidence of progress|
|---|---|---|---|
|AEM Developer|[Understanding AEM Internals](../20-understanding-aem-internals/README.md)|Request lifecycle, Dispatcher/Apache, Sling resolution, HTL, JCR/Oak, and response generation|Explain the request path and investigate a rendering or resolution problem using evidence|
|Senior AEM Engineer|[Engineering Principles](../00-engineering-principles/README.md), then [Understanding AEM Internals](../20-understanding-aem-internals/README.md)|Debugging, performance, security, maintainability, and production ownership|Produce a reasoned investigation plan with assumptions, evidence, risks, and validation|
|AEM Technical Lead|[Technical Lead Principles](../00-engineering-principles/02-technical-lead-principles.md), then the AEM internals path|Design trade-offs, code review, delivery governance, mentoring, and operational readiness|Explain a decision, alternatives, consequences, rollout, and recovery approach|
|Future Solution Architect|[Engineering Principles](../00-engineering-principles/README.md), then the technical-lead path|Business/system context, integrations, cross-cutting qualities, architecture governance, and communication|Present a context-aware architecture proposal with constraints, options, risks, and validation|

These are recommended routes, not formal certifications. Completion should be based on demonstrated understanding and practical evidence, not reading time alone.

## Recommended sequence

### Stage 1 — Engineering foundations

**Read:** [Engineering Principles](../00-engineering-principles/README.md)

Build a shared approach to clean code, production ownership, decision-making, performance thinking, security, debugging, mentorship, and engineering values.

**Practice:** Take a recent technical decision and document its goal, constraints, alternatives, trade-offs, and validation plan.

### Stage 2 — Understand AEM request behavior

**Read:** [Understanding AEM Internals](../20-understanding-aem-internals/README.md)

Follow the request path through delivery layers, Apache/Dispatcher, Sling, resource and servlet resolution, script resolution, HTL, JCR/Oak, and response generation.

**Practice:** Draw the request path for one page. Mark cache boundaries, repository access, external dependencies, and points where logs or metrics could distinguish failure causes.

### Stage 3 — Delivery, performance, security, and cloud

**Planned expansion:** AEM Core, Dispatcher, Performance, Security, and AEM as a Cloud Service.

Use the [project roadmap](../ROADMAP.md) and [release dashboard](../release-dashboard.md) to check publication status before treating these as complete learning modules.

**Practice when the relevant material and environment are available:** Assess cacheability, query cost, permissions, outbound calls, deployment constraints, and cloud compatibility with evidence.

### Stage 4 — Enterprise engineering

**Planned expansion:** Enterprise Java, design patterns, system design, and production support.

**Practice:** Describe failure modes and operational expectations for a service integration. Include timeouts, retries where appropriate, observability, security boundaries, and recovery behavior.

### Stage 5 — Technical leadership and architecture

**Planned expansion:** Technical leadership, reviewed case studies, interview preparation, and architecture readiness.

**Practice:** Write a short decision record that connects business context to system options, non-functional requirements, risks, ownership, rollout, and rollback.

## Checkpoints

Use these checkpoints as self-review prompts:

- **Explain:** Can I describe the behavior without relying on memorized definitions?
- **Trace:** Can I follow a request or failure across relevant system boundaries?
- **Investigate:** Can I state what evidence I need before choosing a fix?
- **Decide:** Can I compare options and make trade-offs explicit?
- **Validate:** Can I define tests, metrics, and recovery checks that demonstrate the outcome?
- **Communicate:** Can I explain the decision and its risks to both engineers and stakeholders?

## Current content status

The published AEM internals section contains 15 chapters. Other roadmap areas are at different stages of development. Check the [Roadmap](../ROADMAP.md) and [Release Dashboard](../release-dashboard.md) for the current status; do not infer publication from a planned stage.

## Related resources

- [Introduction](../00-introduction/README.md)
- [Engineering Principles](../00-engineering-principles/README.md)
- [Understanding AEM Internals](../20-understanding-aem-internals/README.md)
- [Engineering Checklists](../12-checklists/README.md)
- [Release Dashboard](../release-dashboard.md)
- [Project Roadmap](../ROADMAP.md)
