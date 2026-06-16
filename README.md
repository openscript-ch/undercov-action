# undercov-action

Don't go undercov! Track your test coverage and check for under-coverage with this Forgejo, Gitea and GitHub Action. 🕵️

## Usage

You can use undercov in your workflow to check for under-coverage. Here's an example of how to set it up:

```yaml
name: Coverage check

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  coverage:
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v6

      - name: Run tests and generate lcov file
        run: |
          # Run your tests and generate the lcov file

      - name: Check coverage with undercov
        uses: openscript-ch/undercov-action@v1
        with:
          threshold: 80
          files: '**/coverage/lcov.info'
          branch: coverage
```
