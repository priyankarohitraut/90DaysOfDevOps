Task 1: PR Lifecycle Workflow

Create:

.github/workflows/pr-lifecycle.yml
name: PR Lifecycle

on:
  pull_request:
    types: [opened, synchronize, reopened, closed]

jobs:
  pr-info:
    runs-on: ubuntu-latest

    steps:
      - name: Print event type
        run: echo "Event: ${{ github.event.action }}"

      - name: Print PR details
        run: |
          echo "Title: ${{ github.event.pull_request.title }}"
          echo "Author: ${{ github.event.pull_request.user.login }}"
          echo "Source: ${{ github.head_ref }}"
          echo "Target: ${{ github.base_ref }}"

      - name: Only when PR merged
        if: github.event.pull_request.merged == true
        run: echo "PR was merged "
 Task 2: PR Validation Workflow (Real Gate)
name: PR Checks

on:
  pull_request:
    branches: [main]

jobs:
  file-size-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Check file size
        run: |
          find . -type f -size +1M && echo "Large file found" && exit 1 || echo "All files OK"

  branch-name-check:
    runs-on: ubuntu-latest
    steps:
      - name: Validate branch name
        run: |
          BRANCH="${{ github.head_ref }}"
          if [[ "$BRANCH" =~ ^(feature|fix|docs)/ ]]; then
            echo "Branch name valid"
          else
            echo "Invalid branch name"
            exit 1
          fi

  pr-body-check:
    runs-on: ubuntu-latest
    steps:
      - name: Check PR description
        continue-on-error: true
        run: |
          if [ -z "${{ github.event.pull_request.body }}" ]; then
            echo "PR description is empty"
            exit 1
          else
            echo "PR description present"
          fi
 Task 3: Scheduled Workflows
name: Scheduled Tasks

on:
  schedule:
    - cron: '30 2 * * 1'
    - cron: '0 */6 * * *'
  workflow_dispatch:

jobs:
  schedule-job:
    runs-on: ubuntu-latest

    steps:
      - name: Print schedule
        run: echo "Triggered by: ${{ github.event.schedule }}"

      - name: Health check
        run: |
          STATUS=$(curl -o /dev/null -s -w "%{http_code}" https://example.com)
          echo "Status: $STATUS"
          if [ "$STATUS" -ne 200 ]; then exit 1; fi
 Notes

 Every weekday 9 AM IST:

30 3 * * 1-5

 First day of month midnight:

0 0 1 * *

 Why delay/skip?

GitHub reduces load
Inactive repos get lower priority
 Task 4: Path & Branch Filters
name: Smart Trigger

on:
  push:
    branches:
      - main
      - release/*
    paths:
      - 'src/**'
      - 'app/**'

jobs:
  run:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Code changed in src/app"
Second workflow (ignore docs)
on:
  push:
    paths-ignore:
      - '*.md'
      - 'docs/**'
 Notes
paths → run ONLY when these change
paths-ignore → skip when ONLY these change
 Task 5: workflow_run (Chain Workflows)
🔹 tests.yml
name: Run Tests

on: push

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Running tests"
🔹 deploy-after-tests.yml
name: Deploy After Tests

on:
  workflow_run:
    workflows: ["Run Tests"]
    types: [completed]

jobs:
  deploy:
    runs-on: ubuntu-latest
    if: ${{ github.event.workflow_run.conclusion == 'success' }}

    steps:
      - run: echo "Deploying..."

  fail:
    runs-on: ubuntu-latest
    if: ${{ github.event.workflow_run.conclusion != 'success' }}

    steps:
      - run: echo "Tests failed. Not deploying."
 Task 6: repository_dispatch
name: External Trigger

on:
  repository_dispatch:
    types: [deploy-request]

jobs:
  external:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Environment: ${{ github.event.client_payload.environment }}"
