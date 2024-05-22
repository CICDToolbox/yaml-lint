<!-- markdownlint-disable -->
<p align="center">
    <a href="https://github.com/CICDToolbox/">
        <img src="https://cdn.wolfsoftware.com/assets/images/github/organisations/cicdtoolbox/black-and-white-circle-256.png" alt="CICDToolbox logo" />
    </a>
    <br />
    <a href="https://github.com/CICDToolbox/yaml-lint/actions/workflows/cicd.yml">
        <img src="https://img.shields.io/github/actions/workflow/status/CICDToolbox/yaml-lint/cicd.yml?branch=master&label=build%20status&style=for-the-badge" alt="Github Build Status" />
    </a>
    <a href="https://github.com/CICDToolbox/yaml-lint/blob/master/LICENSE.md">
        <img src="https://img.shields.io/github/license/CICDToolbox/yaml-lint?color=blue&label=License&style=for-the-badge" alt="License">
    </a>
    <a href="https://github.com/CICDToolbox/yaml-lint">
        <img src="https://img.shields.io/github/created-at/CICDToolbox/yaml-lint?color=blue&label=Created&style=for-the-badge" alt="Created">
    </a>
    <br />
    <a href="https://github.com/CICDToolbox/yaml-lint/releases/latest">
        <img src="https://img.shields.io/github/v/release/CICDToolbox/yaml-lint?color=blue&label=Latest%20Release&style=for-the-badge" alt="Release">
    </a>
    <a href="https://github.com/CICDToolbox/yaml-lint/releases/latest">
        <img src="https://img.shields.io/github/release-date/CICDToolbox/yaml-lint?color=blue&label=Released&style=for-the-badge" alt="Released">
    </a>
    <a href="https://github.com/CICDToolbox/yaml-lint/releases/latest">
        <img src="https://img.shields.io/github/commits-since/CICDToolbox/yaml-lint/latest.svg?color=blue&style=for-the-badge" alt="Commits since release">
    </a>
    <br />
    <a href="https://github.com/CICDToolbox/yaml-lint/blob/master/.github/CODE_OF_CONDUCT.md">
        <img src="https://img.shields.io/badge/Code%20of%20Conduct-blue?style=for-the-badge" />
    </a>
    <a href="https://github.com/CICDToolbox/yaml-lint/blob/master/.github/CONTRIBUTING.md">
        <img src="https://img.shields.io/badge/Contributing-blue?style=for-the-badge" />
    </a>
    <a href="https://github.com/CICDToolbox/yaml-lint/blob/master/.github/SECURITY.md">
        <img src="https://img.shields.io/badge/Report%20Security%20Concern-blue?style=for-the-badge" />
    </a>
    <a href="https://github.com/CICDToolbox/yaml-lint/issues">
        <img src="https://img.shields.io/badge/Get%20Support-blue?style=for-the-badge" />
    </a>
</p>

## Overview

A tool to validate your yaml files in CI/CD pipelines using [yamllint](https://pypi.org/project/yamllint/).

This tool has been tested against the following:

1. GitHub Actions
2. Travis CI
3. CircleCI
4. BitBucket pipelines
5. Local command line

However due to the way that they are built they should work on most CICD platforms where you can run arbitrary scripts.

We provide a [script](https://github.com/CICDToolbox/get-all-tools) which pulls the latest copy of all the CICD tools and
places them in a local bin directory to allow them to be run any time locally for added validation.

## Basic Usage

```yml
on: [push, pull_request]

jobs:
  build:
    name: YAML Lint
    runs-on: ubuntu-latest

    steps:
      - name: Checkout the Repository
        uses: actions/checkout@v4

      - name: Set up Ruby 3.3
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: "3.3"

      - name: Run YAML Lint
        run: bash <(curl -s https://raw.githubusercontent.com/CICDToolbox/yaml-lint/master/pipeline.sh)
```

### Configuration Options

The following environment variables can be set in order to customise the script.

| Name          | Default Value | Purpose                                                                                                         |
| :------------ | :-----------: | :-------------------------------------------------------------------------------------------------------------- |
| INCLUDE_FILES |     Unset     | A comma separated list of files to include for being scanned. You can also use `regex` to do pattern matching.  |
| EXCLUDE_FILES |     Unset     | A comma separated list of files to exclude from being scanned. You can also use `regex` to do pattern matching. |
| NO_COLOR      |     False     | Turn off the color in the output.                                                                               |
| REPORT_ONLY   |     False     | Generate the report but do not fail the build even if an error occurred.                                        |
| SHOW_ERRORS   |     True      | Show the actual errors instead of just which files had errors.                                                  |
| SHOW_SKIPPED  |     False     | Show which files are being skipped.                                                                             |

> If you set INCLUDE_FILES - it will skip ALL files that do not match, including anything in EXCLUDE_FILES.

You can use any combination of the above settings.

```yml
on: [push, pull_request]

jobs:
  build:
    name: YAML Lint
    runs-on: ubuntu-latest

    steps:
      - name: Checkout the Repository
        uses: actions/checkout@v4

      - name: Set up Ruby 3.3
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: "3.3"

      - name: Run YAML Lint
        env:
          REPORT_ONLY: true
          SHOW_ERRORS: true
        run: bash <(curl -s https://raw.githubusercontent.com/CICDToolbox/yaml-lint/master/pipeline.sh)
```

### Example Output

This is an example of the output report generated by this tool, this is the actual output from the tool running against itself.
```
--------------------------------------------------------------------- Stage 1: Parameters --
 No parameters given
---------------------------------------------------------- Stage 2: Install Prerequisites --
 [ OK ] python -m pip install --quiet --upgrade pip
 [ OK ] yamllint is already installed
--------------------------------------------------------- Stage 3: Run yamllint (v1.35.1) --
 [ OK ] .github/dependabot.yml
 [ OK ] .github/FUNDING.yml
 [ OK ] .github/ISSUE_TEMPLATE/ask_question.yml
 [ OK ] .github/ISSUE_TEMPLATE/bug_report.yml
 [ OK ] .github/ISSUE_TEMPLATE/config.yml
 [ OK ] .github/ISSUE_TEMPLATE/feature_request.yml
 [ OK ] .github/workflows/cicd.yml
 [ OK ] .github/workflows/citation-validation.yml
 [ OK ] .github/workflows/delete-old-workflow-runs.yml
 [ OK ] .github/workflows/dependabot.yml
 [ OK ] .github/workflows/document-validation.yml
 [ OK ] .github/workflows/generate-release.yml
 [ OK ] .github/workflows/generate-test-release.yml
 [ OK ] .github/workflows/greetings.yml
 [ OK ] .github/workflows/purge-deprecated-workflow-runs.yml
 [ OK ] .github/workflows/repository-validation.yml
 [ OK ] .github/workflows/security-hardening.yml
 [ OK ] .github/workflows/stale.yml
------------------------------------------------------------------------- Stage 4: Report --
 Total: 18, OK: 18, Failed: 0, Skipped: 0
----------------------------------------------------------------------- Stage 5: Complete --
```

### File Identification

Yaml files are identified using the following code:

```shell
[[ ${filename} =~ \.(yml|yaml)$ ]]
```

There is not magic type for yaml files so file -b is of not use for identifying the files.

## Yamllint Defaults

This is a pre-defined configuration named relaxed. As its name suggests, it is more tolerant:

```shell
---
extends: default

rules:
  braces:
    level: warning
    max-spaces-inside: 1
  brackets:
    level: warning
    max-spaces-inside: 1
  colons:
    level: warning
  commas:
    level: warning
  comments: disable
  comments-indentation: disable
  document-start: disable
  empty-lines:
    level: warning
  hyphens:
    level: warning
  indentation:
    level: warning
    indent-sequences: consistent
  line-length:
    level: warning
    allow-non-breakable-inline-mappings: true
  truthy: disable
```

<br />
<p align="right"><a href="https://wolfsoftware.com/"><img src="https://img.shields.io/badge/Created%20by%20Wolf%20on%20behalf%20of%20Wolf%20Software-blue?style=for-the-badge" /></a></p>
