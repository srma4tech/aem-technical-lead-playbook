# Sling Engine

## Overview

Sling is AEM's request-processing engine. It interprets a request as a resource plus request metadata, then resolves a handler using resource type, selectors, extension, and method.

## Why this Matters

Sling explains AEM's content-driven programming model. Teams that understand it avoid fragile URL switches and diagnose resolution failures from evidence.

## Learning Objectives

- Describe Sling's resource-first routing model.
- Place filters, servlets, scripts, and models in the handler chain.
- Distinguish resource resolution from handler resolution.

## Architecture Overview

```mermaid
flowchart LR
  Q[Request] --> P[Request path parsing]
  P --> R[Resource resolver]
  R --> T[Resource type]
  T --> H[Servlet or script resolver]
  H --> F[Sling filters]
  F --> O[Response]
```

## Internal Working

Sling first asks a `ResourceResolver` for a resource. It then derives the resource type, including inheritance through `sling:resourceSuperType`, and uses request metadata to choose a servlet or script. Filters can wrap request processing before or after the handler.

## Request Flow

The resource path is only one input. For a reproducible test, capture method, selectors in order, extension, suffix, resource type, and authentication context.

## Production Behaviour

Resolution is predictable when content types and registrations are explicit. Ambiguous registrations become deployment-order and maintenance risks.

## Performance

Avoid handler logic that repeatedly adapts or resolves the same content. Cache only request-safe computed values and keep component dependency graphs shallow.

## Security

Handlers run with the request or service identity depending on their access pattern. Validate input and keep privileged repository work behind narrowly scoped services.

## Debugging

Use Sling request logs, resource inspection, servlet resolver diagnostics, and targeted logging of request path info. Do not infer a handler from file names alone.

## Common Mistakes

- Confusing a resource path with a script path.
- Registering servlets too broadly by path.
- Hiding authorization in a rendering branch.

## Best Practices

Use resource-type servlet registrations where appropriate, explicit selector contracts, and small filters with clear ordering.

## Design Trade-offs

Resource-type routing is flexible and reusable, but content modeling errors can surface as runtime rendering errors. Path-bound endpoints are direct but more tightly coupled to URLs.

## Technical Lead Notes

Set conventions for resource types, selectors, and endpoint ownership. During design review, ask what happens when a component is overlaid or a supertype changes.

## Production Story

A component started using an unexpected script after an overlay introduced a matching resource type. Making the intended `sling:resourceType` explicit and removing the broad overlay restored deterministic resolution.

## Interview Readiness

### Developer Questions

What does Sling resolve before it resolves a script?

### Senior Questions

How can a resource supertype change rendering behavior?

### Technical Lead Questions

How would you govern selector namespaces across teams?

### Adobe Style Questions

What is the difference between a Sling resource and a JCR node?

### Scenario Based Questions

Why could a deployed servlet be ignored?

### Architecture Questions

When is a path servlet preferable to a resource-type servlet?

## References

- [Sling Engine](https://sling.apache.org/documentation/the-sling-engine/)

## Cross References

- [How AEM Works](01-how-aem-works.md)
