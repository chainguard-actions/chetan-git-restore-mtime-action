<!-- markdownlint-disable -->

# Hardening Report: chetan--git-restore-mtime-action/v2.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **chetan--git-restore-mtime-action/v2.3** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a), direct expression interpolation. The composite step's run block puts `${{ inputs.working-directory }}` and `${{ inputs.args }}` straight into the shell script: `cd "${{ inputs.working-directory }}" && "$GITHUB_ACTION_PATH"/git-restore-mtime ${{ inputs.args }}`. A value containing `"`, `;`, `$(...)` or similar can break out and run arbitrary commands. The fix is to map both inputs into env: variables (for example WORKING_DIRECTORY and ARGS) and quote them, e.g. `cd "$WORKING_DIRECTORY"`. For ARGS, keep in mind it is meant to hold multiple arguments.

Locations:

- `action.yml:21`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.working-directory }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:21`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.args }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:21`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Moved inputs.working-directory and inputs.args into env (WORKING_DIRECTORY, INPUT_ARGS). working-directory is quoted in cd; args is tokenized with the quote-aware xargs split (with unmatched-quote check and empty guard) into a bash array, matching upstream's inline-expression splitting behavior.

