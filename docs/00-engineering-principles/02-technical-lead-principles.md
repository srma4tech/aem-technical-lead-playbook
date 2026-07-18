# Technical Lead Principles

## Why this Principle Exists

Teams need technical direction without creating a single decision bottleneck. The technical lead role exists to improve the quality and speed of team decisions, manage cross-cutting risk, and build the capability of others.

## Philosophy

Technical leadership is a responsibility for clarity, alignment, and engineering outcomes—not a claim to make every decision. A lead establishes context, creates mechanisms, delegates at the right level, and intervenes where blast radius or irreversibility requires broader judgment.

## Core Ideas

- **Own outcomes:** Accept responsibility for technical health and delivery consequences across team boundaries.
- **Communicate context:** Explain goals, constraints, decision drivers, and what has changed.
- **Review architecture:** Test assumptions, boundaries, failure modes, operations, and evolution—not diagram aesthetics.
- **Improve code review:** Set review standards, resolve deadlock, and keep the codebase healthier without demanding perfection.
- **Mentor through responsibility:** Delegate meaningful decisions with support and feedback.
- **Decide transparently:** Record consequential choices, dissent, evidence, and review conditions.
- **Handle ambiguity:** Separate unknowns, assumptions, constraints, and decisions; run bounded discovery where needed.
- **Manage risk:** Prioritize by impact, likelihood, detectability, reversibility, and time sensitivity.
- **Build trust:** Make commitments explicit, surface problems early, and apply standards consistently.
- **Shape culture:** Turn values into review behavior, operating routines, and recognition.

## Engineering Mindset

Ask what the team needs in order to make the next responsible decision without you. Provide context and constraints before solutions. Escalate attention when consequences cross boundaries, but push reversible local decisions toward the closest informed owner.

## Real World Examples

1. **Ambiguous initiative:** The lead defines decision deadlines, identifies high-risk unknowns, assigns discovery owners, and keeps low-risk delivery moving.
2. **Architecture disagreement:** The lead restates shared drivers, requests evidence, records alternatives, and uses the agreed decision process rather than authority alone.
3. **Repeated review delays:** The lead clarifies blocking versus optional feedback, assigns suitable reviewers, and measures review flow.

## Common Mistakes

- Becoming the default implementer, reviewer, incident responder, and decision maker.
- Providing solutions without the context that would let others reason independently.
- Using architecture review as late approval rather than early risk reduction.
- Protecting the team from difficult information until options have narrowed.

## Trade-offs

| Tension                       | Practical position                                                                                 |
| ----------------------------- | -------------------------------------------------------------------------------------------------- |
| Delegation vs control         | Delegate reversible decisions; increase review depth with blast radius and irreversibility.        |
| Consensus vs timeliness       | Seek informed consensus, but use a named decision owner and deadline when delay has material cost. |
| Technical quality vs delivery | Negotiate scope and sequencing while preserving non-negotiable safety and integrity controls.      |

## Technical Lead Perspective

The lead is accountable for the decision environment. Useful evidence includes fewer surprise escalations, clearer ownership, smaller decision latency, healthier reviews, more independent senior contributors, and controlled production risk. Personal code volume is not the primary measure.

## Questions to Ask Yourself

- Am I creating clarity or becoming a dependency?
- Who is the closest informed owner for this decision?
- Which risk requires my attention now, and which can be delegated?
- Have affected people understood the decision and its consequences?

## Checklist

- [ ] Goals, constraints, roles, and decision rights are explicit.
- [ ] High-risk assumptions have validation owners.
- [ ] Architecture and code reviews occur early enough to change direction.
- [ ] Decisions and exceptions are recorded.
- [ ] Delegation includes context, authority, support, and feedback.

## References

- [Google Engineering Practices — Code Review](https://google.github.io/eng-practices/review/)
- [Google — The Standard of Code Review](https://google.github.io/eng-practices/review/reviewer/standard.html)
- [AWS — Operational Readiness Review Mechanisms](https://docs.aws.amazon.com/wellarchitected/latest/operational-readiness-reviews/building-mechanisms.html)

## Related Principles

- [Decision Making](05-decision-making.md)
- [Mentorship](13-mentorship.md)
- [Engineering Values](15-engineering-values.md)
- [Architecture Decision Records](../architecture/README.md)
- [Architecture decision template](../../templates/architecture-decision-record.md)
- [Architecture review checklist](../../checklists/architecture-review.md)
- [Repository roadmap](../../ROADMAP.md)

## Future Reading

- Architecture facilitation and stakeholder communication.
- Team topology, decision rights, and cross-team technical governance.
