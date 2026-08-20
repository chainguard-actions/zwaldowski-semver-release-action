<!-- markdownlint-disable -->

# Hardening Report: zwaldowski--semver-release-action/v2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zwaldowski--semver-release-action/v2.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file uses `actions/checkout@v1`, which is a mutable tag reference rather than a pinned 40-character commit SHA. This means the action could be silently updated to a different (potentially malicious) version without any change to the workflow file, creating a supply-chain risk.

Locations:

- `.github/workflows/test.yml:11`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key, and the single job `test` also has no job-level `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions (which may be `write-all` for older repositories), granting broader access than necessary. A minimal permissions block (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

In .github/workflows/test.yml: (1) Pinned `actions/checkout@v1` to `actions/checkout@50fbc622fc4ef5163becd7fab6573eac35f8462e # v1` to eliminate the mutable tag supply-chain risk. (2) Added a top-level `permissions: contents: read` block to restrict the GITHUB_TOKEN to the minimum access needed for this CI workflow (checkout + npm test).

