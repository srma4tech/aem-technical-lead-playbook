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

| Area | Observed structure | Baseline status | Next action |
| --- | --- | --- | --- |
| Introduction | Learner-facing introduction and start guide | Published; updated in PR #34 | Keep links and onboarding guidance current |
| Engineering Principles | Overview plus 15 topic chapters | Published; audit required | Spot-check links, accuracy, and cross-links during final editorial review |
| Learning Roadmap | Role-based paths, sequence, and practice checkpoints | Published; updated in PR #34 | Keep planned vs published areas explicit |
| Understanding AEM Internals | Overview plus 15 chapters | Published; audit required | Review technical accuracy and source coverage before v1.0 |
| AEM Core | One landing-page README in its chapter directory | Scaffold only | Write substantive core architecture content |
| Dispatcher | One landing-page README in its chapter directory | Scaffold only | Write request flow, filters, caching, invalidation, security, and operations content |
| Performance | One landing-page README in its chapter directory | Scaffold only | Write measurement-first performance engineering content |
| Security | One landing-page README in its chapter directory | Scaffold only | Write AEM-specific security and secure delivery content |
| AEM as a Cloud Service | One landing-page README in its chapter directory | Scaffold only | Write cloud architecture, deployment, configuration, and operations content |
| Enterprise Java | One landing-page README in its chapter directory | Scaffold only | Write Java runtime, concurrency, integration, testing, and resilience content |
| Design Patterns | One landing-page README in its chapter directory | Scaffold only | Add context-driven patterns, anti-patterns, and trade-offs |
| System Design | One landing-page README in its chapter directory | Scaffold only | Add requirements, architecture, quality attributes, and worked designs |
| Technical Leadership | One landing-page README in its chapter directory | Scaffold only | Add leadership practices, decision records, reviews, and delivery governance |
| Production Support | One landing-page README in its chapter directory | Scaffold only | Add incident response, diagnostics, mitigation, recovery, and postmortems |
| Engineering Checklists | One landing-page README in its chapter directory; separate contributor checklists exist elsewhere | Audit required | Inventory existing checklists and consolidate around real decisions |
| Interview Guide | One landing-page README in its chapter directory | Scaffold only | Add role/topic map, scenario questions, model answers, and evaluation rubrics |
| Case Studies | One landing-page README in its chapter directory | Scaffold only | Add evidence-based, anonymized scenarios with outcomes and lessons |
| Reference | One landing-page README in its chapter directory | Scaffold only | Curate authoritative sources and map them to relevant chapters |

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
