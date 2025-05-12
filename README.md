# Magento README linter

A GitHub Action to validate README files in Magento modules using
[Super-Linter](https://github.com/super-linter/super-linter).

## Overview

This action helps maintain consistency and quality in README documentation across Magento
modules by leveraging the Super-Linter tool to perform automated validation of README
files. It automatically generates a detailed report of any rule violations and integrates
it into the GitHub Actions job summary.

The linter ensures that module README files comply with the Markdown rules required by
the [Adobe Commerce module reference](https://developer.adobe.com/commerce/php/module-reference/)
documentation, which is automatically generated from these README files.

Consistent Markdown formatting improves user experience by ensuring compatibility between
different Markdown editors and rendering engines, while also maintaining better readability
in raw Markdown format.

## Quick start

1. Add this action to your GitHub workflow:

```yaml
name: Lint README Files
on: pull_request

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: commerce-docs/magento-readme-linter@v1
```

1. Create `.github/super-linter.env` with basic configuration:

```env
FILTER_REGEX_INCLUDE=app/code/[^/]+/[^/]+/README.md$
IGNORE_GITIGNORED_FILES=true
MARKDOWN_CONFIG_FILE=.markdownlint.yml
MULTI_STATUS=false
VALIDATE_ALL_CODEBASE=false
VALIDATE_GITHUB_ACTIONS=true
VALIDATE_MARKDOWN=true
```

Note: The `FILTER_REGEX_INCLUDE` pattern should match your module locations. See
[Configuration](#configuration) for examples of different repository structures.

1. Create `.markdownlint.yml` with Markdown rules (conventionally placed in
   `.github/linters/.markdownlint.yml`). You can use the
   [Adobe Commerce Markdown rules](https://github.com/AdobeDocs/commerce-php/blob/main/.github/linters/.markdownlint.yml)
   as a starting point:

```yaml
default: true
MD013: false  # Line length
MD033: false  # No inline HTML
MD041: false  # First line in file should be a top level heading
```

See [Configuration](#configuration) for more details on customizing these files.

## Features

- Validates README files using [Super-Linter](https://github.com/super-linter/super-linter)
- Supports Markdown linting rules via [markdownlint](https://github.com/DavidAnson/markdownlint)
  with [available rules](https://github.com/DavidAnson/markdownlint?tab=readme-ov-file#rules--aliases)
- Ensures compatibility with Adobe Commerce module reference documentation
- Improves Markdown rendering compatibility across different editors
- Maintains consistent formatting for better raw Markdown readability
- Runs on pull request
- Automatically generates linting reports in GitHub Actions job summary
- Configurable file inclusion/exclusion patterns
- Supports custom Markdown linting rules

## Rule violations

When the linter detects rule violations, it provides detailed feedback in the GitHub Actions
job summary. For example:

```text
README.md: 8: MD013/line-length Line length [Expected: 80; Actual: 89]
README.md: 13: MD013/line-length Line length [Expected: 80; Actual: 86]
README.md: 17: MD013/line-length Line length [Expected: 80; Actual: 89]
README.md: 51: MD013/line-length Line length [Expected: 80; Actual: 88]
```

Each violation report includes:

- File name and line number where the violation occurred
- Rule identifier (e.g., `MD013/line-length`)
- Description of the violation
- Expected and actual values

When a check fails, you can find the detailed linting results in the Job Summary. To access it:

1. Click on the failed check in your pull request
2. In the error log view, click on the "Summary" tab at the top of the page
3. Look for the "Markdown Linter Report" section

For more information about the rules and how to fix violations, refer to the
[markdownlint rules documentation](https://github.com/DavidAnson/markdownlint?tab=readme-ov-file#rules--aliases).

## Configuration

The action uses the Super-Linter configuration from `.github/super-linter.env`. Here are
example configurations for different repository structures:

### Example 1: Modules in root directory

For repositories where modules are stored directly in the root directory:

```env
FILTER_REGEX_EXCLUDE=_metapackage/.*
FILTER_REGEX_INCLUDE=^/github/workspace/[^/]+/README.md$
IGNORE_GITIGNORED_FILES=true
MARKDOWN_CONFIG_FILE=.markdownlint.yml
MULTI_STATUS=false
VALIDATE_ALL_CODEBASE=false
VALIDATE_GITHUB_ACTIONS=true
VALIDATE_MARKDOWN=true
```

### Example 2: Modules in app/code directory

For repositories following the standard Magento 2 structure with modules in `app/code`:

```env
FILTER_REGEX_INCLUDE=app/code/[^/]+/[^/]+/README.md$
IGNORE_GITIGNORED_FILES=true
MARKDOWN_CONFIG_FILE=.markdownlint.yml
MULTI_STATUS=false
VALIDATE_ALL_CODEBASE=false
VALIDATE_GITHUB_ACTIONS=true
VALIDATE_MARKDOWN=true
```

### Key configuration options

- `FILTER_REGEX_INCLUDE`: Pattern to match module README files based on your repository
  structure
- `FILTER_REGEX_EXCLUDE`: Optional pattern to exclude specific paths from linting
- `MARKDOWN_CONFIG_FILE`: Path to Markdown linting rules. See
  [markdownlint configuration](https://github.com/DavidAnson/markdownlint#optionsconfig)
  for available rules and options.
- `VALIDATE_MARKDOWN`: Enable/disable Markdown validation
- `VALIDATE_ALL_CODEBASE`: Validate all files or only changed files
- `MULTI_STATUS`: When set to `false`, creates a single status check for all linting
  results. When `true`, creates separate status checks for each linter. Set to `false`
  by default to provide a single consolidated status check for simpler CI integration.

## Workflow integration

The action integrates with GitHub Actions workflows and provides:

1. Automatic linting on pull requests
2. Detailed reports in the GitHub Actions job summary
3. Status checks for pull requests
4. Configurable validation rules

### Complete workflow example

Here's a complete example of how to use this action in your workflow:

```yaml
name: Lint README Files
on: pull_request

permissions:
  contents: read
  statuses: write

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: commerce-docs/magento-readme-linter@v1
```

## Requirements

- GitHub repository with Magento modules
- README files in Markdown format
- GitHub Actions enabled in your repository
- `.github/super-linter.env` configuration file
- `.markdownlint.yml` for custom Markdown rules

## Customization

You can customize the action's behavior by:

1. Modifying the `super-linter.env` configuration
2. Adding custom Markdown rules in `.markdownlint.yml`
3. Adjusting the workflow triggers and permissions
4. Configuring file inclusion/exclusion patterns

## Support

For issues and feature requests, please create an issue in this repository.
