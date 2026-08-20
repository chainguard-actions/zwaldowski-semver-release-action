<!-- markdownlint-disable -->

# Hardening Report: zwaldowski--semver-release-action/v3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zwaldowski--semver-release-action/v3** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses `actions/checkout@v1`, which is pinned to a mutable tag rather than an immutable 40-character commit SHA. This means the action could be silently updated or compromised without the workflow noticing, enabling supply-chain attacks.

Locations:

- `.github/workflows/test.yml:11`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key, and the single job `test` also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the default (often write-all) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both findings in .github/workflows/test.yml: (1) Pinned actions/checkout@v1 to its full commit SHA (50fbc622fc4ef5163becd7fab6573eac35f8462e) with a # v1 comment for readability. (2) Added a top-level `permissions: contents: read` block — the minimum required for the checkout step; npm ci and npm test need no additional GitHub token permissions.

