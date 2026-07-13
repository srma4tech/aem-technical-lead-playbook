# First Contribution

## Before You Begin

Read the [contribution guide](CONTRIBUTING.md), [style guide](STYLE_GUIDE.md), and [code of conduct](CODE_OF_CONDUCT.md). Choose a scoped issue, preferably one labeled `good first issue`, and confirm that it is unassigned.

## Fork

Use GitHub's **Fork** action to create a copy under your account.

## Clone

```bash
git clone https://github.com/<your-github-username>/aem-technical-lead-playbook.git
cd aem-technical-lead-playbook
git remote add upstream https://github.com/srma4tech/aem-technical-lead-playbook.git
```

## Branch

Synchronize the default branch and create a focused branch.

```bash
git fetch upstream
git switch main
git merge --ff-only upstream/main
git switch -c docs/short-description
```

## Make and Validate the Change

Keep the change focused. Run:

```bash
npm install
npm run check
pip install -r requirements-docs.txt
mkdocs build --strict
```

## Commit

```bash
git add path/to/changed-file.md
git commit -m "docs: describe the reader-facing change"
```

Do not commit generated `site/` output, dependencies, credentials, or confidential information.

## Push and Open a Pull Request

```bash
git push --set-upstream origin docs/short-description
```

Open a pull request against the upstream default branch. Complete the template, link the issue, and explain validation and review needs.

## Review

Respond to review comments constructively. Add follow-up commits to the same branch and rerun the checks. Resolve conversations after agreement, not merely after responding.

## Merge

A maintainer merges after required approvals and checks pass. Delete the feature branch when it is no longer needed and synchronize your fork before the next contribution.
