<!-- markdownlint-disable -->

# Hardening Report: zwaldowski--semver-release-action/v6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zwaldowski--semver-release-action/v6** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses `actions/checkout@v6` which is pinned to a mutable tag (`v6`) rather than a full 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit, enabling supply-chain attacks. It should be pinned to a specific SHA, e.g. `actions/checkout@<40-char-sha> # v6`.

Locations:

- `.github/workflows/test.yml:11`

### script-injection (severity: high)

Sub-rule (a): A `${{ ... }}` expression is interpolated directly inside a `run:` shell command string. The offending line is: `run: echo "New version is ${{ steps.semver.outputs.version }}"`. The value of `steps.semver.outputs.version` flows through YAML template substitution before the shell sees it, meaning an attacker who can influence the step output could inject arbitrary shell commands. The expression should be moved to an `env:` variable and the env var double-quoted in the shell script instead.

Locations:

- `.github/workflows/test.yml:22`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the single job (`test`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g. `write` access to contents). A minimal explicit `permissions:` block should be added (e.g. `permissions: read-all` or specific scopes like `contents: read`).

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings in .github/workflows/test.yml: (1) Pinned actions/checkout@v6 to full SHA d23441a48e516b6c34aea4fa41551a30e30af803 with a # v6 comment. (2) Moved ${{ steps.semver.outputs.version }} out of the run: shell string into an env: block as SEMVER_VERSION, referenced as $SEMVER_VERSION in the shell. (3) Added top-level `permissions: {}` to enforce least-privilege (no permissions granted by default).

