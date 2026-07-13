# Release Strategy

## Overview

The project uses Semantic Versioning: `MAJOR.MINOR.PATCH`. Before v1.0, minor releases represent planned handbook milestones and patch releases contain compatible corrections or foundation improvements.

## Version Roadmap

| Version | Milestone             | Intended outcome                                              |
| ------- | --------------------- | ------------------------------------------------------------- |
| v0.1    | Repository Foundation | Governance, platform, automation, templates, and placeholders |
| v0.2    | AEM Core              | Reviewed AEM core learning foundation                         |
| v0.3    | Dispatcher            | Reviewed Dispatcher handbook                                  |
| v0.4    | Performance           | Performance engineering guidance                              |
| v0.5    | Security              | Security engineering guidance                                 |
| v0.6    | AEMaaCS               | AEM as a Cloud Service guidance                               |
| v0.7    | Enterprise Java       | Enterprise Java guidance                                      |
| v0.8    | Technical Leadership  | Technical-leadership handbook                                 |
| v0.9    | Interview Handbook    | Interview preparation and review                              |
| v1.0    | Public Release        | Stable, reviewed public handbook                              |

## Release Policy

- **Major:** incompatible information-architecture or governance changes after v1.0.
- **Minor:** backward-compatible content areas, capabilities, or planned milestones.
- **Patch:** corrections, editorial improvements, dependency maintenance, and compatible automation fixes.

## Release Process

1. Confirm the milestone acceptance criteria and required reviews.
2. Run all quality and strict site-build checks.
3. Update the changelog and version references.
4. Create an annotated Semantic Versioning tag and GitHub release.
5. Publish GitHub Pages and verify the release.
