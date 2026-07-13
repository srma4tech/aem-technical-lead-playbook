# Project Board

## Purpose

Use a GitHub Project to make work visible from early exploration through release. Each item should have an owner, acceptance criteria, priority, and target milestone when known.

## Recommended Columns

| Column      | Entry criteria                              | Exit criteria                                |
| ----------- | ------------------------------------------- | -------------------------------------------- |
| Ideas       | Unvalidated suggestion or opportunity       | Triaged for backlog, future, or closure      |
| Backlog     | Accepted work without immediate capacity    | Prioritized and sufficiently defined         |
| Ready       | Scoped, unblocked, and ready to start       | Assigned and actively being worked           |
| In Progress | An owner is implementing the work           | Pull request or review artifact is available |
| Review      | Technical or editorial review is active     | Required approvals and changes are complete  |
| Testing     | Automated and manual validation is underway | Acceptance criteria and quality gates pass   |
| Released    | Included in the default branch or a release | Retained for reporting and release notes     |
| Future      | Valuable but outside the active horizon     | Reconsidered during roadmap review           |

## Operating Rules

- Limit work in progress and keep item status current.
- Use labels for domain, priority, and work type; do not encode status in labels.
- Link pull requests, ADRs, discussions, and releases to their project items.
- Review Ready, Blocked, and stale In Progress items during maintainer triage.
