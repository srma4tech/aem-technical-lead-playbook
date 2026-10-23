# Design Patterns: Context, Trade-offs, and Maintainability

## Purpose and audience

Design patterns describe recurring solutions to recurring design problems. They are useful when they clarify responsibilities, isolate change, or make behavior easier to test. They are harmful when introduced by habit, when a simple function would suffice, or when they hide control flow behind layers of indirection.

This chapter is for senior engineers and technical leads designing Java, AEM, and enterprise integration systems. The goal is judgment: understand the problem, select the smallest useful pattern, and evaluate the cost it introduces.

## Learning outcomes

- Recognize common creational, structural, and behavioral patterns.
- Select patterns based on variation points and quality attributes.
- Identify over-abstraction and pattern misuse.
- Apply patterns to AEM components, services, integrations, and tests.
- Explain the trade-offs and operational consequences of a design.

## 1. Start with the problem, not the pattern

Before proposing a pattern, describe the problem in plain language. What varies? What must remain stable? Who owns the behavior? What failure mode or maintenance cost are we trying to reduce?

A useful pattern should improve at least one of these:
- clarity of responsibility;
- isolation of change;
- testability;
- control over resource lifecycle;
- correctness under concurrency or failure;
- ability to add a genuine new behavior without rewriting existing code.

It will usually introduce costs too: more types, more indirection, additional configuration, harder debugging, or a wider public API. Make those costs explicit in the review.

## 2. Strategy: vary behavior behind a stable contract

Use Strategy when a caller needs to select one of several interchangeable behaviors without embedding every variation in a large conditional.

**Example:** An integration layer can choose a pricing policy for different product classes. The caller depends on a pricing-policy contract; each implementation owns one policy.

Good fit:
- the alternatives are real and expected to evolve;
- they share a meaningful contract;
- selection is explicit and testable;
- adding a strategy does not require modifying unrelated code.

Poor fit:
- there are only two trivial branches unlikely to change;
- the interface merely renames one method without reducing coupling;
- behavior is selected through hidden global state.

In AEM, a strategy can isolate external-system variants or content transformation rules. Keep OSGi service selection and configuration understandable; do not make service ranking or implicit lookup the only place where business policy is visible.

## 3. Adapter: isolate an external contract

Adapter converts one interface into another expected by the application. It is useful when integrating a vendor API, legacy service, or platform API whose types and errors should not leak through the domain.

An adapter should own:
- request/response mapping;
- protocol and authentication details;
- timeout configuration;
- transport-level error translation;
- response validation and safe logging.

The application layer should own the business decision about retry, fallback, or rejection. Avoid an adapter that silently returns empty success data whenever the vendor API fails.

**Trade-off:** The adapter adds a layer, but it can make vendor replacement, contract testing, and error handling much clearer. Do not build a generic integration framework for a single endpoint unless a real reuse need exists.

## 4. Decorator: add a responsibility without changing the core

Decorator wraps an object implementing the same contract to add behavior such as metrics, tracing, caching, or policy enforcement.

Use it when the added behavior should compose around an existing implementation and remain independently testable. Ensure the wrapper preserves the original contract, including exceptions, timing semantics, and resource cleanup.

Risks:
- decorators can stack into an opaque call chain;
- ordering can change behavior;
- hidden caching can introduce stale data or security leakage;
- metrics wrappers can accidentally record sensitive values.

Document composition order and expose enough observability to identify which layer produced a result.

## 5. Factory: centralize complex creation

Factory is useful when object creation requires non-trivial selection, validation, or construction logic. It can hide concrete classes from callers and centralize a creation policy.

Avoid factories that simply call a constructor with no additional value. Avoid global factories that become service locators, hiding dependencies and making tests difficult.

In managed frameworks, prefer the framework's supported dependency-injection and lifecycle model rather than manually constructing objects whose lifecycle belongs to the container.

## 6. Builder: make complex construction readable

Builder is useful when an object has many optional parameters, validation rules, or construction stages. It can make call sites more readable than a long constructor or a boolean-heavy parameter list.

Do not add a builder to a simple immutable value with two required fields unless the builder improves the actual call sites. Builders should validate invariants before producing an object; they should not create partially valid objects that fail much later.

## 7. Template Method and composition

Template Method defines a stable algorithm with selected steps overridden by subclasses. It can be useful when the sequence is invariant and variation is genuinely limited to a few steps.

Its risk is inheritance coupling: subclasses depend on protected hooks and base-class lifecycle. Changes to the base algorithm can have surprising effects.

Prefer composition when behavior needs to vary independently, multiple dimensions of behavior exist, or testing individual collaborators is simpler. Inheritance remains useful for a clear subtype relationship with stable substitutability; it should not be the default reuse mechanism.

## 8. Observer and event-driven design

Observer or publish-subscribe patterns separate the event producer from interested consumers. They can reduce direct coupling, but they also make execution order, failure propagation, and end-to-end behavior less obvious.

Before using events, decide:
- Is delivery synchronous or asynchronous?
- Can events be duplicated, delayed, or reordered?
- What happens when a consumer fails?
- Is retry supported, and is the handler idempotent?
- How are event schemas versioned?
- How can operators trace an event through the system?

For AEM workflows and repository events, use platform-supported mechanisms and understand their lifecycle and execution semantics. Do not assume that an event means every consumer completed successfully, or that an in-process event provides durable delivery.

## 9. Repository and service patterns in AEM

Patterns must respect AEM's lifecycle and security model.

- Keep Sling Models focused on adapting and exposing data, not on becoming hidden orchestration layers.
- Put reusable business behavior in appropriately scoped OSGi services.
- Inject dependencies explicitly and use supported Declarative Services patterns.
- Keep request-specific data out of singleton service fields.
- Use request-bound repository access for user-scoped work where appropriate.
- Use dedicated service identities with least privilege for system operations.
- Keep resource resolver and session lifetimes bounded.
- Avoid a generic repository service that exposes arbitrary paths and operations to every caller.
- Keep external API adapters separate from repository and rendering responsibilities.

A pattern is not safe simply because it is common in Java. Verify that it fits AEM's framework-managed lifecycle and the security requirements of the operation.

## 10. Anti-patterns and warning signs

### God object

One service handles validation, persistence, remote calls, transformation, caching, and presentation. Split responsibilities along real policy and change boundaries, not arbitrary line counts.

### Service locator

A class looks up arbitrary dependencies at runtime rather than declaring what it needs. This hides coupling and makes tests less explicit. Prefer constructor or framework-managed dependency injection unless the runtime genuinely requires dynamic lookup.

### Singleton with mutable request state

A singleton service stores the current user, request, or temporary result in fields. Concurrent requests can overwrite each other's state. Keep request state local or pass it explicitly.

### Abstraction for hypothetical reuse

A generic framework is introduced before a second real use case exists. Start with a clear implementation and extract a shared abstraction when repeated behavior or a stable variation point is demonstrated.

### Boolean parameter explosion

A method accepts several booleans that select different behavior combinations. Replace unclear combinations with named operations, an options object, or separate strategies when those states represent real domain concepts.

### Catch, log, and continue

A failure is logged and then hidden behind an empty result. The caller cannot distinguish no data from a failed operation. Preserve the error contract and make fallback behavior explicit.

### Distributed monolith

Services are split across network boundaries without clear ownership or independent scaling needs. The result adds latency, retries, deployment coordination, and partial failures without enough benefit. Separate processes when a real boundary justifies the cost.

## 11. Pattern selection decision guide

Ask these questions in order:

1. Is there a recurring problem or a credible requirement for variation?
2. Can a straightforward function, conditional, or composition solve it clearly?
3. Which responsibility or dependency becomes easier to change?
4. Does the pattern improve tests or correctness in a measurable way?
5. What extra types, configuration, indirection, or operational failure modes does it add?
6. Can a new team member trace behavior from entry point to result?
7. Is the abstraction aligned with the framework lifecycle and security model?
8. What evidence would tell us the design is no longer helping?

If the benefit cannot be stated clearly, defer the abstraction.

## 12. Worked scenario: multiple downstream providers

A product application currently calls one inventory provider. A second provider is being added for a different market, and each provider has different request formats and error codes.

A reasonable design:
1. Define a narrow inventory capability in application language.
2. Implement one adapter per provider, each owning protocol mapping and error translation.
3. Select the provider through explicit market or configuration policy.
4. Keep timeout and retry policy visible and bounded.
5. Validate provider responses before returning domain data.
6. Add contract tests for each adapter and unit tests for provider selection.
7. Test timeout, malformed responses, authorization failures, and duplicate mutation requests.
8. Instrument each provider separately without logging credentials or sensitive payloads.
9. Avoid adding a generic plugin framework until there is evidence of additional provider complexity.
10. Document how a new provider is added and what compatibility it must satisfy.

The useful design is not “use Strategy because there are two providers.” The useful design is that the variation point is explicit, transport details are isolated, and failure behavior is testable.

## 13. Design review checklist

- [ ] The problem and variation point are stated without naming a pattern first.
- [ ] The chosen pattern reduces a real coupling, correctness, or maintenance problem.
- [ ] Simpler alternatives were considered.
- [ ] Contracts, errors, lifecycle, and ownership remain explicit.
- [ ] The abstraction is not hiding global state or service lookup.
- [ ] Concurrency and resource behavior are safe.
- [ ] AEM framework lifecycle and access-control expectations are respected.
- [ ] Tests cover the contract and meaningful failure modes.
- [ ] Operational behavior remains traceable.
- [ ] The added complexity has a clear owner and reason to exist.

## Authoritative references

- [Refactoring.Guru design patterns](https://refactoring.guru/design-patterns)
- [Java SE documentation](https://docs.oracle.com/en/java/javase/)
- [AEM as a Cloud Service development guidelines](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines)
- [AEM 6.5 JCR integration best practices](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/bestpractices/jcr-integration)
- [OSGi Declarative Services specification](https://docs.osgi.org/specification/osgi.cmpn/7.0.0/service.component.html)

## Completion criteria

This chapter is ready to be marked published only after technical/editorial review, link validation, strict documentation build, required CI checks, and live-site verification. Repository presence and a successful build alone are not release evidence.
