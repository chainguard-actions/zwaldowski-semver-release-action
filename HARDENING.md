<!-- markdownlint-disable -->

# Hardening Report: zwaldowski--semver-release-action/v4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zwaldowski--semver-release-action/v4** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses `actions/checkout@v1`, which is pinned to a mutable tag rather than an immutable 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit, enabling supply-chain attacks. Replace with a full SHA pin, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v1`.

Locations:

- `.github/workflows/test.yml:9`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/test.yml` has no top-level `permissions:` key, and the only job (`test`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g. `write` access to contents). Add a top-level `permissions: {}` block and grant only the minimum scopes required.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed two findings in .github/workflows/test.yml: (1) Pinned `actions/checkout@v1` to its full commit SHA `50fbc622fc4ef5163becd7fab6573eac35f8462e` with the original tag preserved as a comment. (2) Added a top-level `permissions: {}` block after the `on:` trigger section to enforce least-privilege token access.

