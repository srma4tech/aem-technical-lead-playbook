# Chapter Inventory and Completion Criteria

This inventory is the working baseline for finishing the AEM Technical Lead Playbook. It records observable repository structure, not an estimated percentage. A directory or landing-page README does not count as a completed chapter by itself.

## Status definitions

- **Published** — substantive content is available in the documentation site and has passed the applicable release checks.
- **Scaffold only** — the directory contains a landing-page outline, but the intended learning material is not yet present in that chapter directory.
- **Audit required** — files exist, but their technical depth, references, examples, and review evidence have not yet been assessed.
- **Planned** — work has not started or has no accepted implementation yet.
- **In review** — a scoped change is under PR review and must not be counted as complete until required checks pass and it is merged.

A chapter can be marked Published only after meeting the [definition of done](#chapter-definition-of-done).

## Observed inventory

This inventory was compiled from the current `main` branch file tree on 2026-10-23. File presence and approximate byte sizes were inspected; this is a structural baseline, not a full technical correctness review of every published page.

### Published or curated

- **Introduction** — learner-facing introduction and start guide. Published; updated in PR #34. Keep links and onboarding guidance current.
- **Engineering Principles** — overview plus 15 topic chapters. Published; audit required. Spot-check links, accuracy, and cross-links during final editorial review.
- **Learning Roadmap** — role-based paths, sequence, and practice checkpoints. Published; updated in PR #34. Keep planned versus published areas explicit.
- **Understanding AEM Internals** — overview plus 15 chapters. Published; audit required. Review technical accuracy and source coverage before v1.0.

### Substantive content merged; final publication review pending

- **AEM Core** — substantive architecture chapter merged in PR #37, including platform layers, content modelling, component/service boundaries, JCR/Oak, security, performance, AEM 6.5 versus Cloud Service considerations, a worked integration scenario, and Adobe references. Verify the live site and complete the editorial/source audit before marking the milestone complete.
- **Dispatcher** — substantive engineering chapter merged in PR #39. Covers request flow, filter security, cache eligibility, invalidation, CDN interactions, AEM as a Cloud Service differences, troubleshooting, a worked 403 scenario, and Adobe references. Verify the live site and complete the editorial/source audit before marking the milestone complete.
- **Performance** — substantive engineering chapter merged in PR #41. Covers measurable objectives, test design, percentiles and saturation, bottleneck isolation across delivery/application/repository/dependency layers, optimization trade-offs, AEM as a Cloud Service caveats, incident workflow, and regression prevention. Verify the live site and complete the editorial/source audit before marking the milestone complete.
- **Security** — substantive engineering chapter merged in PR #43. Covers threat modelling, least privilege, repository permissions, public delivery, custom endpoint safety, secrets, dependency hygiene, incident readiness, and security review. Verify the live site and complete the editorial/source audit before marking the milestone complete.
- **AEM as a Cloud Service** — substantive engineering chapter merged in PR #44. Covers service tiers and ownership boundaries, Cloud Manager delivery, configuration, repository design, delivery caching, performance, integrations, migration, troubleshooting, and go-live readiness. Verify the live site and complete the editorial/source audit before marking the milestone complete.
- **Enterprise Java** — substantive engineering chapter merged in PR #46. Covers service boundaries, collections, error contracts, concurrency, ExecutorService, CompletableFuture, resilience, integration design, observability, testing, and AEM-specific lifecycle constraints. Verify the live site and complete the editorial/source audit before marking the milestone complete.
- **Design Patterns** — substantive engineering chapter merged in PR #48. Covers context-driven pattern selection, Strategy, Adapter, Decorator, Factory, Builder, Template Method, Observer, AEM-specific patterns, anti-patterns, and review criteria. Verify the live site and complete the editorial/source audit before marking the milestone complete.

### Scaffold-only chapter directories

Each of the following directories currently contains only its landing-page `README.md`; the substantive chapter content still needs to be written.

- **System Design** — add requirements, architecture, quality attributes, and worked designs.
- **Technical Leadership** — add leadership practices, decision records, reviews, and delivery governance.
- **Production Support** — add incident response, diagnostics, mitigation, recovery, and postmortems.
- **Interview Guide** — add a role/topic map, scenario questions, model answers, and evaluation rubrics.
- **Case Studies** — add evidence-based, anonymized scenarios with outcomes and lessons.
- **Reference** — curate authoritative sources and map them to relevant chapters.

### Requires a deeper inventory

- **Engineering Checklists** — the chapter directory currently has a landing-page README; separate contributor checklists exist elsewhere. Inventory the existing checklists and consolidate around real decisions.

## Chapter definition of done

A chapter is not complete merely because its README exists. The author and reviewer should verify:

- [ ] Clear audience, prerequisites where applicable, and measurable learning outcomes.
- [ ] Substantive explanation of the topic, not just a mission statement or list of planned topics.
- [ ] Accurate diagrams or examples where they improve understanding.
- [ ] Practical scenarios, failure modes, and troubleshooting guidance where relevant.
- [ ] Security, performance, reliability, and operational trade-offs where relevant.
- [ ] Version/deployment caveats for AEM 6.5 versus AEM as a Cloud Service where applicable.
- [ ] Authoritative references for version-sensitive or security-sensitive claims.
- [ ] Working internal links and links to related learning paths.
- [ ] Technical and editorial review completed.
- [ ] Markdown lint, Prettier, spell check, link check, and strict MkDocs build pass.
- [ ] Release/dashboard metadata updated after merge, not before.

## Delivery order

1. **Baseline and governance:** resolve release metadata conflict; define the editorial review and source policy.
2. **AEM Core:** establish platform fundamentals before deeper delivery chapters.
3. **Dispatcher:** build on request lifecycle and delivery boundaries.
4. **Performance and Security:** substantive content for both chapters is merged; complete their publication and editorial review.
5. **AEM as a Cloud Service:** substantive content is merged; complete publication review and verify the live page.
6. **Enterprise practice:** Enterprise Java, Design Patterns, System Design, and Production Support.
7. **Leadership and learner tools:** Technical Leadership, Checklists, Interview Guide, Case Studies, and Reference.
8. **Public readiness:** technical/editorial audit, accessibility, navigation, release evidence, and v1.0 acceptance.

## Release metadata discrepancy

The Engineering Dashboard currently records `v0.4.0` and an overall progress estimate, while `CHANGELOG.md` and the Release Dashboard identify `v0.3.0` as the latest documented release. The project should verify Git tags and GitHub release evidence before selecting a canonical current version. Until then, progress percentages remain unverified and are not used as a delivery metric.

## Maintenance rules

- Update this inventory in the same PR as meaningful chapter-status changes, or in a dedicated status-reconciliation PR.
- Use **In review** for work awaiting checks/review and change it to **Published** only after merge and release validation.
- Never infer content completion from a folder, navigation entry, or planned roadmap milestone.
- Report progress as counts of chapters meeting the definition of done until a defensible weighting model is approved.
