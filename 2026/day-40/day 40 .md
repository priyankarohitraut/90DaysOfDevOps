Task 1: Set Up
Create a new public GitHub repository called github-actions-practice
Clone it locally
Create the folder structure: .github/workflows/
i will done

Task 2: Hello Workflow
Create .github/workflows/hello.yml with a workflow that:

Triggers on every push
Has one job called greet
Runs on ubuntu-latest
Has two steps:
Step 1: Check out the code using actions/checkout
Step 2: Print Hello from GitHub Actions!
Push it. Go to the Actions tab on GitHub and watch it run.

Verify: Is it green? Click into the job and read every step.
i will done


Task 3: Understand the Anatomy
Look at your workflow file and write in your notes what each key does:

on:
jobs:
runs-on:
steps:
uses:
run:
name: (on a step)
i will done
Defines the event that triggers the workflow.

Example:

on: push

 Runs pipeline whenever code is pushed

🔹 jobs:

What it does:
Defines one or more jobs (tasks) that the workflow will run.

 Each job runs independently

🔹 runs-on:

What it does:
Specifies the type of machine (runner) where the job will run.

Example:

runs-on: ubuntu-latest

 Uses a Linux machine provided by GitHub

🔹 steps:

What it does:
Defines a list of steps inside a job.

 Steps run one-by-one in order

🔹 uses:

What it does:
Used to run a pre-built action (reusable code).

Example:

uses: actions/checkout@v3

 Downloads your repository code

🔹 run:

What it does:
Executes a command in the runner.

Example:

run: echo "Hello"

 Runs shell command

🔹 name: (on a step)

What it does:
Gives a readable name to a step (for logs/UI).

Example:

- name: Print message

 Helps you understand logs easily

 One-Line Summary (Very Important)



on → when to run
jobs → what to run
runs-on → where to run
steps → how to run
uses → reuse actions
run → execute commands


Task 4: Add More Steps
Update hello.yml to also:

Print the current date and time
Print the name of the branch that triggered the run (hint: GitHub provides this as a variable)
List the files in the repo
Print the runner's operating system
Push again — watch the new run.
Updated hello.yml

Replace your file with this:

name: Hello Workflow

on:
  push:

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Print hello
        run: echo "Hello from GitHub Actions!"

      - name: Print date and time
        run: date

      - name: Print branch name
        run: echo "Branch: ${{ github.ref_name }}"

      - name: List files in repo
        run: ls -la

      - name: Print runner OS
        run: echo "Running on: $RUNNER_OS"
Push Changes
git add .
git commit -m "Added more steps to workflow"
git push origin main


Task 5: Break It On Purpose
Add a step that runs a command that will fail (e.g., exit 1 or a misspelled command)
Push and observe what happens in the Actions tab
Fix it and push again
Write in your notes: What does a failed pipeline look like? How do you read the error?

