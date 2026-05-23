
# 🟣 TASK 9 — Terraform Workspaces (Advanced Environment Management)

## 🎯 Goal of This Task

By the end of this task, you will:

* Understand Terraform workspaces
* Create and switch between environments dynamically
* Compare workspaces vs tfvars (very important for interviews)
* Manage isolated state per environment
* Use one codebase for multiple environments

---

# 🧠 Real-Time Scenario (Interview Context)

> “In some of our projects, we used Terraform workspaces to manage multiple environments like dev, QA, and production using a single codebase while maintaining separate state files for each environment.”

---

# 🏗️ WHAT ARE TERRAFORM WORKSPACES?

A workspace is:

> A way to create multiple isolated state files using the same Terraform configuration.

---

# 📊 WORKSPACES ARCHITECTURE

```text id="wsflow"
Single Terraform Codebase
        ↓
┌──────────┬──────────┬────────────┐
↓          ↓          ↓
dev      qa        prod
state1   state2    state3
```

Each workspace has its own state file.

---

# ⚖️ WORKSPACES vs TFVARS (VERY IMPORTANT INTERVIEW QUESTION)

| Feature          | Workspaces          | tfvars                   |
| ---------------- | ------------------- | ------------------------ |
| State separation | Automatic           | Manual                   |
| Code reuse       | Yes                 | Yes                      |
| Complexity       | Medium              | Simple                   |
| Best for         | Same infra diff env | Different config per env |
| Industry usage   | Medium              | High                     |

---

# 🧩 STEP 1 — Create Project

```bash id="ws1"
mkdir terraform-workspaces
cd terraform-workspaces
```

---

# 📄 STEP 2 — Create main.tf

```bash id="wsmain"
vim main.tf
```

---

## Terraform Code (Workspace-based)

```hcl id="wscode"
provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "app" {
  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"

  tags = {
    Name        = "Terraform-${terraform.workspace}-instance"
    Environment = terraform.workspace
  }
}
```

---

# 🧠 KEY CONCEPT

## 🔹 terraform.workspace

This gives current workspace name dynamically:

* dev
* qa
* prod

---

# 🚀 STEP 3 — Initialize Terraform

```bash id="wsinit"
terraform init
```

---

# 📊 STEP 4 — Check Current Workspace

```bash id="wslist"
terraform workspace list
```

Expected:

```text id="defaultws"
* default
```

---

# ➕ STEP 5 — Create Workspaces

## Dev

```bash id="wsdev"
terraform workspace new dev
```

## QA

```bash id="wsqa"
terraform workspace new qa
```

## Prod

```bash id="wsprod"
terraform workspace new prod
```

---

# 🔄 STEP 6 — Switch Workspace

```bash id="switchws"
terraform workspace select dev
```

---

# 📊 STEP 7 — Verify Workspace

```bash id="showws"
terraform workspace show
```

Expected:

```text id="devws"
dev
```

---

# 🚀 STEP 8 — Deploy Infrastructure

```bash id="applyws"
terraform apply
```

Type:

```text id="yesws"
yes
```

---

# 🎉 RESULT

Terraform will create:

* EC2 instance per workspace
* Separate state file for each environment
* Different tagging per environment

---

# 🧠 HOW STATE IS STORED

```text id="statews"
terraform.tfstate.d/
 ├── dev/
 ├── qa/
 └── prod/
```

Each workspace has isolated state.

---

# 🧠 INTERVIEW EXPLANATION (VERY IMPORTANT)

> “Terraform workspaces allow us to manage multiple environments using a single codebase. Each workspace maintains its own state file, enabling isolated infrastructure deployments for dev, QA, and production environments.”

---

# 🔥 REAL PROJECT USE CASE

> “We used Terraform workspaces in smaller projects to manage multiple environments efficiently without duplicating code. Each workspace represented a different environment with its own state file.”

---

# ⚠️ LIMITATION OF WORKSPACES (VERY IMPORTANT)

You MUST mention this in interviews:

❌ Not good for large enterprise setups
❌ Hard to manage complex differences
❌ Not suitable for strict production separation

---

# 🧠 WHEN TO USE WORKSPACES

✔ Small projects
✔ PoC environments
✔ Simple infra separation
✔ Dev/test environments

---

# 🧠 WHEN NOT TO USE

❌ Enterprise production systems
❌ Multi-account AWS setups
❌ Highly complex architectures

---

# 💬 INTERVIEW QUESTIONS FROM TASK 9

---

## ❓ What are Terraform workspaces?

> “Workspaces allow multiple isolated state files using the same configuration.”

---

## ❓ Difference between workspace and tfvars?

* Workspaces → state separation
* tfvars → configuration separation

---

## ❓ Where are workspace states stored?

> “Inside terraform.tfstate.d directory.”

---

## ❓ Is workspace good for production?

> “Not always. For enterprise setups, separate state backends or accounts are preferred.”

---

# 🔥 REAL-TIME INTERVIEW ANSWER

> “In my project, we used Terraform workspaces for small-scale environments to manage dev, QA, and production using a single codebase. Each workspace maintained its own state file, ensuring isolation between environments.”

---

# 🧠 WHAT YOU LEARNED IN TASK 9

✔ Terraform workspaces
✔ Dynamic environment management
✔ State isolation concept
✔ Workspace vs tfvars comparison
✔ Real production usage understanding
✔ Interview-ready explanation

---

# 🚀 NEXT STEP (VERY IMPORTANT)

Now you are entering **state management mastery level**

Next:

# 🟣 TASK 10 — Terraform State Commands (Interview Critical Topic)

You will learn:

* terraform state list
* terraform state show
* terraform state rm
* terraform import
* real production troubleshooting

---

Just say:

👉 **TASK 10**

and we will move into **real production Terraform state debugging like senior DevOps engineers**.
