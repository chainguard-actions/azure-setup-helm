<!-- markdownlint-disable -->

# Hardening Report: Azure--setup-helm/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Azure--setup-helm/v5.0.0** was hardened automatically. 6 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (b): The env var `PR_BASE_REF` is sourced from the untrusted expression `${{ github.event.pull_request.base.ref }}` (set in the job-level `env:` block) and then used **unquoted** inside a `run:` block: `echo $PR_BASE_REF` and `if [[ $PR_BASE_REF != releases/* ]]`. An attacker controlling the PR base ref could inject shell metacharacters. The variable must be double-quoted: `"$PR_BASE_REF"`.

Locations:

- `.github/workflows/integration-tests.yml:27`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. This means the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/defaultLabels.yml:1`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. This means the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/integration-tests.yml:1`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. This means the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/prettify-code.yml:1`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. This means the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/tag-and-draft.yml:1`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. This means the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/unit-tests.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, missing-permissions

**Notes:**

Fixed all 6 findings across 5 workflow files:
1. integration-tests.yml: Added `permissions: {}` top-level block; double-quoted both unquoted uses of `$PR_BASE_REF` (`echo "$PR_BASE_REF"` and `if [[ "$PR_BASE_REF" != releases/* ]]`) to prevent shell metacharacter injection.
2. defaultLabels.yml: Added `permissions: { issues: write, pull-requests: write }` — required by the stale action to label issues and PRs.
3. prettify-code.yml: Added `permissions: { contents: read }` — only needs to read code for formatting checks.
4. tag-and-draft.yml: Added `permissions: { contents: write }` — needs to create tags and release drafts.
5. unit-tests.yml: Added `permissions: { contents: read }` — only needs to check out and run tests.

