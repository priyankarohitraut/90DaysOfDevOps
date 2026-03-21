:

 1. What can go wrong?

When 5 developers are pushing code and deploying manually:

Code conflicts
Two developers change the same file → one overwrites the other’s work
Human errors
Wrong command
Deploying wrong version
Missing files or configs
No proper testing
Code goes to production without validation → bugs in live app
Environment mismatch
Dev, test, and production are different → unexpected failures
Downtime risk
Manual deployment may stop the app temporarily
No rollback plan
If something breaks → difficult to go back to previous version

 2. What does “It works on my machine” mean?

 It means:

The code runs perfectly on a developer’s local system but fails in another environment (like production).

 Why this is a real problem:
Different OS (Windows vs Linux)
Different software versions (Node, Python, Java)
Missing dependencies
Different environment variables
 Example:
Developer has Python 3.11 → works fine
Server has Python 3.8 → app crashes

 So the problem is lack of consistency
3. How many times can a team safely deploy manually?

 Realistically:

1–2 times per day (maximum)
Sometimes even once every few days
Why so limited?
Manual work takes time
High chance of mistakes
Needs coordination between team members
Risk increases with every deployment

Continuous Integration (CI)

Definition (2–3 lines):
Continuous Integration is the practice of frequently merging code changes into a shared repository (multiple times a day). Every change triggers automated builds and tests. It helps catch bugs, integration issues, and broken code early.

What it catches:

Build failures
Test failures
Code conflicts

Real-world example:
A developer pushes code to GitHub → pipeline runs automatically → builds app + runs tests → fails if something is broken.

🟢 Continuous Delivery (CD)

Definition (2–3 lines):
Continuous Delivery is an extension of CI where code is automatically prepared for release after passing all tests. The application is always in a deployable state, but deployment to production is done manually (with approval).

What “delivery” means:
Code is ready to release anytime, but not automatically released.

Real-world example:
After CI passes, the app is deployed to a staging environment → QA or manager clicks a button to release to production.

🟣 Continuous Deployment (CD)**

Definition (2–3 lines):
Continuous Deployment goes one step further than Continuous Delivery. Every change that passes all tests is automatically deployed to production without manual approval.

When teams use it:

High automation maturity
Strong testing (unit + integration + monitoring)

Real-world example:
A developer pushes code → tests pass → system automatically deploys to production → users see changes instantl


Pipeline Anatomy
🔹 Trigger

What it does:
A trigger is the event that starts the pipeline.

Examples:

Code push to GitHub
Pull request created
Scheduled time (cron job)

 Basically: “When should pipeline run?”

🔹 Stage

What it does:
A stage is a logical phase in the pipeline (group of jobs).

Common stages:

Build
Test
Deploy

 Think: big steps of the pipeline

🔹 Job

What it does:
A job is a unit of work inside a stage. Each job runs independently on a runner.

Example:

Build job
Unit test job

 Think: tasks inside a stage

🔹 Step

What it does:
A step is a single command or action inside a job.

Examples:

npm install
docker build
pytest

 Think: smallest action (one command)

🔹 Runner

What it does:
A runner is the machine (server/VM/container) that executes the jobs.

Examples:

GitHub-hosted runner
Self-hosted runner (your own server)

 Think: where the job actually runs

🔹 Artifact

What it does:
An artifact is the output produced by a job and stored for later use.

Examples:

Compiled app (.jar, .war)
Docker image
Test reports

 Think: result/output of pipeline

 Easy Way to Remember

 Trigger → Stage → Job → Step → Runner → Artifact

Trigger starts
Stage organizes
Job executes
Step runs commands
Runner provides machine
Artifact saves output


Hand-Drawn Pipeline (What to Draw)

Draw boxes connected with arrows like this:

[ Developer Pushes Code ]
              ↓
        (Trigger: Git Push)
              ↓
        ┌──────────────┐
        │   STAGE 1    │
        │    BUILD     │
        └──────────────┘
              ↓
   - Install dependencies
   - Build application

              ↓
        ┌──────────────┐
        │   STAGE 2    │
        │     TEST     │
        └──────────────┘
              ↓
   - Run unit tests
   - Run integration tests

              ↓
        ┌──────────────┐
        │   STAGE 3    │
        │    DOCKER    │
        └──────────────┘
              ↓
   - Build Docker image
   - Tag image

              ↓
        ┌──────────────┐
        │   STAGE 4    │
        │   DEPLOY     │
        └──────────────┘
              ↓
   - Push image to registry
   - Deploy to staging server
 Simple Explanation (for notes/interview)
Trigger: Code push to GitHub
Stage 1 (Build): App is compiled/prepared
Stage 2 (Test): Automated tests ensure code quality
Stage 3 (Docker): App is packaged into a Docker image
Stage 4 (Deploy): Image is deployed to staging serve




Answer:

Triggered on code push
Triggered on pull requests

 Means: runs whenever someone adds or updates code

🔹 2. How many jobs does it have?

Example:

jobs:
  test:
  lint:

Answer:

Usually 2–3 jobs
test job
lint job (code quality)

 Some projects also test on multiple Python versions (matrix jobs)

🔹 3. What does it do? (Best guess)

Inside jobs you’ll see steps like:

- uses: actions/checkout@v3
- name: Install dependencies
- name: Run tests

Answer:

Downloads the code
Installs dependencies
Runs tests
Checks code quality (linting)

 Final purpose:

Ensure new code doesn’t break the project

 How YOU should do this (important)

Pick any repo like:

Kubernetes (complex)
React (medium)
FastAPI (easy)

Then:

Open GitHub
Go to .github/workflows/
Open any .yml file
Just answer 3 things:
Trigger
Jobs
Purpose
