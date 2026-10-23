# AEM Security Engineering: Secure Delivery and Operations

## Purpose and audience

Security is a system property spanning identity, application code, repository permissions, public delivery, dependencies, secrets, deployment, and incident response. A secure AEM implementation is not achieved by one Dispatcher rule or a single code scan; controls must work together and be tested at the boundaries where the system can be abused.

This chapter is for AEM developers, technical leads, and architects designing or operating AEM 6.5 and AEM as a Cloud Service. Exact platform capabilities and supported configuration depend on the target version and hosting model. Confirm implementation details against the current Adobe documentation and organizational security requirements.

## Learning outcomes

After completing this chapter, you should be able to:

- threat-model an AEM feature and identify its trust boundaries;
- design least-privilege access for users, service identities, and repository paths;
- secure public delivery and custom endpoints;
- handle input, output, secrets, and dependencies safely;
- include security checks in development, deployment, and incident response;
- explain the evidence required to accept or remediate a security risk.

## 1. Start with assets, actors, and trust boundaries

Before selecting controls, identify what needs protection and who can interact with it.

### Assets

Typical assets include unpublished content, customer or employee data, authoring access, repository content, service credentials, integration tokens, deployment credentials, and the availability and integrity of public sites.

### Actors and entry points

Consider anonymous visitors, authenticated site users, authors, administrators, service identities, CI/CD systems, external APIs, and compromised dependencies. Entry points may include rendered pages, forms, custom Sling servlets, JSON endpoints, GraphQL or other content APIs, asset upload flows, scheduled jobs, and deployment pipelines.

### Trust-boundary questions

- Which routes are reachable from the public internet?
- Which identity is used for each operation, and what permissions does it hold?
- Which input values cross from a less-trusted boundary into code or repository operations?
- Which data or responses can be cached and shared?
- Where are credentials stored, rotated, and revoked?
- What happens if an integration, author account, build credential, or dependency is compromised?

Document a simple threat model for high-risk features. Prioritize risks by likelihood, impact, exposure, and blast radius, then assign an owner and verification method.

## 2. Identity and least privilege

Use the narrowest permissions needed for each human or service identity. Avoid solving permission errors by granting broad administrative privileges.

### Human access

- Separate authoring, administration, deployment, and read-only responsibilities where the platform and organization support those roles.
- Grant repository access through groups and defined roles rather than ad hoc user-level permissions where practical.
- Review access during onboarding, role changes, and offboarding.
- Protect privileged accounts with the organization's approved strong-authentication and access-review controls.
- Avoid using administrator accounts for routine development or automated tasks.

### Service identities

A service identity should have only the permissions required for its defined task and paths. Document its purpose, owner, credentials or authentication mechanism, rotation process, and removal procedure. Do not reuse one highly privileged identity across unrelated integrations.

For repository operations, reason about the effective permissions on the exact paths and operations used. A read-only consumer should not need write or administrative permissions. Validate access using a representative identity rather than assuming the configuration is correct because the service starts.

### Repository access design

- Prefer explicit, minimal grants over broad subtree access.
- Understand inherited permissions and overlapping groups.
- Keep sensitive or operational content outside public content trees where appropriate.
- Test both intended access and intended denial.
- Re-review permissions when content structure, service behavior, or ownership changes.

Use supported AEM permission-management mechanisms for the target deployment. Do not rely on direct repository edits or undocumented permission shortcuts in production.

## 3. Secure public delivery

The public delivery layer should expose only intended routes and content. Dispatcher filters, web-server routing, CDN policy, application authorization, and repository permissions form different layers of defense; none should be treated as a substitute for the others.

### Dispatcher and CDN

- Follow a deny-by-default filter strategy and allow only required public request patterns.
- Block administrative, repository, debugging, and internal operational endpoints from public access.
- Review allowed methods, selectors, extensions, path patterns, and wildcard behavior.
- Keep edge and origin rules consistent; a blocked path at one layer should not accidentally be reachable through an alternate route.
- Test forbidden paths as well as successful public pages.
- Review cacheability separately from request eligibility.

For practical filter, cache, and invalidation guidance, see the [Dispatcher chapter](../03-dispatcher/README.md).

### Cache and privacy

Do not cache personalized or authenticated responses in a shared cache unless the design explicitly guarantees correct separation. Understand whether cookies, headers, query strings, and identity affect the response. Verify the behavior across browser, CDN, and Dispatcher layers.

A security review should ask whether one user could receive another user's content, whether stale authorization state could be served, and whether sensitive data appears in URLs, logs, or cacheable responses.

## 4. Custom endpoints and application code

Custom servlets, Sling Models, workflows, forms, and integration endpoints are part of the attack surface. Follow platform-supported patterns and enforce authorization at the operation that accesses or changes protected data.

### Input validation

- Treat path parameters, selectors, suffixes, query parameters, headers, uploaded files, and external API responses as untrusted.
- Validate against explicit expected formats, lengths, ranges, and allowed values.
- Prefer allowlists and typed parsing over brittle string checks.
- Enforce payload and upload size limits appropriate to the use case.
- Do not trust client-side validation as a security control.
- Avoid building repository queries or commands by concatenating untrusted strings.

Validation should happen at the boundary and be paired with correct authorization. A syntactically valid input is not necessarily an authorized operation.

### Output encoding and injection

Encode output for its actual context: HTML text, HTML attributes, JavaScript, CSS, and URLs have different rules. Use platform-supported context-aware rendering and escaping mechanisms. Do not assume a value is safe merely because it came from JCR or an internal API; stored content can also contain attacker-controlled data.

For repository queries, use safe query APIs and parameterization where supported. Review selectors, paths, and query shapes to prevent unintended data access and expensive operations. Avoid passing user-controlled data into shell commands, dynamic script evaluation, or unsafe URL fetches.

### Authorization

Authentication answers who the caller is; authorization answers whether that caller may perform this operation on this resource. Enforce authorization server-side for every sensitive read or mutation. Do not rely on a hidden button, unguessable URL, or client-provided role flag.

For custom endpoints, test anonymous, authorized, and unauthorized callers. Confirm that errors do not disclose secrets, repository structure, stack traces, or sensitive object existence beyond the application's intended contract.

### CSRF and browser-based mutations

For browser requests that change state, use the protections required by the platform and application architecture. Review the applicable AEM CSRF mechanism, session behavior, and accepted content types for the endpoint. Do not disable a protection globally to make one integration work; determine why the request is rejected and implement the supported pattern.

### File uploads and downloads

Validate type, size, filename handling, storage destination, and authorization. Do not trust only the filename extension or client-supplied MIME type. Ensure uploaded content cannot be executed or served with unsafe interpretation, and prevent users from downloading assets they are not entitled to access.

## 5. Secrets and integration security

Secrets should not be embedded in source code, content packages, client-side JavaScript, documentation, logs, or error messages.

- Use the approved secret-management mechanism for the environment and deployment model.
- Keep credentials out of Git history; removing a secret from the latest commit does not revoke it.
- Scope tokens to the minimum required APIs, resources, and lifetime where supported.
- Rotate and revoke credentials through a documented process.
- Keep local development credentials separate from shared and production credentials.
- Redact authorization headers, tokens, personal data, and secret-bearing query parameters from logs.
- Prefer supported authentication flows and secure transport for external calls.

For each integration, document the caller identity, target system, data exchanged, authorization scope, timeout, retry policy, error handling, owner, and credential rotation path. A timeout or retry storm can become an availability issue even when authentication is correct.

## 6. Dependency and build-pipeline hygiene

AEM projects inherit risk from Java libraries, frontend packages, build plugins, base images or build environments, and CI/CD actions.

- Track direct and transitive dependencies and review security advisories.
- Prefer maintained versions compatible with the target AEM SDK/runtime and project constraints.
- Pin or constrain versions deliberately; understand the reproducibility and update policy.
- Review provenance and permissions of third-party build actions and scripts.
- Keep CI credentials scoped to the minimum required repositories and operations.
- Protect release branches and require the expected quality/security checks.
- Review dependency updates for compatibility and behavioral changes, not only scanner output.
- Establish a documented process for triage, upgrade, mitigation, and exception expiry.

A scanner finding is a signal to investigate, not a complete risk assessment. Confirm whether the affected dependency and code path are present, reachable, and exploitable in the deployment; record the rationale and remediation date for accepted exceptions.

## 7. AEM 6.5 and AEM as a Cloud Service

Security practices are shared, but the operational controls differ.

### AEM 6.5 and self-managed environments

Depending on the hosting model, the organization may own operating-system patching, JVM/runtime configuration, web-server hardening, network segmentation, log retention, backup protection, and infrastructure access. Define ownership between application, platform, and operations teams.

### AEM as a Cloud Service

Adobe manages more of the underlying platform and infrastructure. Project teams still own application code, content and permission design, Dispatcher configuration, integrations, secrets used by the application, dependency hygiene, and secure use of supported deployment workflows. Do not assume direct host access or traditional operational procedures are available.

In both models, verify the responsibility boundary for the exact service and contract. Use current Adobe guidance for supported security controls and configuration. A managed platform reduces some operational burden; it does not make unsafe application logic or excessive permissions safe.

## 8. Logging, detection, and incident readiness

Logs and metrics should help detect abuse and diagnose failures without becoming a new source of sensitive-data exposure.

- Record useful security events such as authorization failures, suspicious request patterns, and privileged configuration changes where supported.
- Include enough context to correlate requests, but avoid logging credentials, full sensitive payloads, or unnecessary personal data.
- Apply access controls and retention policies to logs and exported diagnostics.
- Alert on meaningful patterns, not every isolated failure.
- Ensure operational staff know where to find relevant evidence in the target hosting model.
- Document escalation, containment, credential revocation, recovery, and post-incident review.

When investigating a suspected incident, preserve relevant evidence, follow the organization's incident process, and avoid ad hoc production changes that destroy forensic context. Contain exposure first when warranted, then establish scope and root cause.

## 9. Security review workflow

For a new feature or material change:

1. Identify data sensitivity, actors, public entry points, and trust boundaries.
2. Identify threats and the impact of unauthorized read, mutation, data disclosure, or denial of service.
3. Review identity, repository permissions, and endpoint authorization.
4. Review input validation, output encoding, query safety, file handling, and error responses.
5. Review Dispatcher/CDN exposure and cache privacy.
6. Review secrets, integration scopes, timeouts, and dependency risks.
7. Add positive and negative tests that demonstrate the intended access boundary.
8. Run supported static analysis, dependency checks, configuration validators, and build checks.
9. Record residual risks, owners, mitigations, and any time-bounded exception.
10. Reassess after deployment and include security signals in operational monitoring.

Security acceptance should be evidence-based. “It works in my browser” is not proof of authorization, safe caching, or correct rejection of malicious input.

## 10. Worked scenario: an integration endpoint exposes data

A custom JSON endpoint was created for a frontend feature. A user reports that changing an identifier in the URL returns another record.

A disciplined response:

1. Treat this as a possible broken object-level authorization issue, not just input validation.
2. Preserve the exact request, response, identity, timestamp, and environment according to incident procedures.
3. Determine whether the endpoint checks authorization for the requested object or trusts the identifier.
4. Restrict exposure and escalate according to the incident severity and data sensitivity.
5. Add server-side authorization checks tied to the caller and target resource.
6. Test anonymous, authorized, unauthorized, and cross-user requests.
7. Review logs and access scope to assess possible historical exposure.
8. Confirm responses and logs do not disclose protected content or sensitive internals.
9. Deploy through the approved release process and validate the fix.
10. Document root cause, impact, follow-up controls, and monitoring.

Do not treat a difficult-to-guess identifier as authorization. Access must be decided by the server using the caller's actual permissions.

## 11. Security change checklist

- [ ] Assets, data sensitivity, actors, and trust boundaries are documented.
- [ ] Human and service identities follow least privilege.
- [ ] Protected operations enforce server-side authorization.
- [ ] Inputs are validated and outputs encoded for their context.
- [ ] Repository queries and external requests are safe and bounded.
- [ ] Public routes are allowlisted and administrative/internal paths are blocked.
- [ ] Cache behavior cannot expose personalized or unauthorized data.
- [ ] Secrets are stored, scoped, rotated, and redacted using approved mechanisms.
- [ ] Dependencies and CI/CD permissions have been reviewed.
- [ ] Positive and negative tests demonstrate the intended security boundary.
- [ ] Relevant logs and alerts support detection without leaking sensitive data.
- [ ] Residual risks have an owner, mitigation, and review date.
- [ ] The controls match the actual AEM version and hosting responsibility model.

## Authoritative references

Use the current documentation for the exact AEM version and deployment. These sources provide implementation-specific details and should be consulted during design and review.

- [AEM 6.5 security checklist](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/security/security-checklist)
- [AEM as a Cloud Service security](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/overview)
- [AEM as a Cloud Service development guidelines](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines)
- [AEM 6.5 user administration and security](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/security/security)
- [Dispatcher overview](https://experienceleague.adobe.com/en/docs/experience-manager-dispatcher/using/dispatcher)
- [AEM as a Cloud Service Dispatcher overview](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)

## Completion criteria

This chapter is ready to be marked published only after technical/editorial review is complete, all links and internal references are checked, the strict documentation build and required CI checks pass, and the live page is verified. Repository presence alone is not release evidence.
