# TASK 1 — Install Terraform & Configure AWS Authentication

This is the most important foundational task.

---

# Goal of This Task

By the end of this task, you will:

* Install Terraform
* Install AWS CLI
* Configure AWS authentication
* Verify AWS connectivity
* Understand Terraform workflow
* Be ready for infrastructure provisioning

---

# Real-Time Project Scenario

> “Our organization wanted Infrastructure as Code implementation using Terraform.
>
> As a DevOps Engineer, the first responsibility was setting up Terraform environment securely with AWS authentication before automating infrastructure deployment.”

---

# Architecture Flow

```text id="lvf7tz"
Local Machine / EC2 VM
        ↓
Terraform Installed
        ↓
AWS CLI Configured
        ↓
IAM Authentication
        ↓
Terraform Communicates with AWS APIs
```

---

# Prerequisites

You need:

* Ubuntu VM / EC2 instance
* AWS account
* Internet access
* Sudo access

---

# STEP 1 — Verify OS Information

Run:

```bash id="vtm2i2"
cat /etc/os-release
```

Expected:

```text id="uygx95"
Ubuntu 22.04
```

---

# STEP 2 — Update Packages

Run:

```bash id="mlv6vg"
sudo apt update -y
```

---

# STEP 3 — Install Required Packages

Run:

```bash id="8mqj8s"
sudo apt install -y gnupg software-properties-common curl unzip
```

---

# STEP 4 — Install Terraform

---

## Add HashiCorp GPG Key

Run:

```bash id="m6nklm"
curl -fsSL https://apt.releases.hashicorp.com/gpg | \
sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
```

---

## Add HashiCorp Repository

Run:

```bash id="5w2pf6"
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list
```

---

## Update Repository

Run:

```bash id="2lqlf3"
sudo apt update
```

---

## Install Terraform

Run:

```bash id="0e7ehs"
sudo apt install terraform -y
```

---

# STEP 5 — Verify Terraform Installation

Run:

```bash id="rp2yzr"
terraform version
```

Expected Output:

```text id="vfz6h6"
Terraform v1.x.x
```

---

# Interview Explanation

> “I installed Terraform using HashiCorp official repository to ensure stable provider and version management.”

---

# STEP 6 — Install AWS CLI

Run:

```bash id="t1t0g7"
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

---

## Unzip AWS CLI

```bash id="vl55g6"
unzip awscliv2.zip
```

---

## Install AWS CLI

```bash id="fgk6kb"
sudo ./aws/install
```

---

# STEP 7 — Verify AWS CLI Installation

Run:

```bash id="g0lmkf"
aws --version
```

Expected:

```text id="t40q57"
aws-cli/2.x.x
```

---

# STEP 8 — Create IAM User in AWS

VERY IMPORTANT.

---

# Real-Time Best Practice

Never use:

* Root user
* Personal admin credentials

Always create:

* Dedicated Terraform IAM user

---

# Create IAM User

Go to:

[AWS Console](https://console.aws.amazon.com/?utm_source=chatgpt.com)

Then:

```text id="4plhzr"
IAM
 ↓
Users
 ↓
Create User
```

---

# User Details

Example:

```text id="2l37y7"
Username: terraform-admin
```

---

# Attach Permissions

For learning purpose attach:

```text id="u4n1bz"
AdministratorAccess
```

NOTE:
In real companies:

* Least privilege policy is used.

---

# Create Access Key

After user creation:

```text id="fudx7s"
Security Credentials
    ↓
Create Access Key
```

Save:

* Access Key ID
* Secret Access Key

VERY IMPORTANT.

---

# STEP 9 — Configure AWS CLI Authentication

Run:

```bash id="rx5tz5"
aws configure
```

Enter:

```text id="mj4jk3"
AWS Access Key ID
AWS Secret Access Key
Region: ap-south-1
Output format: json
```

---

# STEP 10 — Verify AWS Authentication

Run:

```bash id="sp0r6y"
aws s3 ls
```

OR

```bash id="r7s4kp"
aws ec2 describe-regions
```

If successful:
AWS authentication is working.

---

# Real-Time Explanation

> “Terraform internally uses AWS API calls. So proper AWS authentication through IAM user and AWS CLI configuration is required before provisioning infrastructure.”

---

# STEP 11 — Create Terraform Working Directory

Run:

```bash id="37o44w"
mkdir terraform-project
cd terraform-project
```

---

# STEP 12 — Create First Terraform File

Create file:

```bash id="7tzz8o"
vim main.tf
```

Add:

```hcl id="s20fjh"
provider "aws" {
  region = "ap-south-1"
}
```

Save file.

---

# STEP 13 — Initialize Terraform

Run:

```bash id="d1m69d"
terraform init
```

---

# What Happens Internally?

Terraform:

* Downloads AWS provider
* Creates `.terraform` directory
* Initializes working directory

---

# Expected Output

```text id="ehswv0"
Terraform has been successfully initialized!
```

---

# Interview Explanation

> “terraform init initializes Terraform working directory and downloads required providers/plugins.”

---

# STEP 14 — Validate Terraform Configuration

Run:

```bash id="jlwmup"
terraform validate
```

Expected:

```text id="6mfruk"
Success! The configuration is valid.
```

---

# STEP 15 — Run Terraform Plan

Run:

```bash id="bq03zw"
terraform plan
```

---

# What Plan Does

Terraform compares:

```text id="oqi5uy"
Desired State
VS
Current Infrastructure State
```

and shows changes before deployment.

---

# Interview Explanation

> “terraform plan provides execution preview before actual infrastructure deployment.”

---

# Important Terraform Workflow

Remember this forever:

```text id="r1rvyj"
terraform init
terraform validate
terraform plan
terraform apply
terraform destroy
```

---

# Common Errors and Fixes

---

# Error 1 — AWS Authentication Failed

```text id="mx6mjlwm"
NoCredentialProviders
```

## Fix

Run:

```bash id="gdh0qr"
aws configure
```

---

# Error 2 — Permission Denied

```text id="wl01vx"
AccessDenied
```

## Fix

Attach proper IAM permissions.

---

# Error 3 — Region Error

```text id="v6b1af"
Invalid region
```

## Fix

Use valid AWS region:

```text id="gn66tw"
ap-south-1
```

---

# Interview Questions From This Task

---

# Q1 — What is terraform init?

## Answer

> “terraform init initializes Terraform working directory and downloads required providers.”

---

# Q2 — Why AWS CLI configuration required?

## Answer

> “Terraform communicates with AWS APIs, so authentication through IAM credentials is required.”

---

# Q3 — Why not use root user?

## Answer

> “Using root user is security risk. Best practice is dedicated IAM user with least privilege access.”

---

# Q4 — What happens during terraform plan?

## Answer

> “Terraform compares desired configuration with current infrastructure state and shows execution changes before deployment.”

---

# Real-Time Experience You Can Mention

> “I configured Terraform environment securely using IAM authentication and AWS CLI before implementing infrastructure automation.”

---

# TASK 1 Completion Checklist

You should now have:

* Terraform installed
* AWS CLI installed
* IAM user configured
* AWS authentication working
* Terraform initialized
* Basic Terraform workflow understood

---

# DO THIS NOW

Implement all steps carefully.

After completion:

1. Share:

```bash id="qf7w0i"
terraform version
aws --version
terraform init
terraform validate
```

outputs/errors/screenshots if any.

2. Then I will give you:

# TASK 2 — Provision EC2 Instance Using Terraform

Where you will learn:

* Resource blocks
* EC2 creation
* terraform apply
* terraform destroy
* State file
* Real infrastructure provisioning
* Production interview explanation
