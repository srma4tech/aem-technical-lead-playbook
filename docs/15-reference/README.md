# Engineering Reference Library

## Purpose and curation policy

This page is a curated starting point for authoritative engineering material referenced throughout the playbook. Prefer official product documentation, standards, and maintained primary sources. Community articles can help explain a concept, but they should not replace official documentation for version-sensitive behavior, security guidance, or platform support.

A reference should have a clear purpose and be linked from the relevant chapter. Before relying on it, check that it applies to the product version, deployment model, and date in question.

## AEM and Adobe Experience Cloud

- [AEM as a Cloud Service documentation](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service) — current cloud platform guidance, development, operations, and migration.
- [AEM 6.5 documentation](https://experienceleague.adobe.com/en/docs/experience-manager-65) — self-managed AEM 6.5 behavior and implementation guidance.
- [AEM as a Cloud Service architecture](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture) — architecture and operating model.
- [AEM as a Cloud Service development guidelines](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines) — supported development practices and constraints.
- [Cloud Manager overview](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-manager/content/introduction) — CI/CD and cloud delivery.
- [AEM Dispatcher documentation](https://experienceleague.adobe.com/en/docs/experience-manager-dispatcher/using/dispatcher) — caching and request-filtering behavior.
- [Dispatcher configuration](https://experienceleague.adobe.com/en/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration) — configuration reference.
- [Dispatcher on AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview) — cloud-specific delivery architecture.
- [AEM 6.5 JCR integration best practices](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/bestpractices/jcr-integration) — repository access and query guidance.
- [AEM security development guidance](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/introduction/security) — security principles and implementation considerations.

Always distinguish AEM 6.5 guidance from AEM as a Cloud Service guidance. Similar concepts do not guarantee identical APIs, configuration, deployment permissions, or operational procedures.

## Java and JVM

- [Java SE documentation](https://docs.oracle.com/en/java/javase/) — language, libraries, and platform references.
- [Java concurrency tutorial](https://docs.oracle.com/javase/tutorial/essential/concurrency/) — introductory concurrency concepts.
- [ExecutorService API](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/ExecutorService.html) — executor lifecycle and task execution contracts.
- [CompletableFuture API](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/CompletableFuture.html) — asynchronous composition and completion behavior.
- [Java Language Specification](https://docs.oracle.com/javase/specs/) — formal language behavior.

Use API documentation matching the Java version supported by the target runtime. A newer API may be unavailable in the deployed environment.

## Software architecture and system design

- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) — quality attributes and architecture review questions.
- [Google Cloud Architecture Framework](https://cloud.google.com/architecture/framework) — operational excellence, security, reliability, performance, and cost.
- [Microsoft Azure Well-Architected Framework](https://learn.microsoft.com/en-us/azure/well-architected/) — design principles and workload review.
- [Architecture Decision Records](https://adr.github.io/) — ADR formats and community guidance.
- [Refactoring.Guru design patterns](https://refactoring.guru/design-patterns) — accessible pattern explanations; use as a learning aid, not as a rule that every pattern should be applied.

Architecture frameworks are prompts for analysis, not universal prescriptions. Adapt them to the actual requirements, platform constraints, and operating model.

## Reliability and production engineering

- [Google Site Reliability Engineering book](https://sre.google/sre-book/table-of-contents/) — reliability concepts, monitoring, incident response, and operations.
- [Google Site Reliability Workbook](https://sre.google/workbook/table-of-contents/) — practical exercises and operational methods.
- [Google Engineering Practices: code review](https://google.github.io/eng-practices/review/) — review principles and reviewer/author expectations.

Use incident and reliability practices that fit the organization's service ownership, severity model, and regulatory needs. Do not copy numerical objectives without validating their applicability.

## Security and secure development

- [OWASP Top 10](https://owasp.org/www-project-top-ten/) — awareness of major web application risk categories.
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/) — practical implementation guidance.
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework) — risk-management framework.
- [Secure by Design guidance from the US Cybersecurity and Infrastructure Security Agency](https://www.cisa.gov/securebydesign) — product security principles.

For AEM-specific access control, Dispatcher exposure, repository permissions, and cloud configuration, prioritize the relevant Adobe documentation alongside general security standards.

## Documentation, versioning, and project governance

- [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) — structuring user-facing change history.
- [Semantic Versioning](https://semver.org/) — version numbering rules.
- [GitHub Pages documentation](https://docs.github.com/en/pages) — publication sources and deployment behavior.
- [GitHub Actions documentation](https://docs.github.com/en/actions) — CI workflow configuration and run history.
- [MkDocs documentation](https://www.mkdocs.org/) — documentation build and configuration.
- [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) — theme features and supported extensions.

A release tag is evidence of a version reference, not proof that the deployed site is healthy. Confirm the release commit, build result, deployment workflow, and live content independently.

## How to evaluate a source

Before citing a claim, ask:

1. Is the source authoritative for the product or standard?
2. Does it apply to the correct product version and deployment model?
3. Is the page current or explicitly historical?
4. Does it directly support the claim, or only a related concept?
5. Is the claim security-sensitive or likely to change?
6. Can the reader reach the source without authentication or a broken redirect?
7. Should the playbook state a caveat or point to more than one version-specific source?

For high-impact claims, prefer a primary source and record the tested version or retrieval date in the relevant chapter when useful.

## Reference maintenance

- Remove links that are obsolete or no longer support the linked claim.
- Replace broad landing-page links with a more precise official page when one is available.
- Check external links during scheduled maintenance and before releases.
- Keep source links near the claims they support, not only in this index.
- Do not treat a link-check pass as proof of technical correctness.
- Record known access restrictions or version limitations.
- Use the repository's link-check workflow and review failures rather than blindly removing all failing URLs.

## Related resources

- [Chapter Inventory and Definition of Done](../chapter-inventory.md)
- [Documentation Review Checklist](../checklists/documentation-review.md)
- [Engineering Checklists](../12-checklists/README.md)
- [Release Strategy](../../RELEASES.md)
- [Roadmap](../ROADMAP.md)

## Completion criteria

This reference library is ready to be marked published only after link validation, strict documentation build, required CI checks, editorial review, and live-site verification. Each reference should support a clear learning or engineering need.
