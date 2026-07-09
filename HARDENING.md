<!-- markdownlint-disable -->

# Hardening Report: azure--setup-helm/v3.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **azure--setup-helm/v3.5** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tags or branch names instead of pinned full-length SHA commit hashes. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the branch is updated. Failing references: defaultLabels.yml: actions/stale@v3 (×2); integration-tests.yml: actions/checkout@v2; prettify-code.yml: actions/checkout@v2, actionsx/prettier@v2; release-pr.yml: OliverMKing/javascript-release-workflow/.github/workflows/release-pr.yml@main; tag-and-draft.yml: OliverMKing/javascript-release-workflow/.github/workflows/tag-and-release.yml@main; unit-tests.yml: actions/checkout@v1.

Locations:

- `.github/workflows/defaultLabels.yml:14`
- `.github/workflows/defaultLabels.yml:27`
- `.github/workflows/integration-tests.yml:18`
- `.github/workflows/prettify-code.yml:12`
- `.github/workflows/prettify-code.yml:15`
- `.github/workflows/release-pr.yml:12`
- `.github/workflows/tag-and-draft.yml:9`
- `.github/workflows/unit-tests.yml:16`

### missing-permissions (severity: medium)

None of the workflow files define a top-level or job-level 'permissions:' block. Without explicit permissions, workflows run with the default (potentially broad) token permissions. All six workflow files are affected: defaultLabels.yml, integration-tests.yml, prettify-code.yml, release-pr.yml, tag-and-draft.yml, unit-tests.yml.

Locations:

- `.github/workflows/defaultLabels.yml:1`
- `.github/workflows/integration-tests.yml:1`
- `.github/workflows/prettify-code.yml:1`
- `.github/workflows/release-pr.yml:1`
- `.github/workflows/tag-and-draft.yml:1`
- `.github/workflows/unit-tests.yml:1`

### script-injection (severity: high)

Rule (b) violation in integration-tests.yml: the env var PR_BASE_REF is set from the attacker-controllable expression '${{ github.event.pull_request.base.ref }}' and then expanded unquoted inside a run: block ('echo $PR_BASE_REF' and 'if [[ $PR_BASE_REF != releases/* ]]'). An attacker controlling the PR base ref could inject shell metacharacters. The variable must be double-quoted: "$PR_BASE_REF".

Locations:

- `.github/workflows/integration-tests.yml:21`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, script-injection

**Notes:**

Fixed all findings across 6 workflow files:

1. unpinned-uses: Pinned all action references to full commit SHAs:
   - actions/stale@v3 → @98ed4cb500039dbcccf4bd9bedada4d0187f2757 (defaultLabels.yml, ×2)
   - actions/checkout@v2 → @ee0669bd1cc54295c223e0bb666b733df41de1c5 (integration-tests.yml, prettify-code.yml)
   - actionsx/prettier@v2 → @8b6d14bd1241e743fa9a6112b43d691d1890a8e9 (prettify-code.yml)
   - OliverMKing/javascript-release-workflow@main → @a2f171c6ca04fea2de31f4dbb6dc576140395512 (release-pr.yml, tag-and-draft.yml)
   - actions/checkout@v1 → @50fbc622fc4ef5163becd7fab6573eac35f8462e (unit-tests.yml)

2. missing-permissions: Added top-level 'permissions: {}' to all 6 workflow files. Added job-level permissions where needed (issues: write, pull-requests: write for stale action; contents: read for checkout-based jobs).

3. script-injection: Fixed unquoted $PR_BASE_REF in integration-tests.yml run block — changed 'echo $PR_BASE_REF' to 'echo "$PR_BASE_REF"' and 'if [[ $PR_BASE_REF != releases/* ]]' to 'if [[ "$PR_BASE_REF" != releases/* ]]'.

