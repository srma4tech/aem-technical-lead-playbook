# Clean Code

## Why this Principle Exists

Code is read, reviewed, debugged, extended, migrated, and operated far more often than it is initially written. Readability and structural clarity reduce defect risk and make future change cheaper.

## Philosophy

Clean code makes intent and constraints visible with the least necessary complexity. It is not a cosmetic standard and it is not a specific style. The test is whether another qualified engineer can understand, validate, and safely change the behavior without reconstructing hidden assumptions.

## Core Ideas

- **Readability:** Optimize for the next reader's correct understanding.
- **Naming:** Use domain language and names that reveal role, unit, boundary, and side effect.
- **Simplicity:** Choose the smallest design that meets current requirements and preserves required safety.
- **Single responsibility:** Group behavior around one reason to change, not an arbitrary line count.
- **SOLID as prompts:** Use design principles to examine cohesion, substitution, interfaces, dependencies, and change—not as mandatory class patterns.
- **Avoid clever code:** Reject compression or novelty that hides control flow, invariants, or failure behavior.
- **Consistency:** Follow established local patterns unless changing them produces a documented improvement.
- **Code smells:** Treat duplication, long parameter lists, hidden state, shotgun changes, and unclear ownership as investigation signals.
- **Refactoring:** Improve structure in small, behavior-preserving steps protected by tests.

## Engineering Mindset

Read code from the perspective of a maintainer under time pressure. Make state transitions, side effects, invariants, errors, and units explicit. Prefer a boring structure that can be tested and replaced over an elegant abstraction whose value is speculative.

## Real World Examples

1. **Unclear naming:** Replace generic names such as `data` or `process` with domain-specific names that reveal what is represented and what changes.
2. **Growing conditional logic:** First characterize behavior with tests, then separate policy from mechanics in small refactoring steps.
3. **Proposed shared abstraction:** Require at least a stable common concept and compatible change reasons; similar-looking code alone is insufficient.

## Common Mistakes

- Equating clean code with formatting while ignoring behavior, coupling, and failure handling.
- Creating interfaces, factories, or layers before variation exists.
- Applying single responsibility at such a small scale that behavior becomes fragmented.
- Combining a structural rewrite with a behavior change that cannot be reviewed independently.

## Trade-offs

| Tension                    | Practical position                                                                     |
| -------------------------- | -------------------------------------------------------------------------------------- |
| Duplication vs abstraction | Tolerate small duplication until the shared concept and change pattern are understood. |
| Explicitness vs brevity    | Prefer explicit intent and failure behavior over fewer lines.                          |
| Consistency vs improvement | Improve local conventions deliberately; do not create a second style casually.         |

## Technical Lead Perspective

A lead defines code health as a maintained system property. They fund incremental refactoring, ensure tests protect behavior, challenge speculative abstractions, and distinguish blocking correctness concerns from optional style preferences during review.

## Questions to Ask Yourself

- Can a reader identify intent, invariants, and side effects without external explanation?
- Does each abstraction have a stable responsibility and a real consumer?
- Which future change is this design making easier, and is that change plausible?
- Can structural improvement be separated from behavior change?

## Checklist

- [ ] Names express domain intent, units, ownership, and side effects.
- [ ] Control flow and error handling are easy to trace.
- [ ] Responsibilities and dependencies are cohesive.
- [ ] Tests protect important behavior before refactoring.
- [ ] Complexity has a current, documented reason.

## References

- [Google — What to Look for in a Code Review](https://google.github.io/eng-practices/review/reviewer/looking-for.html)
- [Agile Manifesto — Principles](https://agilemanifesto.org/principles.html)
- [Google SRE — Simplicity](https://sre.google/sre-book/simplicity/)

## Related Principles

- [Code Review Philosophy](08-code-review-philosophy.md)
- [Engineering Excellence](06-engineering-excellence.md)
- [Technical Lead Principles](02-technical-lead-principles.md)
- [Architecture Decision Records](../architecture/README.md)
- [Architecture decision template](../../templates/architecture-decision-record.md)
- [Architecture review checklist](../../checklists/architecture-review.md)
- [Repository roadmap](../../ROADMAP.md)

## Future Reading

- Characterization testing and safe refactoring patterns.
- Modularity, cohesion, coupling, and evolutionary architecture.
