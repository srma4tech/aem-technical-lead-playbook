# Response Generation

## Overview

Response generation turns a resolved resource and handler output into status, headers, cookies, and bytes. Correctness includes cache directives and error behavior, not only HTML.

## Why this Matters

Headers control browser, CDN, and Dispatcher behavior. A response committed too early cannot safely change its security or cache policy.

## Learning Objectives

- Identify response ownership and commit points.
- Design cache, error, and security headers.
- Avoid partial and inconsistent output.

## Architecture Overview

```mermaid
flowchart LR
  H[Handler] --> S[Status and headers]
  S --> B[Body rendering]
  B --> C[Response committed]
  C --> E[Edge cache/client]
```

## Internal Working

Servlets and scripts write through the Sling response. Once buffering is exhausted or output is flushed, headers and status are committed. Error handlers may render a different resource, subject to the same filters and permissions.

## Request Flow

Set status and headers before streaming the body; then confirm what Apache and Dispatcher forward or cache.

## Production Behaviour

Error pages must be fast, cache-safe, and independent of the failing feature. Cache directives determine whether failure or success is amplified at the edge.

## Performance

Stream large safe responses, avoid building large strings in memory, and set content type and encoding explicitly.

## Security

Apply security headers consistently and never reflect untrusted input into headers. Cookies need deliberate Secure, HttpOnly, and SameSite policy.

## Debugging

Compare origin and public headers with the same Host and request variants. Check whether a cached response predates a header change.

## Common Mistakes

- Writing body content before setting status.
- Caching error or user-specific responses accidentally.
- Returning stack traces to clients.

## Best Practices

Centralize shared header policy, test error responses, and document cache directives as endpoint contracts.

## Design Trade-offs

Aggressive caching protects origin but makes rollback and personalization harder. Detailed errors aid support but expose implementation information.

## Technical Lead Notes

Define a response policy matrix for public pages, APIs, authenticated views, redirects, and errors. Monitor status distribution and cache headers.

## Production Story

A maintenance response was cached for hours after recovery because its cache-control header matched normal content. A dedicated no-store error policy prevented repeat incidents.

## Interview Readiness

### Developer Questions

When are response headers committed?

### Senior Questions

How do headers affect Dispatcher behavior?

### Technical Lead Questions

What response contract belongs in platform standards?

### Adobe Style Questions

How can Sling render an error response?

### Scenario Based Questions

Users see old headers after deployment. What layers do you check?

### Architecture Questions

How do you design cache-safe error handling?

## References

- [HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110)

## Cross References

- [Request Lifecycle](02-request-lifecycle.md)
- [Dispatcher Overview](03-dispatcher-overview.md)
