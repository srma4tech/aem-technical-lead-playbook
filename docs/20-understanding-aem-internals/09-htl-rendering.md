# HTL Rendering

## Overview

HTL is AEM's server-side templating language. It renders trusted component structure while context-aware escaping protects output from common injection mistakes.

## Why this Matters

Templates sit on every page-render path. A small data-access or context mistake can become a site-wide latency or security problem.

## Learning Objectives

- Explain HTL's role in component rendering.
- Use models and services to keep templates declarative.
- Recognize escaping and context boundaries.

## Architecture Overview

```mermaid
flowchart LR
  R[Resource] --> M[Sling Model]
  M --> T[HTL template]
  T --> E[Escaped HTML response]
```

## Internal Working

HTL evaluates expressions against bindings, models, and use objects, then emits HTML. The engine applies context-aware escaping; explicit context overrides require a documented security reason.

## Request Flow

A component resolves its resource, adapts to its model, reads required services or content, and renders its HTL. Includes repeat this pattern for child components.

## Production Behaviour

Rendering cost compounds across a page tree. A model that performs a query per component can be harmless in author preview and expensive on publish.

## Performance

Keep templates presentation-focused, batch content access, avoid expensive getters called repeatedly, and measure render time by component.

## Security

Rely on default escaping. Never mark author or request input as safe HTML without sanitization and a threat-model review.

## Debugging

Check model adaptation, component resource type, error logs, and output context. Distinguish absent data from an expression escaping issue.

## Common Mistakes

- Performing repository queries directly from presentation logic.
- Disabling escaping to make markup appear.
- Treating model getters as cost-free.

## Best Practices

Use Sling Models for view data, small reusable templates, and component-level performance tests.

## Design Trade-offs

Server rendering improves initial delivery and author integration but couples page latency to backend work. Client rendering can defer work but complicates cacheability and accessibility.

## Technical Lead Notes

Establish a component performance budget and require explicit review for unsafe context declarations.

## Production Story

A search component ran the same query once per result card. Moving aggregation into one model call reduced publish CPU and made the page cacheable again.

## Interview Readiness

### Developer Questions

Why does HTL escape output by default?

### Senior Questions

How can a model getter create a rendering hotspot?

### Technical Lead Questions

Which component metrics would you collect?

### Adobe Style Questions

What should live in HTL versus a Sling Model?

### Scenario Based Questions

Dynamic markup does not render. What is your safe investigation path?

### Architecture Questions

How do you preserve cacheability with dynamic component data?

## References

- [HTL Specification](https://experienceleague.adobe.com/en/docs/experience-manager-htl/content/specification)

## Cross References

- [Script Resolution](08-script-resolution.md)
- [How AEM Works](01-how-aem-works.md)
