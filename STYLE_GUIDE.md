# Style Guide

## Writing Tone

Write with calm authority, empathy, and precision. Prefer direct explanations over marketing language, jokes, idioms, or unnecessary jargon. Address the reader consistently and distinguish requirements, recommendations, examples, and opinions.

## Terminology

Use official Adobe and Java terminology where available. Define acronyms on first use and use the glossary for project-wide terms. Write “AEM as a Cloud Service” on first use; use “AEMaaCS” only afterward. Do not invent abbreviations for one page.

## Capitalization

Use sentence case for headings and table headers. Preserve official product, API, class, protocol, and standard names. Capitalize job roles only when they are part of a formal title.

## Markdown Rules

- Use CommonMark-compatible Markdown with GitHub extensions.
- Use one level-one heading matching the page title.
- Do not skip heading levels.
- Keep blank lines around headings, lists, tables, and fenced blocks.
- Use ordered lists for sequences and unordered lists for non-sequential items.
- Keep tables focused; use prose when a table would become difficult to scan.
- Use admonitions only when the message benefits from explicit emphasis.

## Code Formatting

Use fenced code blocks with an explicit language identifier. Keep examples minimal and runnable where practical. Explain important inputs, outputs, assumptions, and risks outside the block. Never include real credentials, customer data, proprietary code, or unsafe defaults.

Use backticks for commands, paths, identifiers, configuration keys, and short code fragments. Do not use code formatting for ordinary emphasis.

## Image Guidelines

Use images only when they materially improve understanding. Prefer SVG for diagrams and WebP or PNG for screenshots. Include meaningful alternative text and, when needed, a caption explaining the insight. Store publishable assets under `docs/assets/images/` and editable diagram sources under `diagrams/`.

## Mermaid Standards

Use fenced `mermaid` blocks and accessible surrounding prose. Use concise node labels, consistent direction, and a small color palette. Prefer `flowchart LR` for broad relationships and top-to-bottom flows for sequences. Avoid encoding meaning through color alone. Complex or reused sources belong under `diagrams/`.

## Screenshot Naming

Use lowercase kebab-case with the feature and state, such as `cloud-manager-pipeline-failed.webp`. Do not include dates unless the date is meaningful. Remove account names, URLs, tokens, customer identifiers, and unrelated browser chrome before committing.

## Folder Naming

Use lowercase kebab-case. Number only stable top-level learning sections. Avoid duplicate topic trees and ambiguous abbreviations. Add a README when a directory needs scope, ownership, or navigation guidance.

The checklist areas have distinct purposes:

- `docs/checklists/` contains contributor and editorial quality gates.
- `docs/12-checklists/` is reserved for future handbook learning content.
- `checklists/` contains reusable engineering delivery controls.

## Cross-Linking Rules

Use relative links for repository content and canonical HTTPS links for external sources. Use descriptive link text rather than “click here.” Link to the nearest relevant page, avoid circular “related topic” chains, and update both navigation and indexes when introducing a published page. Do not link to private, session-bound, or redirect URLs.

## Writing Standards

Write clear international English. Scope technical claims by product version and deployment model where relevant. Separate facts from recommendations, cite authoritative sources, and state assumptions and trade-offs. Do not imply official Adobe endorsement.

## Standard Learning Page

Use these sections when relevant: Overview, Why this Matters, Learning Objectives, Key Concepts, Architecture, Best Practices, Common Mistakes, Production Notes, Performance, Security, Interview Questions, References, Related Topics, and TODO.

## Documentation Review Checklist

Before requesting review:

- [ ] The audience, purpose, and learning objectives are clear.
- [ ] Grammar, terminology, capitalization, and tone follow this guide.
- [ ] Technical claims are accurate, scoped, and source-backed.
- [ ] Architecture, performance, security, and production implications are addressed.
- [ ] Code, images, screenshots, and Mermaid diagrams are safe and accessible.
- [ ] Cross-links, navigation, references, and anchors are correct.
- [ ] Formatting, markdownlint, spelling, link checks, and the strict MkDocs build pass.
- [ ] No confidential, proprietary, customer, or credential material is present.

Use the complete [documentation review checklist](docs/checklists/documentation-review.md) for formal reviews.
