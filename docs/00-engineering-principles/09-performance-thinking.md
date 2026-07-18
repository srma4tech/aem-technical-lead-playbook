# Performance Thinking

## Why this Principle Exists

Performance work consumes engineering time and can add substantial complexity. Without measurement, teams optimize what is visible rather than what limits user outcomes, and may trade away reliability or maintainability for negligible benefit.

## Philosophy

Performance is a requirement expressed through a workload, a percentile or distribution, a resource budget, and a user outcome. Measure before changing, optimize the controlling bottleneck, and verify the whole-system result under realistic conditions.

## Core Ideas

- **Define the outcome:** Translate “fast” into latency, throughput, concurrency, freshness, efficiency, or completion targets.
- **Establish a baseline:** Measure representative behavior with known inputs, environment, and variance.
- **Profile the bottleneck:** Use evidence to identify where time, allocation, I/O, contention, or capacity is consumed.
- **Benchmark responsibly:** Control warm-up, data shape, concurrency, duration, noise, and reproducibility.
- **Cache intentionally:** Define key, lifetime, invalidation, consistency, capacity, and failure behavior.
- **Optimize constraints:** Improve the bottleneck that limits the target outcome, then measure again.
- **Avoid premature optimization:** Preserve simple design until a requirement and evidence justify complexity.

## Engineering Mindset

Treat performance as an end-to-end budget. A faster component does not improve the outcome if another component, queue, dependency, or user interaction dominates. Include cost, reliability, security, and observability effects in the result.

## Real World Examples

1. **Slow request path:** Use traces and profiles to locate the dominant wait before changing algorithms or adding cache layers.
2. **Proposed cache:** Document staleness tolerance, invalidation triggers, memory limits, miss behavior, and how correctness is tested.
3. **Capacity concern:** Model arrival rate, service time, concurrency, saturation, and scaling delay; then validate with a representative load test.

## Common Mistakes

- Reporting averages that hide tail latency or error behavior.
- Benchmarking an isolated function when the system bottleneck is external or concurrent.
- Adding caching without ownership of invalidation and stale-data behavior.
- Optimizing code while making it harder to observe, secure, or change.

## Trade-offs

| Tension                  | Practical position                                                                             |
| ------------------------ | ---------------------------------------------------------------------------------------------- |
| Latency vs consistency   | Choose staleness and coordination behavior from explicit business requirements.                |
| Throughput vs isolation  | Batching and sharing may improve utilization while increasing tail latency or blast radius.    |
| Efficiency vs resilience | Running near saturation reduces cost headroom but increases sensitivity to spikes and failure. |

## Technical Lead Perspective

The lead requires performance budgets and evidence at design time, not only after complaints. They ensure benchmarks are reviewable, performance regressions are observable, capacity has an owner, and optimizations do not quietly weaken correctness or resilience.

## Questions to Ask Yourself

- Which user outcome and percentile are we improving?
- Is the workload representative and the measurement reproducible?
- Where is the actual constraint under expected concurrency?
- What complexity, cost, consistency, or reliability are we trading?

## Checklist

- [ ] A measurable target, workload, percentile, and resource budget exist.
- [ ] Baseline and bottleneck evidence are captured.
- [ ] Benchmark and profiling methods are reproducible.
- [ ] Cache correctness and invalidation are defined where applicable.
- [ ] The end-to-end result and trade-offs are verified after change.

## References

- [Google SRE — Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/)
- [Azure Well-Architected — Reliability Trade-offs](https://learn.microsoft.com/en-us/azure/well-architected/reliability/tradeoffs)
- [AWS — Application Telemetry](https://docs.aws.amazon.com/wellarchitected/latest/operational-excellence-pillar/ops_observability_application_telemetry.html)

## Related Principles

- [Engineering Mindset](01-engineering-mindset.md)
- [Production Ownership](04-production-ownership.md)
- [Code Review Philosophy](08-code-review-philosophy.md)
- [Architecture Decision Records](../architecture/README.md)
- [Architecture decision template](../../templates/architecture-decision-record.md)
- [Architecture review checklist](../../checklists/architecture-review.md)
- [Repository roadmap](../../ROADMAP.md)

## Future Reading

- Workload modeling, capacity planning, and tail latency.
- Profiling, benchmarking, caching, and performance regression controls.
