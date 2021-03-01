# grandcentrix Actions Validator

GitHub Action for https://github.com/grandcentrix/grandcentrix-actions-simulator

# Usage

<!-- start usage -->
```yaml
- uses: grandcentrix/grandcentrix-actions-validator@v1.0.0
  with:
    # Required. Path to workflow file to use
    file: ''

    # Optional. Path to config file to override defaults
    config: ''

    # Optional. In dry run no shell scripts are executed
    #
    # Default: 'false'
    dry_run: ''

    # Optional. Fail as soon as there is an unknown variable or undeclared action input is used
    #
    # Default: 'false'
    fail_fast: ''
    
    # Optional. Sets an variable - e.g. foo=bar, separated by whitespace
    vars: ''
```
<!-- end usage -->

# License

The scripts and documentation in this project are released under the [Apache 2.0 License](LICENSE)
