<!-- markdownlint-disable -->

# Hardening Report: chetan--git-restore-mtime-action/v2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **chetan--git-restore-mtime-action/v2.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): a GitHub Actions expression is put straight into the run: script. Offending line: `cd "${{ inputs.working-directory }}" && "$GITHUB_ACTION_PATH"/git-restore-mtime`. The input is substituted into the script text before bash runs it, so a value containing a double quote or $(...) can break out of the quotes and run its own commands. Fix: set the value in env: (WORKING_DIRECTORY: ${{ inputs.working-directory }}) and use cd "$WORKING_DIRECTORY" in the script.

Locations:

- `action.yml:17`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.working-directory }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:17`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

In action.yml, the run: script no longer contains inputs.working-directory directly. The value now goes through env as WORKING_DIRECTORY, and the script runs cd "$WORKING_DIRECTORY". The bash shell and the git-restore-mtime call are unchanged.

