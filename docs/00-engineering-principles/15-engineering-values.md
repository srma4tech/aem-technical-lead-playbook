# Engineering Values

## Why this Principle Exists

Under pressure, teams reveal the standards they actually use. Explicit values provide a basis for decisions and accountability when schedule, hierarchy, uncertainty, and incentives pull in different directions.

## Philosophy

Values are observable commitments, not personality traits or branding. A useful value changes how work is planned, reviewed, operated, and corrected. When values conflict, engineers explain the trade-off and protect user, public, and professional obligations.

## Core Ideas

- **Integrity:** Report evidence, uncertainty, mistakes, and risk accurately, even when inconvenient.
- **Ownership:** Carry outcomes through design, delivery, operation, learning, and hand-off.
- **Curiosity:** Test assumptions and seek mechanisms rather than accepting the first explanation.
- **Continuous learning:** Update practices when evidence changes and make learning reusable.
- **Respect:** Critique work without diminishing people; include affected expertise and perspectives.
- **Quality:** Build correctness, security, reliability, accessibility, and maintainability into the work.
- **Customer focus:** Understand user outcomes and protect trust rather than optimizing internal activity.
- **Craftsmanship:** Apply disciplined care proportionate to risk and lifecycle, without pursuing perfection for its own sake.

## Engineering Mindset

Translate every stated value into behavior and mechanism. Integrity appears in risk reporting and incident records. Ownership appears in runbooks and corrective actions. Respect appears in review comments and decision participation. Quality appears in acceptance criteria and production evidence.

## Real World Examples

1. **Schedule pressure:** Surface the consequence, reduce scope, and preserve essential safety rather than reporting false confidence.
2. **Design disagreement:** Represent the other option accurately, evaluate shared drivers, record dissent, and support the decided path until review evidence changes.
3. **Personal mistake:** Disclose it early, reduce impact, preserve evidence, and improve the system conditions that made recurrence possible.

## Common Mistakes

- Using values to judge personality instead of specific behavior and impact.
- Rewarding visible heroics while ignoring prevention, documentation, and mentoring.
- Claiming customer focus to bypass security, privacy, accessibility, or maintainability.
- Treating craftsmanship as unlimited polish disconnected from risk and value.

## Trade-offs

| Tension                           | Practical position                                                                                       |
| --------------------------------- | -------------------------------------------------------------------------------------------------------- |
| Transparency vs confidentiality   | Share decision-relevant truth with the right audience while protecting legitimate sensitive information. |
| Customer urgency vs system health | Reduce scope and stage delivery; do not transfer hidden risk to users or operators.                      |
| Craftsmanship vs pragmatism       | Apply rigor according to consequence and reversibility, and record deliberate compromises.               |

## Technical Lead Perspective

The lead aligns incentives with stated values. They reward early risk disclosure, preventive work, clear documentation, constructive review, and growth of others. They address violations consistently, including when short-term delivery results appear positive.

## Questions to Ask Yourself

- What behavior would demonstrate this value in the current decision?
- Which incentive is pushing us away from the stated standard?
- Are we reporting uncertainty and impact accurately?
- Would we defend this decision to affected users and future maintainers?

## Checklist

- [ ] Risks, evidence, uncertainty, and mistakes are reported accurately.
- [ ] Customer and public impact are included in priorities.
- [ ] Review and decision behavior remains respectful and evidence-led.
- [ ] Quality compromises are explicit, owned, and time-bound.
- [ ] Recognition includes prevention, operations, documentation, and mentoring.

## References

- [ACM Code of Ethics](https://www.acm.org/code-of-ethics)
- [Google — Writing Review Comments](https://google.github.io/eng-practices/review/reviewer/comments.html)
- [NIST Secure Software Development Framework](https://csrc.nist.gov/projects/ssdf)

## Related Principles

- [Engineering Principles](README.md)
- [Technical Lead Principles](02-technical-lead-principles.md)
- [Production Ownership](04-production-ownership.md)
- [Architecture Decision Records](../architecture/README.md)
- [Architecture decision template](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/templates/architecture-decision-record.md)
- [Architecture review checklist](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/checklists/architecture-review.md)
- [Repository roadmap](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/ROADMAP.md)

## Future Reading

- Professional ethics in architecture and operations.
- Inclusive engineering decisions, accessibility, privacy, and responsible technology.
