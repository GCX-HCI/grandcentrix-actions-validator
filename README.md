# grandcentrix Actions Validator

This action is built around https://github.com/grandcentrix/grandcentrix-actions-simulator. It allows you to check your workflow files e.g. when you edit them in a Pull Request. It checks for correct syntax and even simulates the run. Also, it is able to do a real run-through of your workflow if you want it to.

# Usage

## Prerequisites 

Currently this Action only runs on Linux runners (e.g. "ubuntu-latest").

The following things on your Linux runner are assumed to be installed - if not this might crash ingracefully
- git
- NodeJS 12 or better

## Parameters

<!-- start usage -->
```yaml
- uses: grandcentrix/grandcentrix-actions-validator@v0.1.4
  with:
    # Required. Path to workflow file to use
    file: ''

    # Optional. Path to config file to override defaults
    config: ''

    # Optional. In dry run no shell scripts are executed
    #
    # Default: 'true'
    dry_run: ''

    # Optional. Fail as soon as there is an unknown variable or undeclared action input is used
    #
    # Default: 'false'
    fail_fast: ''
    
    # Optional. Prepopulate some of the default vars and environment vars - you can still override them
    #
    # Default: 'true'
    prepopulate: ''
    
    # Optional. Sets an variable - e.g. foo=bar, separated by whitespace
    vars: ''
```
<!-- end usage -->

## Sample workflow

```yaml
name: Workflow Check

on:
  push:
    paths:
      - '.github/workflows/**'

jobs:
  runAction:
    name: Test Workflows
    runs-on: ubuntu-latest
    steps:
      - name: Checkout project files
        uses: actions/checkout@v2
      - name: Use Node.js
        uses: actions/setup-node@v1
        with:
          node-version: '12.x'
      - name: Check PR workflow
        id: pr
        uses: grandcentrix/grandcentrix-actions-validator@v0.1.4
        with:
          file: ${{ github.workspace }}/.github/workflows/pr.yml
          vars: github.actor=testuser secrets.GITHUB_TOKEN=token github.event_name=push
        continue-on-error: true
      - name: Check Release workflow
        id: release
        uses: grandcentrix/grandcentrix-actions-validator@v0.1.4
        with:
          file: ${{ github.workspace }}/.github/workflows/release.yml
          vars: github.actor=testuser secrets.GITHUB_TOKEN=token github.event_name=push
        continue-on-error: true
      - name: Check results
        if: steps.pr.outcome != 'success' || steps.release.outcome != 'success'
        run: |
          echo "At least one of the workflow checks failed. Will fail this job now"
          exit 1
```

# License

The scripts and documentation in this project are released under the [Apache 2.0 License](LICENSE)
