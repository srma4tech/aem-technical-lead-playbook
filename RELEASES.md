# Release Strategy

## Overview

The project uses Semantic Versioning: `MAJOR.MINOR.PATCH`. Before v1.0, minor releases represent planned handbook milestones and patch releases contain compatible corrections or foundation improvements.

## Version Roadmap

| Version | Milestone                   | Intended outcome                                                            |
| ------- | --------------------------- | --------------------------------------------------------------------------- |
| v0.1    | Repository Foundation       | Governance, platform, automation, templates, and placeholders               |
| v0.2    | AEM Core                    | Reviewed AEM core learning foundation                                       |
| v0.3    | Understanding AEM Internals | Request lifecycle, Sling, Oak, HTL, caching, and production troubleshooting |
| v0.4    | Dispatcher                  | Reviewed Dispatcher handbook                                                |
| v0.5    | Performance                 | Performance engineering guidance                                            |
| v0.6    | Security                    | Security engineering guidance                                               |
| v0.7    | AEMaaCS                     | AEM as a Cloud Service guidance                                             |
| v0.8    | Enterprise Java             | Enterprise Java guidance                                                    |
| v0.9    | Technical Leadership        | Technical-leadership handbook                                               |
| v1.0    | Public Release              | Stable, reviewed public handbook                                            |

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

## Release evidence audit — 2026-10-24

The repository tag reference list contains only `v0.1.1` and `v0.2.0`. Both annotated tags point to the same commit (`c78d41c1ddf7038034a5e99b35f0aac72a508217`). The GitHub Releases list is empty. Therefore, the changelog entry for `v0.3.0` and the former dashboard claim of `v0.4.0` are not currently backed by corresponding tag and GitHub Release records.

Do not retarget existing tags or create a release retroactively just to make dashboard numbers agree. First decide the intended release boundary, validate the exact commit with required CI and a strict MkDocs build, verify the Pages deployment and live site, then create a new annotated tag and matching GitHub Release through a GitHub write action. The available repository connection in this workflow can inspect tags and releases but does not expose tag-creation or release-creation actions, so those write operations remain outstanding.
