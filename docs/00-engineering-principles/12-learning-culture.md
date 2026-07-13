# Learning Culture

## Why this Principle Exists

Engineering knowledge decays as systems, teams, and constraints change. Teams that depend on informal experts repeat mistakes and make safe change harder. A learning culture converts individual experience into shared capability.

## Philosophy

Learning is part of delivery, not an extracurricular activity. It should address real capability gaps, produce observable application, and leave reusable evidence through code, documentation, review, teaching, or improved mechanisms.

## Core Ideas

- **Read deliberately:** Use books, standards, specifications, and primary documentation to build durable models.
- **Engage with open source:** Study decisions, tests, issues, reviews, and maintenance practices—not only usage examples.
- **Read code:** Trace behavior, boundaries, tests, and history in systems that matter to current work.
- **Write code:** Use bounded exercises and production work to test understanding.
- **Write documentation:** Expose gaps by explaining a concept, decision, or procedure for another engineer.
- **Teach others:** Use review, pairing, talks, and mentoring to test transfer and broaden ownership.
- **Learn from operations:** Turn incidents, near misses, and delivery friction into changes in practice.

## Engineering Mindset

Start from a capability needed by the team or system. Define how improved capability will appear in a decision, implementation, review, or operation. Create short feedback cycles and share artifacts so learning survives role and team changes.

## Real World Examples

1. **New system area:** Read its design and incident history, trace one request or workflow, make a small reviewed change, and update the missing documentation.
2. **Recurring review issue:** Run a focused workshop using real anonymized examples, update the checklist, and inspect later reviews for change.
3. **Conference or book learning:** Require a short applicability note or experiment rather than distributing summaries without action.

## Common Mistakes

- Collecting courses or certifications without applying the capability.
- Treating one expert as the permanent answer path.
- Reading only simplified secondary material when a specification or source is available.
- Running lessons learned sessions without changing a backlog, standard, test, or document.

## Trade-offs

| Tension                           | Practical position                                                                                 |
| --------------------------------- | -------------------------------------------------------------------------------------------------- |
| Breadth vs depth                  | Build enough shared breadth for collaboration and deliberate depth around owned risks.             |
| Learning time vs delivery         | Integrate learning into current work and reserve explicit capacity for high-value capability gaps. |
| Standard curriculum vs local need | Maintain foundations while adapting practice to system and role context.                           |

## Technical Lead Perspective

The lead maps capability risk, creates opportunities to apply learning, rotates ownership, and rewards documentation and teaching as delivery contributions. They avoid using learning as a substitute for staffing, clear design, or access to experienced review.

## Questions to Ask Yourself

- Which team or system risk will this learning reduce?
- How will the capability be applied and reviewed?
- What artifact or mechanism will make the learning reusable?
- Which knowledge is concentrated in one person?

## Checklist

- [ ] The learning objective is tied to a real capability need.
- [ ] Primary sources and working systems are included.
- [ ] Application includes feedback from a qualified reviewer.
- [ ] Knowledge is captured in code, documentation, or a repeatable practice.
- [ ] Ownership and learning outcomes are reviewed over time.

## References

- [Google Engineering Practices — Code Review](https://google.github.io/eng-practices/review/)
- [Google SRE — Postmortem Culture](https://sre.google/sre-book/postmortem-culture/)
- [AWS — Operate and Improve](https://docs.aws.amazon.com/wellarchitected/latest/operational-excellence-pillar/operate.html)

## Related Principles

- [Documentation Culture](07-documentation-culture.md)
- [Mentorship](13-mentorship.md)
- [Career Growth](14-career-growth.md)
- [Architecture Decision Records](../architecture/README.md)
- [Architecture decision template](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/templates/architecture-decision-record.md)
- [Architecture review checklist](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/checklists/architecture-review.md)
- [Repository roadmap](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/ROADMAP.md)

## Future Reading

- Capability mapping, deliberate practice, and technical learning plans.
- Open-source contribution and internal knowledge-system design.
