Task 1: Multi-Job Workflow

Create:

.github/workflows/multi-job.yml
name: Multi Job Pipeline

on: push

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building the app"

  test:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - run: echo "Running tests"

  deploy:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - run: echo "Deploying"
 Result
build → test → deploy (chain)
In Actions tab → you’ll see a dependency graph
 Task 2: Environment Variables
name: Env Variables Demo

on: push

env:
  APP_NAME: myapp   # workflow level

jobs:
  demo:
    runs-on: ubuntu-latest
    env:
      ENVIRONMENT: staging   # job level

    steps:
      - name: Print variables
        env:
          VERSION: 1.0.0   # step level
        run: |
          echo "App: $APP_NAME"
          echo "Env: $ENVIRONMENT"
          echo "Version: $VERSION"

      - name: GitHub context
        run: |
          echo "Commit SHA: ${{ github.sha }}"
          echo "Actor: ${{ github.actor }}"
 Task 3: Job Outputs
name: Job Outputs

on: push

jobs:
  generate:
    runs-on: ubuntu-latest
    outputs:
      today: ${{ steps.date.outputs.today }}

    steps:
      - id: date
        run: echo "today=$(date +%Y-%m-%d)" >> $GITHUB_OUTPUT

  consume:
    runs-on: ubuntu-latest
    needs: generate

    steps:
      - run: echo "Today's date is ${{ needs.generate.outputs.today }}"
 Notes (Interview)

 Why pass outputs?

Share data between jobs
Dynamic pipelines (versioning, tags, results)
Decouple jobs cleanly
 Task 4: Conditionals
name: Conditionals Demo

on:
  push:
  pull_request:

jobs:
  conditional-job:
    runs-on: ubuntu-latest
    if: github.event_name == 'push'   # job runs only on push

    steps:
      - name: Run only on main branch
        if: github.ref == 'refs/heads/main'
        run: echo "This runs only on main"

      - name: Force failure
        run: exit 1

      - name: Run if previous failed
        if: failure()
        run: echo "Previous step failed"

      - name: Continue even if error
        continue-on-error: true
        run: exit 1

      - name: Still runs
        run: echo "Pipeline continues"
 Notes
if: github.ref == 'refs/heads/main' → branch check
failure() → runs only if previous step failed
continue-on-error: true →
 Step fails BUT workflow continues
 Task 5: Smart Pipeline (Real CI/CD)

Create:

.github/workflows/smart-pipeline.yml
name: Smart Pipeline

on:
  push:

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Linting code"

  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Running tests"

  summary:
    runs-on: ubuntu-latest
    needs: [lint, test]

    steps:
      - name: Print branch type
        run: |
          if [[ "${{ github.ref }}" == "refs/heads/main" ]]; then
            echo "Main branch push"
          else
            echo "Feature branch push"
          fi

      - name: Print commit message
        run: echo "Commit message: ${{ github.event.head_commit.message }}"
