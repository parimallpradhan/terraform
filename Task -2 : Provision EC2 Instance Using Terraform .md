# 🟢 TASK 2 — Provision EC2 Instance Using Terraform

## 🎯 Goal of This Task

By the end of this task, you will:

* Create an EC2 instance using Terraform
* Understand Terraform resource block
* Learn AMI, instance type, tags
* Execute `plan` and `apply`
* Understand Terraform state file
* Be able to explain this in interviews confidently

---

# 🧠 Real-Time Scenario (Interview Answer Context)

> “In my project, we automated EC2 provisioning using Terraform to eliminate manual AWS console creation. This ensured consistency across dev, QA, and production environments.”

---

# 🏗️ Architecture Flow

```text id="ec2flow1"
Terraform Code (main.tf)
        ↓
terraform init
        ↓
terraform plan
        ↓
terraform apply
        ↓
AWS EC2 Instance Created
        ↓
State stored in terraform.tfstate
```

---

# 🛠️ STEP 1 — Create Project Directory

```bash id="dir1"
mkdir terraform-ec2
cd terraform-ec2
```

---

# 🧾 STEP 2 — Create Terraform File

```bash id="file1"
vim main.tf
```

---

# ⚙️ STEP 3 — Terraform EC2 Configuration

Paste this code:

```hcl id="ec2code1"
provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "devops_ec2" {
  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"

  tags = {
    Name = "DevOps-Terraform-EC2"
    Environment = "Dev"
  }
}
```

---

# 🧠 Explanation (Very Important for Interview)

## 🔹 provider "aws"

* Tells Terraform to use AWS cloud
* Region defines where infrastructure will be created

## 🔹 resource "aws_instance"

* Defines EC2 instance creation

## 🔹 ami

* Amazon Machine Image (OS template)

## 🔹 instance_type

* Hardware size (t2.micro = free tier)

## 🔹 tags

* Metadata for identification

---

# 🚀 STEP 4 — Initialize Terraform

```bash id="init1"
terraform init
```

### What happens:

* Downloads AWS provider plugin
* Creates `.terraform` directory
* Prepares working directory

---

# 🔍 STEP 5 — Validate Configuration

```bash id="validate1"
terraform validate
```

Expected:

```text id="validout"
Success! The configuration is valid.
```

---

# 📊 STEP 6 — Run Terraform Plan

```bash id="plan1"
terraform plan
```

### What you see:

* Terraform shows EC2 will be created
* No changes applied yet

---

# ⚡ STEP 7 — Apply Infrastructure

```bash id="apply1"
terraform apply
```

Type:

```text id="confirm1"
yes
```

---

# 🎉 EXPECTED OUTPUT

* EC2 instance created in AWS
* Instance visible in AWS Console

---

# 🧠 STEP 8 — Verify EC2 in AWS

Go to:

[AWS EC2 Console](https://console.aws.amazon.com/ec2/?utm_source=chatgpt.com)

Check:

* Instance running
* Name: DevOps-Terraform-EC2

---

# 📁 STEP 9 — Terraform State File

After apply:

```bash id="state1"
ls
```

You will see:

```text id="stateout"
main.tf
terraform.tfstate
```

---

# 🧠 Interview Explanation (VERY IMPORTANT)

> “Terraform state file maintains mapping between real infrastructure and Terraform configuration. It tracks EC2 instance ID, attributes, and metadata.”

---

# 🔥 STEP 10 — Destroy Infrastructure (Cleanup)

```bash id="destroy1"
terraform destroy
```

Type:

```text id="destroyconfirm"
yes
```

---

# ⚠️ Why destroy is important?

* Avoid AWS charges
* Clean environment
* Testing lifecycle management

---

# 🧯 COMMON ERRORS & FIXES

## ❌ Error 1: Invalid AMI

```text id="errami"
InvalidAMIID.NotFound
```

### Fix:

* Change AMI based on region
* Use correct Ubuntu AMI

---

## ❌ Error 2: Permission Denied

```text id="errperm"
UnauthorizedOperation
```

### Fix:

* Check IAM permissions
* Attach EC2 full access

---

## ❌ Error 3: Region mismatch

```text id="errregion"
Invalid region
```

### Fix:

* Ensure region = ap-south-1

---

# 💬 Interview Explanation (Must Memorize)

> “I used Terraform to provision EC2 instances in AWS by defining resource blocks in HCL. The workflow included terraform init, plan, and apply. Once applied, Terraform created EC2 instances and maintained their state in terraform.tfstate file. This helped automate infrastructure provisioning and ensure consistency across environments.”

---

# 🔥 WHAT YOU LEARNED IN TASK 2

✔ Terraform provider setup
✔ EC2 resource creation
✔ HCL syntax
✔ Terraform lifecycle
✔ State file concept
✔ Plan vs Apply
✔ AWS integration
✔ Real-world automation

---

# 🚀 BEFORE MOVING TO TASK 3

Make sure you can answer:

1. What is terraform resource block?
2. What is AMI?
3. What happens in terraform apply?
4. What is state file?
5. How EC2 is created using Terraform?

---

# 👉 NEXT STEP

If Task 2 is done successfully, we will move to:

# 🟡 TASK 3 — Variables & Outputs (VERY IMPORTANT for interviews)

Where you will learn:

* Input variables
* Output values
* Dynamic infrastructure
* Reusability concept
* Real project usage

---

Just say:

👉 **“TASK 3”**

and we will continue like real DevOps production training.
