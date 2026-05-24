# 🟣 TASK 11 — Terraform Import + Drift Handling (Production Scenarios)

## 🎯 Goal of This Task

By the end of this task, you will:

* Understand infrastructure drift
* Detect manual changes in AWS
* Fix state vs real infra mismatch
* Use `terraform refresh` / `plan` effectively
* Import real AWS resources into Terraform
* Explain production incident handling confidently

---

# 🧠 Real-Time Scenario (Interview Context)

> “In production, sometimes engineers or automation tools make manual changes in AWS console. This creates drift between Terraform state and actual infrastructure. We detect and fix this using Terraform plan, refresh, and import operations.”

---

# ⚠️ WHAT IS DRIFT? (VERY IMPORTANT)

## Drift Definition:

> When actual infrastructure in AWS is different from Terraform state file.

---

# 🏗️ DRIFT FLOW

```text id="driftflow"
Terraform Code (Desired State)
        ↓
Terraform State File
        ↓
AWS Infrastructure (Actual State)
        ↓
❌ Manual change happens in AWS Console
        ↓
💥 DRIFT OCCURS
```

---

# 🧩 PART 1 — Simulate Drift Scenario

We will assume EC2 already exists.

---

## STEP 1 — Create EC2 using Terraform

```bash id="dr1"
mkdir terraform-drift
cd terraform-drift
```

---

## main.tf

```hcl id="driftcode"
provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "demo" {
  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"

  tags = {
    Name = "Drift-EC2"
  }
}
```

---

## Apply

```bash id="drapply"
terraform init
terraform apply
```

---

# 🧩 STEP 2 — MANUAL CHANGE (DRIFT SIMULATION)

Go to AWS Console:

[AWS EC2 Console](https://console.aws.amazon.com/ec2/?utm_source=chatgpt.com)

---

## Change manually:

* Change tag Name → "Manual-Change-EC2"
* Stop instance manually OR change instance type

---

# 💥 NOW DRIFT HAPPENED

Terraform does NOT know about this change.

---

# 🔍 STEP 3 — Detect Drift

## Run:

```bash id="drplan"
terraform plan
```

---

## Output shows:

* Difference between state and AWS
* Terraform wants to revert changes

---

# 🧠 INTERVIEW EXPLANATION

> “Terraform plan detects drift by comparing actual AWS infrastructure with stored state file and shows differences.”

---

# 🔄 STEP 4 — Refresh State (IMPORTANT)

```bash id="refreshcmd"
terraform refresh
```

---

## What happens:

* Updates state file with real AWS data
* Syncs Terraform state with AWS

---

# 🧠 NOTE

👉 `terraform refresh` is now mostly replaced internally by plan/apply in newer versions, but still important conceptually.

---

# 🧩 PART 5 — Fix Drift (Reconcile State)

## Option 1 — Revert AWS to Terraform state

```bash id="fix1"
terraform apply
```

👉 This will overwrite manual changes

---

## Option 2 — Accept manual change

Update Terraform code to match AWS

---

# 🧩 PART 6 — terraform import (REAL SCENARIO)

---

## Scenario:

EC2 already exists in AWS but not in Terraform.

---

## STEP 1 — Write empty resource

```hcl id="import1"
resource "aws_instance" "imported" {}
```

---

## STEP 2 — Import resource

```bash id="import11"
terraform import aws_instance.imported i-0123456789abcdef
```

---

## STEP 3 — Verify

```bash id="showimport"
terraform state list
```

---

# 🧠 WHAT HAPPENS

* AWS resource linked to Terraform
* Now Terraform controls it
* No recreation happens

---

# 🧠 INTERVIEW EXPLANATION

> “terraform import is used to bring existing AWS resources under Terraform management without recreating them.”

---

# 🔥 REAL PRODUCTION USE CASES

✔ Manual AWS console changes
✔ Legacy infrastructure onboarding
✔ Accidentally unmanaged resources
✔ Cloud migration projects
✔ Disaster recovery setups

---

# 🧯 COMMON ERRORS & FIXES

---

## ❌ Error 1: Drift not detected

### Fix:

* Run terraform plan with refresh enabled

---

## ❌ Error 2: Import failed

### Fix:

* Verify correct instance ID
* Check IAM permissions

---

## ❌ Error 3: State mismatch

### Fix:

* Use terraform refresh or re-apply

---

# 💬 INTERVIEW QUESTIONS FROM TASK 11

---

## ❓ What is Terraform drift?

> “Drift occurs when actual infrastructure differs from Terraform state file.”

---

## ❓ How do you detect drift?

* terraform plan
* terraform refresh

---

## ❓ How do you fix drift?

* terraform apply
* update code
* import resource

---

## ❓ What is terraform import used for?

> “It is used to bring existing infrastructure into Terraform state.”

---

# 🔥 REAL PROJECT EXPLANATION

> “In my project, we detected drift when AWS resources were modified manually. We used terraform plan to identify changes and terraform apply to restore desired state. In some cases, we used terraform import to bring unmanaged resources under Terraform control.”

---

# 🧠 WHAT YOU LEARNED IN TASK 11

✔ Drift concept
✔ Detecting infrastructure mismatch
✔ State reconciliation
✔ Terraform import usage
✔ Production troubleshooting
✔ Incident handling skills

---

# 🚀 NEXT STEP (IMPORTANT)

Now you are moving into **advanced Terraform lifecycle control**

Next:

# 🟣 TASK 12 — Terraform Provisioners (File, Remote Exec, Local Exec)

You will learn:

* Real automation after infra creation
* Script execution
* Configuration automation
* Real DevOps workflows

---

