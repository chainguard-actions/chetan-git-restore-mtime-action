<!-- markdownlint-disable -->

# Hardening Report: chetan--git-restore-mtime-action/v2.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **chetan--git-restore-mtime-action/v2.1.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a), direct expression interpolation: the composite action's run block puts the input expression straight into the shell script: `cd "${{ inputs.working-directory }}" && "$GITHUB_ACTION_PATH"/git-restore-mtime`. The value is substituted into the script before bash runs it, so a crafted working-directory value containing a double quote can break out of the quoting and run commands. Fix: map the input to an env var (e.g. `WORKING_DIRECTORY: ${{ inputs.working-directory }}`) and use `cd "$WORKING_DIRECTORY"`.

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

In action.yml, inputs.working-directory now goes into the step's env: as WORKING_DIRECTORY. The run block uses cd "$WORKING_DIRECTORY", so the value is no longer pasted into the bash script. The cd and git-restore-mtime commands work as before.

