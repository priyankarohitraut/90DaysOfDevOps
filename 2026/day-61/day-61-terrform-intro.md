Task 1: Understand Infrastructure as Code (IaC)
1) What is Infrastructure as Code (IaC)?

Infrastructure as Code means managing servers, storage, networking, and cloud resources using code files instead of manually clicking in the cloud console.

Example:
Instead of manually creating an EC2 instance in AWS console, we write:

resource "aws_instance" "web" {
  instance_type = "t2.micro"
}

Terraform reads this code and creates the resource.

2) Why does it matter in DevOps?

IaC is important because it brings:

automation → no manual work
consistency → same setup every time
version control → store infra code in Git
reusability → use same code in multiple environments
faster deployment → infra in minutes

Interview answer:

“IaC helps automate infrastructure creation and ensures consistent, repeatable deployments across environments.”

3) Problems solved compared to manual AWS console

Manual creation causes:

human mistakes
inconsistent environments
difficult rollback
no version history
time-consuming setup

Example:
Dev and Prod may become different if created manually.

Terraform solves this using code.

4) Terraform vs Other Tools
Tool	Purpose
Terraform	Infrastructure provisioning
CloudFormation	AWS-only provisioning
Ansible	Configuration management
Pulumi	Infrastructure using programming languages
Simple difference
Terraform → create infra
Ansible → configure software inside servers
CloudFormation → AWS only
Pulumi → code in Python/JS/Go
5) Declarative + Cloud Agnostic
Declarative

You define what you want, not how.

Example:

resource "aws_instance" "web" {
  instance_type = "t2.micro"
}

You say desired state.

Terraform decides steps.

Cloud Agnostic

Same tool works across:

AWS
Azure
GCP
Kubernetes

That’s why Terraform is called cloud agnostic.

Task 2: Install Terraform and AWS CLI

Because this is software installation and version-sensitive, I should verify the latest Linux installation steps from the web.

For Ubuntu/Linux, use:

wget -O - https://apt.releases.hashicorp.com/gpg | \
sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update
sudo apt install terraform

Verify:

terraform -version

Expected:

Terraform v1.x.x
AWS CLI

Install AWS CLI and configure:

aws configure

Enter:

Access Key ID
Secret Access Key
Region: ap-south-1
Output: json

Verify:

aws sts get-caller-identity

Expected:

{
  "Account": "123456789",
  "Arn": "arn:aws:iam::..."
}
Task 3: First Terraform Config

Create project:

mkdir terraform-basics
cd terraform-basics

Create main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "ap-south-1"
}

resource "aws_s3_bucket" "mybucket" {
  bucket = "rohit-terraform-bucket-2026-demo"
}
Terraform Lifecycle
terraform init
terraform plan
terraform apply
What does init download?

terraform init downloads:

provider plugins
dependency files
lock file

Inside .terraform/

provider binaries
module cache
metadata
Task 4: Add EC2 Instance

Add in main.tf

resource "aws_instance" "web" {
  ami           = "ami-0f5ee92e2d63afc18"
  instance_type = "t2.micro"

  tags = {
    Name = "TerraWeek-Day1"
  }
}

Run:

terraform plan
terraform apply
Important Interview Question

How does Terraform know S3 already exists?

Answer:

“Terraform checks the terraform.tfstate file to compare current resources with desired configuration.”

This is a top interview question 

Task 5: Understand State File

Very important for interviews.

Open:

cat terraform.tfstate

This stores:

resource IDs
ARNs
IP addresses
metadata
dependencies
Commands
terraform show
terraform state list
terraform state show aws_s3_bucket.mybucket
terraform state show aws_instance.web
Why never edit state manually?

Because it can cause:

resource drift
broken infra mapping
accidental deletion
Why not commit to Git?

State file may contain:

resource IDs
public IPs
sensitive values
secrets

Always add:

terraform.tfstate
.terraform/

to .gitignore

Task 6: Modify + Destroy

Change:

Name = "TerraWeek-Day1"

to

Name = "TerraWeek-Modified"

Run:

terraform plan
Symbols Meaning
Symbol	Meaning
+	create
~	modify
-	destroy

Very common interview question.

Destroy Everything
terraform destroy

This removes:

S3 bucket
EC2 instance
