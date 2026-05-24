# 🟣 TASK 14 — Terraform Data Sources (Real Cloud Integration)

## 🎯 Goal of This Task

By the end of this task, you will:

* Understand Terraform data sources
* Fetch existing AWS resources dynamically
* Remove hardcoded AMI IDs (VERY IMPORTANT)
* Build production-ready dynamic infrastructure
* Explain real-time cloud discovery in interviews

---

# 🧠 Real-Time Scenario (Interview Context)

> “In our project, we avoided hardcoding AMI IDs and instead used Terraform data sources to fetch the latest Amazon Linux AMI dynamically from AWS. This ensured our infrastructure always used updated and secure images.”

---

# ⚠️ WHY DATA SOURCES ARE IMPORTANT

Without data sources:

* Hardcoded AMIs ❌ (breaks in different regions)
* Manual updates required ❌
* Not scalable ❌

With data sources:

* Dynamic cloud lookup ✅
* Always latest config ✅
* Production-safe infrastructure ✅

---

# 🏗️ ARCHITECTURE FLOW

```text id="datasourceflow"
Terraform Code
      ↓
Data Source Query (AWS API)
      ↓
Fetch Latest AMI / Resource
      ↓
Use in EC2 Provisioning
```

---

# 🧩 STEP 1 — Create Project

```bash id="ds1"
mkdir terraform-data-sources
cd terraform-data-sources
```

---

# ⚙️ STEP 2 — main.tf (Dynamic AMI Example)

```hcl id="dscode"
provider "aws" {
  region = "ap-south-1"
}

# ---------------------------
# DATA SOURCE (LATEST AMI)
# ---------------------------
data "aws_ami" "latest_amazon_linux" {
  most_recent = true
  owners      = ["amazon"]

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
}

# ---------------------------
# EC2 INSTANCE
# ---------------------------
resource "aws_instance" "app" {
  ami           = data.aws_ami.latest_amazon_linux.id
  instance_type = "t2.micro"

  tags = {
    Name = "Terraform-Dynamic-AMI"
  }
}
```

---

# 🧠 EXPLANATION (VERY IMPORTANT)

---

## 🔹 data "aws_ami"

This is a **data source**, not a resource.

👉 It fetches existing AWS information.

---

## 🔹 most_recent = true

Ensures latest AMI is selected.

---

## 🔹 owners = ["amazon"]

Filters only official Amazon images.

---

## 🔹 filters

Narrow down AMI search:

* name pattern
* virtualization type

---

## 🔹 usage in EC2

```hcl id="amiuse"
ami = data.aws_ami.latest_amazon_linux.id
```

👉 This replaces hardcoded AMI IDs.

---

# 🚀 STEP 3 — Terraform Commands

```bash id="dsinit"
terraform init
```

```bash id="dsplan"
terraform plan
```

```bash id="dsapply"
terraform apply
```

Type:

```text id="yesds"
yes
```

---

# 🎉 RESULT

Terraform will:

✔ Query AWS for latest AMI
✔ Fetch image dynamically
✔ Launch EC2 with latest OS

---

# 🧠 INTERVIEW EXPLANATION (VERY IMPORTANT)

> “Terraform data sources allow us to fetch existing information from cloud providers dynamically. In my project, we used AWS AMI data source to fetch the latest Amazon Linux image instead of hardcoding AMI IDs, making the infrastructure more flexible and production-ready.”

---

# 🔥 REAL PROJECT USE CASES

✔ Fetch latest AMI automatically
✔ Get existing VPC IDs
✔ Fetch subnet information
✔ Retrieve security groups
✔ Query existing load balancers

---

# 🧯 COMMON ERRORS & FIXES

---

## ❌ Error 1: AMI not found

### Fix:

* Check region mismatch
* Adjust filter pattern

---

## ❌ Error 2: Invalid owner

### Fix:

* Use ["amazon"] or correct account ID

---

## ❌ Error 3: No AMI returned

### Fix:

* Relax filter conditions

---

# 💬 INTERVIEW QUESTIONS FROM TASK 14

---

## ❓ What are Terraform data sources?

> “Data sources allow Terraform to fetch existing infrastructure information from cloud providers.”

---

## ❓ Difference between resource and data source?

| Resource             | Data Source          |
| -------------------- | -------------------- |
| Creates infra        | Reads existing infra |
| Managed by Terraform | Read-only            |

---

## ❓ Why use data sources?

* Avoid hardcoding
* Dynamic infrastructure
* Cloud-native design

---

## ❓ Real use case?

> “Fetching latest AMI dynamically instead of hardcoding it.”

---

# 🔥 REAL PROJECT EXPLANATION

> “In my project, we used Terraform data sources to fetch latest AMI IDs and existing VPC configurations dynamically, ensuring infrastructure always remained up to date with cloud environment changes.”

---

# 🧠 WHAT YOU LEARNED IN TASK 14

✔ Data sources concept
✔ AWS AMI lookup
✔ Dynamic infrastructure design
✔ Cloud-native Terraform usage
✔ Eliminating hardcoding
✔ Production-ready patterns

---

# 🚀 NEXT STEP (VERY IMPORTANT)

Now you are moving into **advanced production DevOps automation level**

Next:

# 🟣 TASK 15 — Terraform Lifecycle Rules (Create, Prevent Destroy, Ignore Changes)

You will learn:

* Prevent accidental deletion
* Ignore specific changes
* Control resource lifecycle
* Production safety mechanisms

---

Just say:

👉 **TASK 15**

and we will move into **real enterprise-grade Terraform safety controls used in production systems**.
