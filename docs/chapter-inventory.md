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

### Scaffold-only chapter directories

Each of the following directories currently contains only its landing-page `README.md`; the substantive chapter content still needs to be written.

- **AEM Core** — write platform architecture and fundamentals.
- **Dispatcher** — cover request flow, filters, caching, invalidation, security, and operations.
- **Performance** — write measurement-first performance engineering guidance.
- **Security** — cover AEM-specific security and secure delivery.
- **AEM as a Cloud Service** — cover cloud architecture, deployment, configuration, and operations.
- **Enterprise Java** — cover Java runtime, concurrency, integration, testing, and resilience.
- **Design Patterns** — add context-driven patterns, anti-patterns, and trade-offs.
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
4. **Performance and Security:** develop as separate reviewable milestones.
5. **AEM as a Cloud Service:** connect platform architecture to cloud deployment and operations.
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
