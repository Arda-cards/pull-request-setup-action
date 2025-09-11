# pull-request setup action

[![ci](https://github.com/Arda-cards/pull-request-setup-action/actions/workflows/ci.yaml/badge.svg?branch=main)](https://github.com/Arda-cards/pull-request-setup-action/actions/workflows/ci.yaml?query=branch%3Amain)
[CHANGELOG.md](CHANGELOG.md)

This action, intended to be executed upon the creation of a pull-request will perform a common set of initialization

1. it adds the person that created the pull-request to the assignees, unless a bot did it,
2. it adds the pull-request to the named project,
3. it sets the status of the pull request in the project to 'In review'
4. it adds the pull request to the current iteration.

A side-effect of this action is that it links the repository to the project.

## Arguments

See [action.yaml](action.yaml).

## Permission Required

```yaml
permissions: {}
```

The action requires a PAT with:

- `projects`
- `read:org`
- `repo`

## Usage

Given the secret `GH_PROJECT_WRITER`, add this workflow:

```yaml
name: Pull Request Upkeep

on:
  pull_request:
    types: [ opened ]

permissions: { }

jobs:
  upkeep:
    if: !endsWith(github.actor, '[bot]')
    runs-on: ubuntu-latest
    steps:
      - uses: Arda-cards/pull-request-setup-action@v1
        with:
          iteration_field_name: "Iteration"
          project_title: "2025-H1"
          pull_request_number: "${{ github.event.pull_request.number }}"
          token: "${{ secrets.GH_PROJECT_WRITER }}"
```
