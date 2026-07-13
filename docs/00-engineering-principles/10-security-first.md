# Security First

## Why this Principle Exists

Security defects can invalidate correctness, expose users, and constrain recovery. Controls added late are expensive, inconsistent, and easy to bypass. Security must shape architecture, implementation, delivery, and operations from the start.

## Philosophy

Assume inputs, identities, networks, dependencies, and operating environments can fail or be abused. Minimize privilege and exposed capability, choose secure defaults, validate at trust boundaries, and layer controls so one failure does not determine the outcome.

## Core Ideas

- **Least privilege:** Grant the minimum capability, scope, and duration required.
- **Secure defaults:** Deny or fail safely unless access and behavior are explicitly allowed.
- **Defense in depth:** Use independent preventive, detective, and responsive controls across layers.
- **Secrets management:** Keep secrets out of code, logs, artifacts, and unmanaged configuration; rotate and audit access.
- **Authentication:** Establish identity with mechanisms appropriate to the threat and assurance level.
- **Authorization:** Check permission for the specific action and resource on every trusted path.
- **Input validation:** Validate syntax, semantics, size, encoding, and authorization at the boundary that owns the contract.
- **Supply-chain awareness:** Inventory, verify, update, and constrain dependencies and build inputs.

## Engineering Mindset

Start with assets, actors, trust boundaries, abuse cases, and recovery. Treat security requirements as observable behavior with tests and operational ownership. Prefer designs that remove capability over controls that depend on every caller behaving correctly.

## Real World Examples

1. **New service identity:** Create a dedicated identity with narrowly scoped permissions, expiry or rotation, and auditable use instead of sharing a broad credential.
2. **Input processing:** Constrain size and format before expensive work, validate business rules, encode output for its destination, and reject safely.
3. **Emergency access:** Use time-bound, approved, logged escalation with explicit revocation and follow-up review.

## Common Mistakes

- Treating authentication as authorization.
- Storing secrets in source, logs, screenshots, examples, or long-lived local files.
- Validating only in a user interface while trusted services accept unchecked input.
- Disabling security controls during incidents without time bounds, approval, audit, and restoration verification.

## Trade-offs

| Tension                   | Practical position                                                                                          |
| ------------------------- | ----------------------------------------------------------------------------------------------------------- |
| Security vs usability     | Reduce unnecessary friction, but do not weaken required assurance; improve the mechanism and recovery path. |
| Granularity vs complexity | Use the narrowest manageable permissions and automate policy verification.                                  |
| Detection vs privacy      | Collect sufficient security evidence while minimizing sensitive content and retention.                      |

## Technical Lead Perspective

The lead ensures threat and trust-boundary review happens early, security ownership is explicit, and exceptions are time-bound. They route specialist review, protect remediation priority, and verify that operational recovery preserves security controls.

## Questions to Ask Yourself

- What assets and actions require protection, and from which actors?
- Where are trust boundaries and authorization decisions enforced?
- What happens when identity, policy, validation, or a dependency fails?
- Can credentials or sensitive data appear in logs, artifacts, or support workflows?

## Checklist

- [ ] Assets, actors, trust boundaries, and abuse cases are documented.
- [ ] Authentication and authorization are separate and tested.
- [ ] Privileges and secrets are scoped, rotated, and auditable.
- [ ] Inputs and outputs are validated at owned boundaries.
- [ ] Security controls, dependencies, alerts, and incident recovery have owners.

## References

- [NIST Secure Software Development Framework](https://csrc.nist.gov/projects/ssdf)
- [OWASP Developer Guide — Security Principles](https://devguide.owasp.org/en/02-foundations/03-security-principles/)
- [OWASP — Least Privilege Principle](https://owasp.org/www-community/controls/Least_Privilege_Principle)

## Related Principles

- [Production Ownership](04-production-ownership.md)
- [Code Review Philosophy](08-code-review-philosophy.md)
- [Engineering Values](15-engineering-values.md)
- [Architecture Decision Records](../architecture/README.md)
- [Architecture decision template](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/templates/architecture-decision-record.md)
- [Architecture review checklist](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/checklists/architecture-review.md)
- [Repository roadmap](https://github.com/srma4tech/aem-technical-lead-playbook/blob/main/ROADMAP.md)

## Future Reading

- Threat modeling, secure delivery, dependency governance, and secrets lifecycle.
- Identity architecture, audit design, and security incident response.
