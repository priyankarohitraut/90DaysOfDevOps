Task 1: GitHub Secrets
🔹 Workflow
name: Secrets Demo

on: push

jobs:
  secrets-job:
    runs-on: ubuntu-latest

    steps:
      - name: Check secret exists
        run: |
          if [ -z "${{ secrets.MY_SECRET_MESSAGE }}" ]; then
            echo "The secret is set: false"
          else
            echo "The secret is set: true"
          fi

      - name: Try printing secret (masked)
        run: echo "Secret value: ${{ secrets.MY_SECRET_MESSAGE }}"
 What happens?

 GitHub will show:

Secret value: ***

 Secrets are automatically masked

 Notes (Important)

 Why never print secrets?

Logs are visible to others
Security risk (token/password leak)
Can be exploited if exposed
 Task 2: Use Secrets as Environment Variables
- name: Use secret safely
  env:
    SECRET_MSG: ${{ secrets.MY_SECRET_MESSAGE }}
  run: echo "Using secret without exposing it"
 Add These Secrets (Important for future)

Go to Settings → Secrets → Actions and add:

DOCKER_USERNAME
DOCKER_TOKEN
 Task 3: Upload Artifacts
name: Artifact Upload

on: push

jobs:
  upload:
    runs-on: ubuntu-latest

    steps:
      - name: Create file
        run: echo "This is a report" > report.txt

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: report
          path: report.txt
 Verify
Go to Actions tab
Open workflow run
Scroll → Artifacts
Download file 
 Task 4: Share Artifacts Between Jobs
name: Artifact Sharing

on: push

jobs:
  generate:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Hello from Job 1" > file.txt

      - uses: actions/upload-artifact@v4
        with:
          name: shared-file
          path: file.txt

  consume:
    runs-on: ubuntu-latest
    needs: generate

    steps:
      - uses: actions/download-artifact@v4
        with:
          name: shared-file

      - run: cat file.txt
 Notes

 When to use artifacts?

Share files between jobs
Store build outputs
Save test reports/logs
Task 5: Run Real Tests in CI
Example (Python)
name: Run Script

on: push

jobs:
  test-script:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install Python
        uses: actions/setup-python@v4
        with:
          python-version: 3.11

      - name: Run script
        run: python script.py
 Test Behavior
Script exits 0 →  success (green)
Script exits 1 →  failure (red)

 Try:

exit(1)
 Task 6: Caching
name: Cache Demo

on: push

jobs:
  cache-job:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Cache pip
        uses: actions/cache@v4
        with:
          path: ~/.cache/pip
          key: pip-cache-${{ runner.os }}-${{ hashFiles('**/requirements.txt') }}

      - name: Install deps
        run: pip install -r requirements.txt
