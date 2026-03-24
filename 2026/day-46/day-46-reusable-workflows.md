Task 1: Notes (Theory)
 What is a reusable workflow?
A workflow that can be called by another workflow
Helps avoid duplication
Centralized logic (build, deploy, etc.)
 What is workflow_call?
A trigger that allows a workflow to be called by another workflow
 Reusable workflow vs regular action (uses:)
Feature	Reusable Workflow	Action
Level	Full workflow	Step-level
Contains	Jobs	Steps
Trigger	workflow_call	uses:
 Where must it live?

 .github/workflows/

 Task 2: Reusable Workflow

Create:

.github/workflows/reusable-build.yml
name: Reusable Build

on:
  workflow_call:
    inputs:
      app_name:
        required: true
        type: string
      environment:
        required: true
        type: string
        default: staging
    secrets:
      docker_token:
        required: true

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Print build info
        run: echo "Building ${{ inputs.app_name }} for ${{ inputs.environment }}"

      - name: Check secret
        run: |
          if [ -z "${{ secrets.docker_token }}" ]; then
            echo "Docker token is set: false"
          else
            echo "Docker token is set: true"
          fi
 Task 3: Caller Workflow

Create:

.github/workflows/call-build.yml
name: Call Reusable Workflow

on:
  push:
    branches:
      - main

jobs:
  build:
    uses: ./.github/workflows/reusable-build.yml
    with:
      app_name: "my-web-app"
      environment: "production"
    secrets:
      docker_token: ${{ secrets.DOCKER_TOKEN }}
 Verify
Go to Actions tab
You’ll see:
Caller workflow
Inside → reusable workflow executed
 Task 4: Add Outputs
🔹 Update reusable workflow
jobs:
  build:
    runs-on: ubuntu-latest

    outputs:
      build_version: ${{ steps.version.outputs.version }}

    steps:
      - uses: actions/checkout@v4

      - id: version
        run: echo "version=v1.0-$(echo $GITHUB_SHA | cut -c1-7)" >> $GITHUB_OUTPUT
🔹 Update caller workflow
jobs:
  build:
    uses: ./.github/workflows/reusable-build.yml
    with:
      app_name: "my-web-app"
      environment: "production"
    secrets:
      docker_token: ${{ secrets.DOCKER_TOKEN }}

  print-version:
    runs-on: ubuntu-latest
    needs: build

    steps:
      - run: echo "Build version is ${{ needs.build.outputs.build_version }}"
 Verify

 Output:

Build version is v1.0-abc1234
 Task 5: Composite Action

Create:

.github/actions/setup-and-greet/action.yml
name: Setup and Greet

inputs:
  name:
    required: true
  language:
    required: false
    default: en

outputs:
  greeted:
    value: true

runs:
  using: "composite"
  steps:
    - run: |
        if [ "${{ inputs.language }}" = "en" ]; then
          echo "Hello ${{ inputs.name }}"
        else
          echo "Namaste ${{ inputs.name }}"
        fi
      shell: bash

    - run: date
      shell: bash

    - run: echo "Runner OS: $RUNNER_OS"
      shell: bash
🔹 Use it in workflow
name: Use Composite Action

on: push

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - uses: ./.github/actions/setup-and-greet
        with:
          name: "Rohit"
          language: "en"
 Verify

 Output:

Greeting message
Date
Runner OS
Task 6: Notes (Comparison)
Feature	Reusable Workflow	Composite Action
Triggered by	workflow_call	uses: in step
Can contain jobs?	✅ Yes	❌ No
Can contain steps?	✅ Yes	✅ Yes
Lives where?	.github/workflows/	.github/actions/
Can accept secrets?	✅ Yes	❌ Not directly
Best for	Full pipelines	Reusable step logic
