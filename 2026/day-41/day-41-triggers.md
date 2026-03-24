
Task 1: Trigger on Pull Request
Create .github/workflows/pr-check.yml
Trigger it only when a pull request is opened or updated against main
Add a step that prints: PR check running for branch: <branch name>
Create a new branch, push a commit, and open a PR
Watch the workflow run automatically
Verify: Does it show up on the PR page?
1. Create Workflow File

Create this file in your repo:

.github/workflows/pr-check.yml

Add this content:

name: PR Check

on:
  pull_request:
    branches:
      - main
    types: [opened, synchronize]

jobs:
  pr-check:
    runs-on: ubuntu-latest

    steps:
      - name: Print PR info
        run: echo "PR check running for branch: ${{ github.head_ref }}"
 2. Create a New Branch

Run these commands:

git checkout -b feature/test-pr

Make a small change (example):

echo "test change" >> test.txt
 3. Commit & Push
git add .
git commit -m "testing PR workflow"
git push origin feature/test-pr
 4. Open Pull Request
Go to your GitHub repo
Click "Compare & pull request"
Make sure:
Base branch = main
Compare branch = feature/test-pr
Click Create PR
 5. Verify Workflow Run

Now check:

Go to the PR page
Scroll down → You will see Checks section
Click Details
You should see output like:
PR check running for branch: feature/test-pr


Task 2: Scheduled Trigger
Add a schedule: trigger to any workflow using cron syntax
Set it to run every day at midnight UTC
Write in your notes: What is the cron expression for every Monday at 9 AM?
i will done


Task 3: Manual Trigger
Create .github/workflows/manual.yml with a workflow_dispatch: trigger
Add an input that asks for an environment name (staging/production)
Print the input value in a step
Go to the Actions tab → find the workflow → click Run workflow
Verify: Can you trigger it manually and see your input printed?
1. Create Workflow File

Create:

.github/workflows/manual.yml

Add this:

name: Manual Trigger Workflow

on:
  workflow_dispatch:
    inputs:
      environment:
        description: "Choose environment (staging/production)"
        required: true
        default: "staging"

jobs:
  manual-job:
    runs-on: ubuntu-latest

    steps:
      - name: Print environment
        run: echo "Selected environment: ${{ github.event.inputs.environment }}"
 2. Commit & Push
git add .
git commit -m "added manual workflow"
git push
 3. Run Manually (Important Step)
Go to your GitHub repo
Click Actions tab
Find workflow → Manual Trigger Workflow
Click Run workflow button
Enter value:
staging OR production
Click Run workflow
 4. Verify Output
Open the workflow run
Click job → step logs

 You should see:



Task 4: Matrix Builds
Create .github/workflows/matrix.yml that:

Uses a matrix strategy to run the same job across:
Python versions: 3.10, 3.11, 3.12
Each job installs Python and prints the version
Watch all 3 run in parallel
Then extend the matrix to also include 2 operating systems — how many total jobs run now?
1. Create Workflow File

Create:

.github/workflows/matrix.yml

Add this:

name: Matrix Build

on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        python-version: [3.10, 3.11, 3.12]

    steps:
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: ${{ matrix.python-version }}

      - name: Print Python version
        run: python --version
 2. Commit & Push
git add .
git commit -m "added matrix workflow"
git push
 3. What You’ll See
Go to Actions tab
You’ll see 3 jobs running in parallel:
Python 3.10
Python 3.11
Python 3.12
 Concept (Very Important)

Matrix = run same job with multiple configurations

 Here:

1 job × 3 Python versions = 3 parallel jobs
 4. Extend with OS Matrix

Update your workflow:

strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    python-version: [3.10, 3.11, 3.12]

runs-on: ${{ matrix.os }}



Task 5: Exclude & Fail-Fast
In your matrix, exclude one specific combination (e.g., Python 3.10 on Windows)
Set fail-fast: false — trigger a failure in one job and observe what happens to the rest
Write in your notes: What does fail-fast: true (the default) do vs false?
1. Update Your Matrix Workflow

Modify your .github/workflows/matrix.yml like this:

name: Matrix Build with Exclude

on:
  push:

jobs:
  build:
    strategy:
      fail-fast: false   
      matrix:
        os: [ubuntu-latest, windows-latest]
        python-version: [3.10, 3.11, 3.12]

        exclude:
          - os: windows-latest
            python-version: 3.10   

    runs-on: ${{ matrix.os }}

    steps:
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: ${{ matrix.python-version }}

      - name: Print Python version
        run: python --version

      - name: Force failure (for testing)
        run: |
          if [ "${{ matrix.python-version }}" = "3.11" ]; then
            echo "Failing this job intentionally"
            exit 1
          fi




