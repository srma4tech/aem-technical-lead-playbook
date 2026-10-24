# Production Support: Incidents, Diagnostics, and Recovery

## Purpose and audience

Production support is the engineering discipline of detecting, understanding, mitigating, and learning from failures in a live system. It is not a separate phase that begins after development. Production behavior should influence architecture, implementation, testing, deployment, and ownership from the start.

This chapter is for engineers, technical leads, and support teams operating AEM and enterprise applications.

## Learning outcomes

- Triage incidents using impact, urgency, and evidence.
- Distinguish symptoms from causes and hypotheses from confirmed facts.
- Use logs, metrics, traces, deployment history, and platform signals to narrow a fault.
- Mitigate safely while preserving evidence and a rollback path.
- Communicate clearly during incidents and coordinate multiple responders.
- Convert incidents into measurable reliability improvements.

## 1. Establish operational ownership before launch

A service is not production-ready if nobody knows who owns it, how to detect failure, or how to restore it.

For every important service or integration, define:
- primary and secondary ownership or escalation route;
- user-facing service expectations and critical journeys;
- dashboards and alerts;
- dependency map and external support contacts;
- runbooks for common failures;
- safe mitigation and rollback options;
- access and authorization required for responders;
- backup, recovery, and data reconciliation procedures where relevant.

A runbook should be actionable during stress. It needs the symptom, impact check, diagnostic commands or dashboards, safe mitigation, verification, rollback, and escalation point. Avoid runbooks that merely say “check logs” without specifying where, what to search for, or what evidence matters.

## 2. Triage by impact and urgency

Start by establishing what users or business operations are affected. A single error log may be noise; a small latency increase on a checkout path may be severe.

Ask:
- Which user journeys or locales are affected?
- Is the issue global, regional, tenant-specific, or limited to one content path?
- When did it begin, and what changed near that time?
- Is it getting worse, stable, or recovering?
- Is data integrity, security, or privacy at risk?
- Is there a safe mitigation that can reduce impact immediately?

Use the organization's severity model. If none exists, define one based on user impact, scope, duration, data/security risk, and available workaround. Do not invent severity from the number of alerts alone.

## 3. Preserve evidence and build a timeline

Create a shared incident timeline with timestamps and time zone, observations, actions, and results. Separate facts from hypotheses.

Useful evidence includes:
- request IDs and correlation IDs;
- latency percentiles, traffic, error rate, and saturation;
- affected URL patterns or API operations, sanitized as needed;
- recent code, configuration, content, or infrastructure changes;
- dependency status and latency;
- queue, thread-pool, connection-pool, memory, and repository signals;
- exact mitigation attempts and their observed effects.

Avoid destructive investigation before collecting relevant evidence. Do not copy secrets, access tokens, or unnecessary personal data into incident channels. Ensure clocks and timestamps can be correlated across systems.

## 4. Diagnose systematically

Use a layered approach rather than changing multiple things at once.

### Client and edge

Check DNS/TLS, CDN behavior, cache status, response headers, request rate, and whether the issue is limited to a region or route. Compare a known-good path with an affected path.

### Dispatcher and web tier

Check filter decisions, rewrite rules, cache eligibility, invalidation, farm or virtual-host selection, and origin response. A 403 can indicate a deliberate filter decision, permissions issue, or an unexpected request path; do not respond by broadly allowing paths.

### AEM application

Inspect error rates, request duration, servlet and Sling resolution, OSGi service state, thread saturation, logs, and recent deployment/configuration changes. Determine whether time is spent in application logic or waiting on a dependency.

### Repository and queries

Review query cost, index use, traversal warnings, session/resolver lifecycle, repository latency, and contention. Confirm the actual query plan or platform-supported diagnostic evidence before changing indexes or query code.

### Downstream dependencies

Inspect connection failures, timeout rate, latency percentiles, response codes, quotas, schema changes, and authentication failures. Distinguish a dependency failure from an application failure caused by unbounded waiting or retries.

### Change correlation

Compare the incident start time with recent releases, configuration changes, content activation, scheduled jobs, traffic changes, and provider incidents. Correlation narrows the search but does not prove causation.

## 5. Mitigate safely

The immediate objective is to reduce user impact without creating a larger failure.

Possible mitigations include:
- rollback or disable a recent change;
- stop or throttle a noncritical background workload;
- restore a known-good configuration;
- reduce traffic to a failing dependency;
- disable a feature through an approved flag;
- serve an explicitly approved stale or partial response;
- scale a constrained resource when evidence shows capacity is the bottleneck.

Before acting, identify the expected benefit, blast radius, authorization, verification signal, and rollback path. Change one major variable at a time when possible. Do not increase thread counts, open access filters, or disable security controls as generic emergency measures.

After mitigation, verify the affected user journey, error rate, latency, and dependency health. A green health endpoint alone does not prove the incident is resolved.

## 6. AEM-specific troubleshooting patterns

### Unexpected 403 at Dispatcher

1. Capture the exact request path, method, host, and response headers.
2. Determine whether the denial occurs at the edge, Dispatcher, or AEM publish tier.
3. Inspect the effective filter and rewrite configuration for the route.
4. Check whether the route is intended to be public and whether the request is correctly normalized.
5. Compare with a known-good route and the last configuration change.
6. Add only the narrow rule required by the intended contract; preserve deny-by-default behavior.
7. Test allowed and disallowed paths, methods, query parameters, and encoded variants.

### Stale published content

1. Confirm the authoritative content version and activation status.
2. Identify which cache layer returns stale content.
3. Inspect cache headers, cache keys, TTLs, and invalidation/purge evidence.
4. Determine whether the stale response is global or limited to a variation key or locale.
5. Use the approved invalidation procedure and verify from an external client.
6. Fix the cause if invalidation is not reliably triggered; repeated manual purges are not a durable solution.

### Slow publish requests

1. Establish which paths and request classes are slow.
2. Compare cached and uncached requests.
3. Inspect latency percentiles and thread/resource saturation.
4. Determine whether time is spent in repository access, rendering, or external calls.
5. Review query and payload costs, and confirm timeout behavior for dependencies.
6. Test the proposed fix under representative load and watch for regressions.

### OSGi service unavailable

1. Check component state and activation errors.
2. Verify required configuration and environment-specific values.
3. Inspect service references and dependency availability.
4. Check bundle or API compatibility after a deployment.
5. Avoid restarting broad platform components before understanding the failure scope.
6. Validate recovery and monitor dependent requests.

AEM 6.5 and AEM as a Cloud Service have different deployment and operational models. Use the supported diagnostics and configuration workflow for the actual target environment; do not assume direct server access or mutable infrastructure in Cloud Service.

## 7. Communicate during an incident

A useful update states:
- current impact and affected scope;
- what is confirmed;
- what is being investigated;
- the current mitigation and its result;
- the next update time;
- any decision or assistance required.

Use one incident coordinator for prioritization and stakeholder updates when several responders are involved. Assign discrete investigation tasks with owners and report-back expectations. Avoid duplicate changes and parallel experiments that obscure cause and effect.

Do not promise a restoration time without evidence. Give a best current estimate only when useful, label uncertainty, and update it as new information arrives.

## 8. Recovery and closure

Before declaring recovery:
- verify the affected user journeys;
- confirm error and latency signals return to an acceptable range;
- verify data integrity and reconcile missed or duplicated work;
- confirm queues and dependency backlogs are draining;
- ensure no emergency security or configuration bypass remains;
- capture the change that restored service;
- define enhanced monitoring and a rollback trigger if residual risk remains.

Close an incident when the service is stable and ownership of remaining work is explicit, not merely when the alert clears.

## 9. Post-incident learning

A useful review reconstructs what happened and how the system's design, signals, and processes shaped the outcome. Avoid hindsight bias: decisions should be evaluated against information available at the time.

Cover:
1. Impact and duration.
2. Timeline of detection, diagnosis, mitigation, and recovery.
3. Trigger and contributing conditions, distinguishing confirmed cause from remaining hypotheses.
4. Why monitoring or safeguards did or did not catch the issue.
5. What made mitigation easier or harder.
6. Actions to reduce recurrence and improve detection/recovery.
7. Owners, priority, due dates, and verification criteria.

Actions should change the system or operating practice. “Be more careful” is not a sufficient corrective action. Prefer a regression test, safer default, improved alert, bounded resource, documented runbook, or removal of an unsafe dependency.

## 10. Worked scenario: external API causes request pileup

A product page calls a third-party inventory API. The provider slows down, request latency rises, and AEM workers begin to queue.

A disciplined response:

1. Confirm user impact and identify the first affected time window.
2. Compare end-to-end latency with inventory API latency.
3. Check timeout configuration, connection-pool use, request concurrency, and retries.
4. Stop retries that are amplifying the provider slowdown, if the policy allows and the evidence supports it.
5. Apply an approved fallback or temporarily disable the dependent feature if safe.
6. Monitor request latency, error rates, queue depth, and provider recovery.
7. Verify that normal traffic drains and the user journey recovers.
8. Review why the integration lacked sufficient isolation, timeout budget, or degradation behavior.
9. Add tests for slow responses and provider failure, plus alerts for saturation and dependency latency.
10. Assign and track the prevention work.

Adding more workers may increase pressure on the failing dependency and delay the next failure. Diagnose the constrained resource before scaling.

## 11. Production readiness checklist

- [ ] Service owner, escalation path, and support hours are documented.
- [ ] Critical user journeys and service expectations are defined.
- [ ] Dashboards and alerts cover latency, errors, traffic, and saturation.
- [ ] Logs are searchable and safe, with correlation identifiers where possible.
- [ ] Dependency timeouts, retries, and fallback behavior are explicit.
- [ ] Common failure runbooks include verification and rollback.
- [ ] Deployment and configuration rollback procedures are tested.
- [ ] Security, access, and data-recovery procedures are documented.
- [ ] AEM Dispatcher, repository, OSGi, and cloud-specific failure modes are covered where relevant.
- [ ] Incident communication and post-incident review practices are agreed.
- [ ] Corrective actions have owners and measurable completion criteria.

## Authoritative references

- [Google Site Reliability Engineering book](https://sre.google/sre-book/table-of-contents/)
- [Google Site Reliability Workbook](https://sre.google/workbook/table-of-contents/)
- [AEM Dispatcher documentation](https://experienceleague.adobe.com/en/docs/experience-manager-dispatcher/using/dispatcher)
- [AEM as a Cloud Service architecture](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture)
- [AEM as a Cloud Service development guidelines](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines)
- [Cloud Manager overview](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-manager/content/introduction)

## Completion criteria

This chapter is ready to be marked published only after technical/editorial review, link validation, strict documentation build, required CI checks, and live-site verification. Repository presence and a successful build alone are not release evidence.
