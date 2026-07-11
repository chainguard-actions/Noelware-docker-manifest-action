<!-- markdownlint-disable -->

# Hardening Report: Noelware--docker-manifest-action/1.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **Noelware--docker-manifest-action/1.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file .github/workflows/CI.yaml references actions using mutable tags instead of pinned full-length commit SHAs. Failing references: actions/checkout@v4 (line 47), oven-sh/setup-bun@v2 (line 48), actions/checkout@v4 (line 57), oven-sh/setup-bun@v2 (line 58), EndBug/add-and-commit@v9 (line 61). These should be pinned to their full 40-character commit SHA to prevent supply-chain attacks via tag mutation.

Locations:

- `.github/workflows/CI.yaml:47`
- `.github/workflows/CI.yaml:48`
- `.github/workflows/CI.yaml:57`
- `.github/workflows/CI.yaml:58`
- `.github/workflows/CI.yaml:61`

### missing-permissions (severity: medium)

The workflow file .github/workflows/CI.yaml has no top-level `permissions:` key and neither the `ci` job nor the `build` job defines its own `permissions:` block. Without explicit permissions, the workflow inherits the default repository permissions (which may include write access), violating the principle of least privilege.

Locations:

- `.github/workflows/CI.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed .github/workflows/CI.yaml: (1) Pinned all 5 action references to full commit SHAs — actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5, oven-sh/setup-bun@v2 → @0c5077e51419868618aeaa5fe8019c62421857d6, EndBug/add-and-commit@v9 → @a94899bca583c204427a224a7af87c02f9b325d5, with original tag names preserved as comments. (2) Added top-level `permissions: {}` to deny all permissions by default, plus job-level overrides: `contents: read` for the ci job and `contents: write` for the build job (which needs to push committed build artifacts).

