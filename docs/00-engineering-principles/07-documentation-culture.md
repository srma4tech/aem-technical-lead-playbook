# Documentation Culture

## Why this Principle Exists

Undocumented knowledge becomes an availability risk, a decision bottleneck, and a repeated discovery cost. Documentation creates durable context for people who were not present when a system or decision was created.

## Philosophy

Documentation is part of the engineering system, not a report produced after delivery. Useful documentation answers a decision or operating need, has an owner, lives close to change, and can be reviewed and validated like code.

## Core Ideas

- **Docs as code:** Version, review, lint, link-check, and publish documentation through the delivery workflow.
- **Architecture documentation:** Describe context, boundaries, flows, constraints, risks, and quality attributes.
- **Knowledge sharing:** Write for the next operator, reviewer, contributor, and decision maker.
- **Living documentation:** Connect updates to code, interface, operational, and ownership changes.
- **Architecture Decision Records:** Preserve consequential decisions, drivers, alternatives, and consequences.
- **Purpose-driven artifacts:** Create runbooks, references, designs, and tutorials for distinct reader tasks.

## Engineering Mindset

Start with the reader's decision. State scope, assumptions, owner, and freshness signals. Prefer a small current page over a comprehensive stale document. Make the authoritative location obvious and delete or redirect competing copies.

## Real World Examples

1. **Architecture change:** Update the design view and ADR in the same pull request as the implementation boundary change.
2. **Incident learning:** Convert a corrective insight into a runbook, alert description, test, or design constraint where future work will encounter it.
3. **Repeated support question:** Improve the canonical page and link responders to it instead of creating another informal explanation.

## Common Mistakes

- Writing a document to satisfy a gate without identifying a reader or decision.
- Copying the same facts into multiple pages with no source of truth.
- Leaving decisions only in meetings, chat, tickets, or review comments.
- Measuring documentation by page count rather than task success and freshness.

## Trade-offs

| Tension                          | Practical position                                                                            |
| -------------------------------- | --------------------------------------------------------------------------------------------- |
| Completeness vs currency         | Document the critical path and explicit unknowns first; expand only with ownership.           |
| Centralization vs proximity      | Centralize discovery and standards while keeping change-sensitive detail close to its source. |
| Automation vs editorial judgment | Automate structure and mechanical checks; review accuracy, usefulness, and risk with people.  |

## Technical Lead Perspective

The lead defines which artifacts are required for which decisions, assigns ownership, and ensures documentation changes accompany system changes. They use ADRs to preserve rationale and treat documentation gaps discovered during incidents or onboarding as engineering work.

## Questions to Ask Yourself

- Who will use this document, for what decision, and under what conditions?
- Where is the source of truth, and what competing copy should be removed?
- What change should trigger review of this page?
- Can a new operator act safely using this information?

## Checklist

- [ ] Audience, purpose, scope, owner, and assumptions are clear.
- [ ] Decision rationale and alternatives are preserved where consequential.
- [ ] Links and navigation lead to one authoritative source.
- [ ] Operational steps include safety, validation, and escalation.
- [ ] Documentation is reviewed in the same workflow as related change.

## References

- [Google Engineering Practices — Documentation in Review](https://google.github.io/eng-practices/review/)
- [Google SRE — Postmortem Culture](https://sre.google/sre-book/postmortem-culture/)
- [NIST Secure Software Development Framework](https://csrc.nist.gov/projects/ssdf)

## Related Principles

- [Decision Making](05-decision-making.md)
- [Production Ownership](04-production-ownership.md)
- [Learning Culture](12-learning-culture.md)
- [Architecture Decision Records](../architecture/README.md)
- [Architecture decision template](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/templates/architecture-decision-record.md)
- [Architecture review checklist](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/checklists/architecture-review.md)
- [Repository roadmap](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/ROADMAP.md)

## Future Reading

- Documentation architecture, content lifecycle, and ownership automation.
- System context diagrams, operational runbooks, and decision-record governance.
