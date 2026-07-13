# Engineering Mindset

## Why this Principle Exists

Engineering decisions fail when they optimize a visible component while ignoring the wider system, operating environment, or business outcome. A shared mindset gives teams a consistent way to reason when requirements are incomplete and no procedure supplies the answer.

## Philosophy

Start from the system and the outcome, not from a preferred implementation. Model how value flows, where uncertainty sits, how failure propagates, and who will operate the result. Treat every design as a hypothesis that must survive evidence from delivery and production.

## Core Ideas

- **Think in systems:** Map dependencies, feedback loops, ownership boundaries, data flows, and failure propagation.
- **Protect maintainability:** Count comprehension, testing, upgrade, migration, and operational effort as real lifecycle cost.
- **Connect to business value:** Translate technical work into risk reduction, revenue protection, cost, learning, or customer outcomes.
- **Manage technical debt:** Record the obligation, consequence, trigger, and repayment strategy instead of using debt as a vague complaint.
- **Seek root causes:** Ask which system conditions made an outcome likely; do not stop at the last human action.
- **Measure before optimizing:** Establish a baseline, target, and bottleneck before changing the system.
- **Automate repeatable work:** Automate stable, understood procedures with guardrails and observable failure.
- **Design for observation:** Make important behavior diagnosable through meaningful signals tied to user and business outcomes.
- **Engineer reliability:** Define acceptable failure, recovery expectations, and degradation behavior before incidents define them.

## Engineering Mindset

Move between three views: the immediate change, the surrounding system, and the lifecycle over time. A locally correct choice can still be globally harmful through coupling, cognitive load, operational toil, or delayed risk. Make these second-order effects part of the decision.

## Real World Examples

1. **Slow delivery:** Before adding people or tools, map waiting time, hand-offs, rework, failure demand, and approval queues; improve the constraint rather than every activity.
2. **Technical debt proposal:** Describe the incidents, change cost, or delivery delay caused by the debt and define a measurable exit condition.
3. **Automation request:** Standardize the manual process and its failure handling first, then automate it with auditability and a safe stop.

## Common Mistakes

- Starting with a framework, pattern, or rewrite before agreeing on the problem.
- Calling all old code debt or all new code improvement.
- Automating an unstable process and multiplying its failure rate.
- Collecting telemetry without defining a decision that the signal will support.

## Trade-offs

| Tension                               | Practical position                                                                                                         |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Local simplicity vs system simplicity | Prefer the option that reduces total cognitive and operating burden, even if one component becomes slightly more explicit. |
| Short-term delivery vs lifecycle cost | Borrow deliberately, record the obligation, and set a repayment trigger.                                                   |
| Standardization vs autonomy           | Standardize interfaces and safety controls; preserve local choice where it does not create system cost.                    |

## Technical Lead Perspective

The lead keeps the system boundary wide enough. They connect product priorities to engineering consequences, make debt and risk visible, and prevent urgent local work from accumulating invisible operational obligations. They also narrow the boundary when analysis stops producing a better decision.

## Questions to Ask Yourself

- What system are we actually changing, including people and operations?
- Where is the current constraint, and what evidence identifies it?
- Which second-order effects will appear after adoption or scale?
- What are we choosing not to optimize?

## Checklist

- [ ] The business or customer outcome is measurable.
- [ ] Dependencies, owners, feedback loops, and failure paths are mapped.
- [ ] Lifecycle and operational costs are included.
- [ ] Optimization work has a baseline and target.
- [ ] Debt and assumptions have owners and review triggers.

## References

- [AWS — Operate workloads](https://docs.aws.amazon.com/wellarchitected/latest/operational-excellence-pillar/operate.html)
- [Google SRE — Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/)
- [Azure Well-Architected — Reliability Trade-offs](https://learn.microsoft.com/en-us/azure/well-architected/reliability/tradeoffs)

## Related Principles

- [Decision Making](05-decision-making.md)
- [Production Ownership](04-production-ownership.md)
- [Performance Thinking](09-performance-thinking.md)
- [Architecture Decision Records](../architecture/README.md)
- [Architecture decision template](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/templates/architecture-decision-record.md)
- [Architecture review checklist](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/checklists/architecture-review.md)
- [Repository roadmap](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/ROADMAP.md)

## Future Reading

- Constraint analysis and value-stream mapping.
- Socio-technical system design and resilience engineering.
