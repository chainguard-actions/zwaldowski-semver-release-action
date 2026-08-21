<!-- markdownlint-disable -->

# Hardening Report: zwaldowski--semver-release-action/v5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zwaldowski--semver-release-action/v5** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses `actions/checkout@v6` which is pinned to a mutable tag (`v6`) rather than a full 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit, enabling supply-chain attacks.

Locations:

- `.github/workflows/test.yml:11`

### script-injection (severity: high)

Sub-rule (a): A `${{ ... }}` expression is interpolated directly inside a `run:` shell command string. The offending line is: `run: echo "New version is ${{ steps.semver.outputs.version }}"`. The value of `steps.semver.outputs.version` flows through YAML template substitution before the shell ever sees it, allowing an attacker who can influence that output to inject arbitrary shell commands.

Locations:

- `.github/workflows/test.yml:22`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/test.yml` has no top-level `permissions:` key and the `test` job also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions (which may be `write-all`), granting broader access than necessary. A minimal explicit `permissions:` block should be added.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, script-injection

**Notes:**

Fixed all three findings in .github/workflows/test.yml: (1) Pinned actions/checkout@v6 to full commit SHA d23441a48e516b6c34aea4fa41551a30e30af803 with a # v6 comment for readability. (2) Added top-level `permissions: {}` and job-level `permissions: contents: read` (the minimum needed for checkout). (3) Moved `${{ steps.semver.outputs.version }}` out of the run: shell string into an env: block as SEMVER_VERSION, then referenced it as a plain environment variable $SEMVER_VERSION in the shell command.

