# 🟣 TASK 10 — Terraform State Commands (Production Troubleshooting)

## 🎯 Goal of This Task

By the end of this task, you will:

* Understand Terraform state deeply
* Inspect resources from state
* Remove resources safely from state
* Import existing AWS resources into Terraform
* Debug real production issues
* Answer critical interview questions confidently

---

# 🧠 Real-Time Scenario (Interview Context)

> “In production, sometimes resources are created manually or get out of sync. We use Terraform state commands to inspect, fix, import, or remove resources from state without destroying infrastructure.”

---

# ⚠️ WHY THIS IS CRITICAL

Terraform state is the **single source of truth**.

If state is wrong:

* Terraform may destroy real infrastructure ❌
* Drift issues happen ❌
* Deployment failures occur ❌

---

# 🏗️ STATE ARCHITECTURE

```text id="stateflow10"
Terraform Code
      ↓
Terraform State File (terraform.tfstate)
      ↓
Maps real AWS resources
      ↓
Used for plan/apply decisions
```

---

# 🧩 STEP 1 — Create Sample Resource (EC2)

We will reuse EC2 example.

```bash id="st1"
mkdir terraform-state-demo
cd terraform-state-demo
```

---

## main.tf

```hcl id="statecode"
provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "demo" {
  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"

  tags = {
    Name = "State-Demo-EC2"
  }
}
```

---

## Initialize & Apply

```bash id="stinit"
terraform init
terraform apply
```

Type:

```text id="yesst"
yes
```

---

# 📦 STEP 2 — View State File

```bash id="stfile"
cat terraform.tfstate
```

👉 You will see:

* EC2 ID
* AMI
* Region
* Attributes

---

# 🔍 COMMAND 1 — terraform state list

## Purpose:

List all resources in state

```bash id="listcmd"
terraform state list
```

### Output:

```text id="listout"
aws_instance.demo
```

---

# 🔍 COMMAND 2 — terraform state show

## Purpose:

Show detailed info of a resource

```bash id="showcmd"
terraform state show aws_instance.demo
```

### Output:

* instance_id
* private_ip
* tags
* AMI

---

# 🧠 INTERVIEW EXPLANATION

> “state show is used to inspect detailed attributes of a specific resource stored in Terraform state.”

---

# ❌ COMMAND 3 — terraform state rm (VERY IMPORTANT)

## Purpose:

Remove resource from state ONLY (NOT AWS)

```bash id="rmcmd"
terraform state rm aws_instance.demo
```

---

## What happens:

* Resource removed from Terraform state
* EC2 still exists in AWS
* Terraform stops managing it

---

# 🧠 REAL-TIME USE CASE

> “We use state rm when a resource is accidentally managed by Terraform or moved to another module.”

---

# ⚠️ IMPORTANT WARNING

Never confuse:

* state rm ❌ (removes from Terraform only)
* destroy ❌ (deletes real infrastructure)

---

# 📥 COMMAND 4 — terraform import (VERY IMPORTANT)

## Purpose:

Bring existing AWS resource into Terraform management

---

## Example Scenario:

EC2 already exists in AWS manually

Now we want Terraform to manage it.

---

### Step 1 — Write empty resource

```hcl id="importcode"
resource "aws_instance" "imported" {}
```

---

### Step 2 — Import EC2

```bash id="importcmd"
terraform import aws_instance.imported i-0123456789abcdef
```

---

# 🧠 WHAT HAPPENS

Terraform:

* Links existing AWS EC2
* Adds it to state file
* Now Terraform manages it

---

# 🧠 INTERVIEW EXPLANATION

> “terraform import is used to bring existing infrastructure into Terraform state so it can be managed using IaC.”

---

# 🔥 REAL PRODUCTION SCENARIOS

You use state commands when:

✔ Manual AWS changes done
✔ Resource moved between modules
✔ Accidentally deleted from state
✔ Import legacy infrastructure
✔ Debugging drift issues

---

# 🧯 COMMON ERRORS & FIXES

---

## ❌ Error 1: Resource not found in state

### Fix:

* Ensure correct resource name

---

## ❌ Error 2: Import failed

### Fix:

* Check correct instance ID
* Check permissions

---

## ❌ Error 3: State mismatch

### Fix:

* Run terraform refresh

---

# 💬 INTERVIEW QUESTIONS FROM TASK 10

---

## ❓ What is Terraform state?

> “It is a file that maps real infrastructure with Terraform configuration.”

---

## ❓ Difference between state rm and destroy?

| state rm                | destroy                   |
| ----------------------- | ------------------------- |
| Removes from state only | Deletes resource from AWS |

---

## ❓ Why is state important?

* Tracks infrastructure
* Enables planning
* Prevents duplication

---

## ❓ What is terraform import?

> “It is used to bring existing infrastructure under Terraform management.”

---

# 🔥 REAL PROJECT EXPLANATION

> “In my project, we used Terraform state commands to troubleshoot drift issues, remove unmanaged resources from state, and import existing AWS infrastructure into Terraform for centralized management.”

---

# 🧠 WHAT YOU LEARNED IN TASK 10

✔ State inspection
✔ State debugging
✔ Resource removal
✔ Import existing infra
✔ Production troubleshooting
✔ Critical DevOps skills

---

# 🚀 NEXT STEP (VERY IMPORTANT)

Now you are entering **advanced Terraform lifecycle control**

Next:

# 🟣 TASK 11 — Terraform Import + Drift Handling (Production Scenarios)

You will learn:

* Real drift detection
* Manual AWS changes handling
* Fixing production mismatches
* Senior DevOps troubleshooting scenarios

---


and we will move into **real production incident-level Terraform problem solving used in top companies**.
