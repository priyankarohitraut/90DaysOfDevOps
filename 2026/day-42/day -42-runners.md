Task 1: GitHub-Hosted Runners
Workflow Summary

Created a workflow with 3 jobs running on:

ubuntu-latest
windows-latest
macos-latest

Each job prints:

OS name
Hostname
Current user
Sample YAML
name: Multi-OS Runner Test

on: push

jobs:
  ubuntu-job:
    runs-on: ubuntu-latest
    steps:
      - run: |
          echo "OS: Ubuntu"
          hostname
          whoami

  windows-job:
    runs-on: windows-latest
    steps:
      - run: |
          echo "OS: Windows"
          hostname
          whoami

  macos-job:
    runs-on: macos-latest
    steps:
      - run: |
          echo "OS: macOS"
          hostname
          whoami
 Notes
GitHub-hosted runner = A virtual machine provided by GitHub to run workflows
Managed by = GitHub (you don’t maintain it)
 Task 2: Explore Pre-installed Tools
Commands Used
- name: Check installed tools
  run: |
    docker --version
    python --version
    node --version
    git --version
 Notes
Runners come with many tools pre-installed (Docker, Node, Python, etc.)
Why it matters:
Saves setup time 
Faster pipelines 
No need to install dependencies manually
 Task 3: Self-Hosted Runner Setup
Steps Performed

Went to:

GitHub → Settings → Actions → Runners
Clicked New self-hosted runner
Selected Linux
Ran setup commands on local machine:
mkdir actions-runner && cd actions-runner
# Download + configure (commands provided by GitHub)
./config.sh
./run.sh
 Verification
Runner showed as Idle (green dot) in GitHub UI

 Add Screenshot Here

 Task 4: Use Self-Hosted Runner
Workflow File
name: Self Hosted Test

on: push

jobs:
  self-hosted-job:
    runs-on: self-hosted

    steps:
      - name: Show details
        run: |
          echo "Hostname:"
          hostname
          echo "Working Directory:"
          pwd

      - name: Create file
        run: echo "Hello from self-hosted runner" > test.txt
 Verification
Workflow ran on my machine
File test.txt created locally 

 Add Screenshot Here

 Task 5: Labels
Added Label
my-linux-runner
Updated Workflow
runs-on: [self-hosted, my-linux-runner]
 Notes
Labels help:
Target specific runners
Manage multiple machines
Control workloads efficiently
 Task 6: GitHub-Hosted vs Self-Hosted
Feature	GitHub-Hosted	Self-Hosted
Who manages it?	GitHub	You
Cost	Free (limited minutes)	Your infrastructure cost
Pre-installed tools	Yes (many tools ready)	You install/manage
Good for	Quick setup, standard CI	Custom setups, heavy workloads
Security concern	Managed securely by GitHub	You must secure it
