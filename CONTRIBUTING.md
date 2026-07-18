# Contributing

## Overview

Contributions are welcome across repository architecture, editorial quality, templates, automation, and technical content. New contributors should begin with [FIRST_CONTRIBUTION.md](FIRST_CONTRIBUTION.md).

## Before You Start

1. Review the [roadmap](ROADMAP.md), [governance model](GOVERNANCE.md), and existing issues.
2. Open an issue or discussion for substantial content, navigation, tooling, policy, or architectural changes.
3. Never include proprietary source code, credentials, customer data, or confidential employer information.
4. Follow the [style guide](STYLE_GUIDE.md) and [code of conduct](CODE_OF_CONDUCT.md).

## Development Setup

- Python 3.12 or newer
- Node.js 22 or newer
- `pip install -r requirements-docs.txt`
- `npm install`

Run `npm run check` and `mkdocs build --strict` before opening a pull request.

## Contribution Workflow

1. Create a focused branch from the default branch.
2. Make one cohesive change with clear commit messages.
3. Add or update navigation and indexes when introducing a publishable page.
4. Complete the [documentation review checklist](docs/checklists/documentation-review.md).
5. Validate formatting, spelling, links, and the strict MkDocs build.
6. Complete the pull request template and link the relevant issue or discussion.

## Content Acceptance Criteria

Content must have a defined audience and learning objective, distinguish fact from opinion, cite authoritative sources, state version and deployment assumptions, address production and security implications where relevant, and avoid unsupported product claims.

## Architecture and Decision Records

Substantial, cross-cutting, or difficult-to-reverse repository decisions require an ADR under `docs/architecture/`. Follow the established ADR structure and link the proposal to its issue or discussion.

## Review and Governance

Maintainers may request technical, editorial, security, accessibility, or legal review. Authors do not approve their own substantial changes. Approval does not imply endorsement by Adobe or any contributor's employer. See [GOVERNANCE.md](GOVERNANCE.md).

## Releases

The project follows Semantic Versioning and the process in [RELEASES.md](RELEASES.md). Update [CHANGELOG.md](CHANGELOG.md) when a change is notable to users or contributors.

## Reporting Problems

Use the matching issue template. Report vulnerabilities according to [SECURITY.md](SECURITY.md), not through public issues.
