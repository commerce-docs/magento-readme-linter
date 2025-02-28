# Magento README Linter

A GitHub Action to validate README files in Magento modules using super-linter.

## Overview

This action helps maintain consistency and quality in README documentation across Magento modules by leveraging the super-linter tool to perform automated validation of README files.

## Usage

Add this action to your GitHub workflow:

```yaml
name: Lint README Files
on: pull_request

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: commerce-docs/magento-readme-linter@v1
```

## Features

- Validates README files using super-linter
- Supports markdown linting rules
- Runs on pull request

## Configuration

The action uses the default super-linter configuration from `.github/super-linter.env`. You can customize the linting rules by modifying this file in your repository.

## Requirements

- GitHub repository with Magento modules
- README files in markdown format
- GitHub Actions enabled in your repository
