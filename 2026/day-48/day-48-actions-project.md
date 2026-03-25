
Task 1: Set Up the Project Repo
Create a new repo called github-actions-capstone (or use your existing github-actions-practice)
Add a simple app — pick any one:
A Python Flask/FastAPI app with one endpoint
A Node.js Express app with one endpoint
Your Dockerized app from Day 36
Add a Dockerfile and a basic test (even a script that curls the health endpoint counts)
Add a README.md with a project description
Step 1: Create Repo

Create repo:

github-actions-capstone

OR use existing:

github-actions-practice
 Step 2: Add Simple App (Python Flask)
 app.py
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from GitHub Actions Capstone!"

@app.route("/health")
def health():
    return {"status": "ok"}

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
 requirements.txt
flask
 Step 3: Add Dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
 Step 4: Add Basic Test
 test.sh
#!/bin/bash

echo "Starting app..."
python app.py &

sleep 3

echo "Testing /health endpoint..."
STATUS=$(curl -s http://localhost:5000/health)

echo "Response: $STATUS"

if [[ "$STATUS" == *"ok"* ]]; then
  echo "Test passed "
  exit 0
else
  echo "Test failed "
  exit 1
fi

 Make executable:

chmod +x test.sh
 Step 5: README.md
Writing
 GitHub Actions Capstone Project
 Overview

This is a simple Flask application used to demonstrate a complete CI/CD pipeline using GitHub Actions.

 Tech Stack
Python (Flask)
Docker
GitHub Actions
 Run Locally
pip install -r requirements.txt
python app.py

App runs at:

http://localhost:5000
 Test
./test.sh
 Docker

Build image:

docker build -t capstone-app .

Run container:

docker run -p 5000:5000 capstone-app
 Features
Simple API with /health endpoint
Dockerized application
Ready for CI/CD pipeline

Built as part of DevOps learning 

Step 6: Commit & Push
git add .
git commit -m "Initial capstone project setup"
git push origin main
 Verify
Repo has:
app.py
Dockerfile
test.sh
README.md



Task 2: Reusable Workflow — Build & Test
Create .github/workflows/reusable-build-test.yml:

Trigger: workflow_call
Inputs: python_version (or node_version), run_tests (boolean, default: true)
Steps:
Check out code
Set up the language runtime
Install dependencies
Run tests (only if run_tests is true)
Set output: test_result with value passed or failed
This workflow does NOT deploy — it only builds and tests.
Create File
.github/workflows/reusable-build-test.yml
 Reusable Workflow (Build & Test)
name: Reusable Build & Test

on:
  workflow_call:
    inputs:
      python_version:
        required: true
        type: string
      run_tests:
        required: false
        type: boolean
        default: true

    outputs:
      test_result:
        description: "Test result status"
        value: ${{ jobs.build-test.outputs.test_result }}

jobs:
  build-test:
    runs-on: ubuntu-latest

    outputs:
      test_result: ${{ steps.set-result.outputs.result }}

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: ${{ inputs.python_version }}

      - name: Install dependencies
        run: |
          if [ -f requirements.txt ]; then
            pip install -r requirements.txt
          fi

      - name: Run tests
        id: run-tests
        if: ${{ inputs.run_tests }}
        continue-on-error: true
        run: |
          if [ -f test.sh ]; then
            chmod +x test.sh
            ./test.sh
          else
            echo "No test script found"
          fi

      - name: Set test result
        id: set-result
        run: |
          if [ "${{ steps.run-tests.outcome }}" = "success" ]; then
            echo "result=passed" >> $GITHUB_OUTPUT
          else
            echo "result=failed" >> $GITHUB_OUTPUT
          fi
 What This Does
🔹 Inputs
python_version → choose runtime
run_tests → control test



Task 3: Reusable Workflow — Docker Build & Push
Create .github/workflows/reusable-docker.yml:

Trigger: workflow_call
Inputs: image_name (string), tag (string)
Secrets: docker_username, docker_token
Steps:
Check out code
Log in to Docker Hub
Build and push the image with the given tag
Set output: image_url with the full image path
Create File
.github/workflows/reusable-docker.yml
 Reusable Workflow — Docker Build & Push
name: Reusable Docker Build & Push

on:
  workflow_call:
    inputs:
      image_name:
        required: true
        type: string
      tag:
        required: true
        type: string

    secrets:
      docker_username:
        required: true
      docker_token:
        required: true

    outputs:
      image_url:
        description: "Full Docker image path"
        value: ${{ jobs.docker.outputs.image_url }}

jobs:
  docker:
    runs-on: ubuntu-latest

    outputs:
      image_url: ${{ steps.set-output.outputs.image }}

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Login to Docker Hub
        run: |
          echo "${{ secrets.docker_token }}" | docker login -u "${{ secrets.docker_username }}" --password-stdin

      - name: Build image
        run: |
          docker build -t ${{ secrets.docker_username }}/${{ inputs.image_name }}:${{ inputs.tag }} .

      - name: Push image
        run: |
          docker push ${{ secrets.docker_username }}/${{ inputs.image_name }}:${{ inputs.tag }}

      - name: Set image output
        id: set-output
        run: |
          echo "image=${{ secrets.docker_username }}/${{ inputs.image_name }}:${{ inputs.tag }}" >> $GITHUB_OUTPUT
 What This Does
🔹 Inputs
image_name → repo name (e.g., capstone-app)
tag → version (e.g., latest, sha-123abc)
🔹 Secrets
docker_username
docker_token


Task 4: PR Pipeline
Create .github/workflows/pr-pipeline.yml:

Trigger: pull_request to main (types: opened, synchronize)
Call the reusable build-test workflow:
Run tests: true
Add a standalone job pr-comment that:
Runs after the build-test job
Prints a summary: "PR checks passed for branch: <branch>"
Do NOT build or push Docker images on PRs
Verify: Open a PR — does it run tests only (no Docker push)?
Create File
.github/workflows/pr-pipeline.yml
 PR Pipeline Workflow
name: PR Pipeline

on:
  pull_request:
    branches:
      - main
    types: [opened, synchronize]

jobs:
  build-test:
    uses: ./.github/workflows/reusable-build-test.yml
    with:
      python_version: "3.11"
      run_tests: true

  pr-comment:
    runs-on: ubuntu-latest
    needs: build-test

    steps:
      - name: Print PR summary
        run: echo "PR checks passed for branch: ${{ github.head_ref }}"
 What This Does
🔹 Trigger
Runs ONLY on PR to main
Events:
PR created (opened)
PR updated (synchronize)
🔹 Jobs
1. build-test (Reusable)
Runs your reusable workflow
Executes tests 
No Docker build 
2. pr-comment
Runs after build-test (needs)
Prints:
Task 5: Main Branch Pipeline
Create .github/workflows/main-pipeline.yml:

Trigger: push to main
Job 1: Call the reusable build-test workflow
Job 2 (depends on Job 1): Call the reusable Docker workflow
Tag: latest and sha-<short-commit-hash>
Job 3 (depends on Job 2): deploy job that:
Prints "Deploying image: <image_url> to production"
Uses environment: production (set this up in repo Settings → Environments)
Requires manual approval if you've set up environment protection rules
Verify: Merge a PR to main — does it run tests → build Docker → deploy in sequence?

Task 6: Scheduled Health Check
Create .github/workflows/health-check.yml:

Trigger: schedule with cron '0 */12 * * *' (every 12 hours) + workflow_dispatch for manual testing
Steps:
Pull your latest Docker image
Run the container in detached mode
Wait 5 seconds, then curl the health endpoint
Print pass/fail based on the response
Stop and remove the container
Add a step that creates a summary using $GITHUB_STEP_SUMMARY:
echo "## Health Check Report" >> $GITHUB_STEP_SUMMARY
echo "- Image: myapp:latest" >> $GITHUB_STEP_SUMMARY
echo "- Status: PASSED" >> $GITHUB_STEP_SUMMARY
echo "- Time: $(date)" >> $GITHUB_STEP_SUMMARY
Create File
.github/workflows/health-check.yml
 Scheduled Health Check Workflow
name: Scheduled Health Check

on:
  schedule:
    - cron: '0 */12 * * *'   # every 12 hours
  workflow_dispatch:

jobs:
  health-check:
    runs-on: ubuntu-latest

    steps:
      - name: Pull latest Docker image
        run: docker pull ${{ secrets.DOCKER_USERNAME }}/capstone-app:latest

      - name: Run container
        run: |
          docker run -d -p 5000:5000 --name health-container \
          ${{ secrets.DOCKER_USERNAME }}/capstone-app:latest

      - name: Wait for app to start
        run: sleep 5

      - name: Check health endpoint
        id: health
        run: |
          STATUS=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:5000/health)
          echo "HTTP Status: $STATUS"

          if [ "$STATUS" -eq 200 ]; then
            echo "status=PASSED" >> $GITHUB_OUTPUT
          else
            echo "status=FAILED" >> $GITHUB_OUTPUT
            exit 1
          fi

      - name: Stop and remove container
        if: always()
        run: |
          docker stop health-container || true
          docker rm health-container || true

      - name: Generate summary
        if: always()
        run: |
          echo "## Health Check Report" >> $GITHUB_STEP_SUMMARY
          echo "- Image: ${{ secrets.DOCKER_USERNAME }}/capstone-app:latest" >> $GITHUB_STEP_SUMMARY
          echo "- Status: ${{ steps.health.outputs.status }}" >> $GITHUB_STEP_SUMMARY
          echo "- Time: $(date)" >> $GITHUB_STEP_SUMMARY


Task 7: Add Badges & Documentation
Add status badges for all your workflows to the repo README.md
Add a pipeline architecture diagram in your notes — draw (or describe) the flow:
PR opened → build & test → PR checks pass
Merge to main → build & test → Docker build & push → deploy
Every 12 hours → health check
Fill in your notes: What would you add next? (Slack notifications? Multi-environment? Rollback?)
Brownie Points: Add Security to Your Pipeline
Want to go above and beyond? Add a DevSecOps step to your main pipeline:

Add aquasecurity/trivy-action after the Docker build step to scan your image for vulnerabilities
Fail the pipeline if any CRITICAL severity CVE is found
Upload the scan report as an artifact
This is a preview of what you'll do in depth on Day 49. If you get this working today, you're already thinking like a DevSecOps engineer.
1. Add Status Badges to README.md

Add this at the top of your README.md:

# GitHub Actions Capstone

![PR Pipeline](https://github.com/priyankaraut19/github-actions-capstone/actions/workflows/pr-pipeline.yml/badge.svg)
![Main Pipeline](https://github.com/priyankaraut19/github-actions-capstone/actions/workflows/docker-publish.yml/badge.svg)
![Health Check](https://github.com/priyankaraut19/github-actions-capstone/actions/workflows/health-check.yml/badge.svg)

 Replace workflow names if different.

 2. Pipeline Architecture (Write in Notes)
 Flow Diagram (Text Version)
PR Opened
   ↓
PR Pipeline
   ↓
Build & Test
   ↓
PR Checks Pass 

------------------------

Merge to main
   ↓
Main Pipeline
   ↓
Build & Test
   ↓
Docker Build
   ↓
Docker Push (Docker Hub)
   ↓
Deploy (future step)

------------------------

Every 12 Hours
   ↓
Health Check Workflow
   ↓
Pull Image
   ↓
Run Container
   ↓
Check /health endpoint
   ↓
PASS / FAIL report
 3. What Would You Add Next? (Interview Answer)

 Strong answers:

 Slack / Email notifications
 Multi-environment deploy (dev → staging → prod)
 Rollback strategy (previous image tag)
 Secrets management (Vault / AWS Secrets Manager)
 Monitoring (Prometheus, Grafana)
 Approval gates before production
