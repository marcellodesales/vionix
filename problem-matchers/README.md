# 🔦 Problem Matchers

This directory contains all the Github Problem Matchers for linters to be displayed in the Github Actions Workflow Summary. These are used for the test workflows.

* [x] Python = `flake8`
* [x] Golang = `golangci`
* [x] Github Actions = `actionlint`
* [x] Docker = `hadolint`

# 🔧 Configuration

Here's an example of the docker-image-test-workflow that builds the test cases and lints the project with the Docker linter `hadolint`.

## github-problem-matcher-name

The name of the problem matcher for a given programming language or framework.

# 🛠️ Example

Using the `docker` linter.

```yaml
name: docker-image-test-workflow

on:
  push:
    branches: [main]

jobs:
  docker-devsecops-check:
    name: 🚦 check
    uses: seceng-devsecops-platform/devsecops-platform-github-workflows/.github/workflows/docker-compose-devsecops-test-workflow.yaml@main
    with:
      # Tests implemented in docker compose, with the contract to report using JUNIT report format
      docker-compose-test: true
      docker-compose-test-file: docker-compose-check.yaml
      docker-compose-test-service: test
      docker-compose-test-report-file: build/test-results.xml

      docker-compose-lint: true
      docker-compose-lint-file: docker-compose-check.yaml
      docker-compose-lint-service: lint
      github-problem-matcher-name: hadolint
```
